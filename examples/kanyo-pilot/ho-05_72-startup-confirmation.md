# HO-05.72: Startup Confirmation & Stream Outage Handling

**Date:** 2026-01-11
**Commit:** 55e0b0f

## Problem

When the kanyo container restarts (e.g., Docker deployment, server reboot), if a falcon is already present on camera:

1. The system immediately assumed `ROOSTING` state and sent a telegram notification
2. This caused **false positive arrival notifications** every time the container restarted
3. Additionally, brief stream outages could corrupt recordings or leave the state machine confused

## Solution

### 1. PENDING_STARTUP State

Added a new state `PENDING_STARTUP` that requires confirmation before transitioning to `ROOSTING`:

```
STARTUP DETECTED → PENDING_STARTUP → (confirmation window) → ROOSTING
                                   ↘ (not confirmed) → ABSENT
```

- Uses the same confirmation ratio/window as arrival confirmation (default 10s, 30% ratio)
- Recordings start immediately (so we don't miss footage)
- Files stay as `.tmp` until confirmed
- Telegram notification **only sent after confirmation** AND only if `notify_on_startup=true`

### 2. Freeze Frame Handling

For brief stream outages (<5 seconds):

- `visit_recorder.write_frame()` now accepts `None` frames
- Uses the last good frame as a "freeze frame" to fill gaps
- Maintains video continuity without corruption

### 3. Extended Outage Recovery

For stream outages >5 seconds:

- Recording stops automatically (prevents corrupted video)
- State machine resets to `ABSENT`
- All pending confirmations cancelled
- When stream resumes and falcon detected, goes through normal confirmation flow

## Files Changed

| File | Changes |
|------|---------|
| `event_types.py` | Added `STARTUP_CONFIRMED` event, `PENDING_STARTUP` state |
| `falcon_state.py` | Added `set_pending_startup()`, `confirm_startup_presence()`, updated `initialize_state()` to return state |
| `buffer_monitor.py` | Startup confirmation tracking, stream outage handling, `_confirm_startup_presence()`, `_cancel_startup_presence()`, `_reset_pending_states()` |
| `visit_recorder.py` | Freeze frame handling, `stream_outage_exceeded` property |
| `clip_service.py` | Added `get_stream_stats()` for admin sidebar |
| `detail.html` | Stats sidebar showing arrivals/departures/visits/clips |

## State Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                      INITIALIZATION                              │
│                           │                                      │
│              falcon detected?                                    │
│                    │                                             │
│         ┌─────────┴─────────┐                                   │
│         │                   │                                   │
│         ▼                   ▼                                   │
│   PENDING_STARTUP        ABSENT ◄──────────────────┐           │
│         │                   │                       │           │
│    (confirm?)          detection                    │           │
│    │       │                │                       │           │
│  ┌─┘       └─┐              ▼                       │           │
│  ▼           ▼         VISITING ───(timeout)───────┤           │
│ROOSTING    ABSENT          │                       │           │
│                       (long stay)                  │           │
│                            │                       │           │
│                            ▼                       │           │
│                        ROOSTING ───(timeout)───────┘           │
└─────────────────────────────────────────────────────────────────┘
```

## Admin UI Enhancement

Added stats sidebar showing:
- 🟢 Arrivals count (last 24h)
- 🔴 Departures count (last 24h)
- 🔵 Visits count (last 24h)
- Total clips
- Last event type and time

## Testing

- 158 tests pass
- Updated `test_falcon_state.py` for new PENDING_STARTUP behavior
- Added test for confirmation flow

## Configuration

No new config options required - uses existing:
- `arrival_confirmation_seconds` (default: 10)
- `arrival_confirmation_ratio` (default: 0.3)
- `notify_on_startup` (default: false)
