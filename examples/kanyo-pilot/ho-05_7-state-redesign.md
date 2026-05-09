You're right, it's a bit too stripped down. Here's a better balance:

```markdown
# HO-05.7: State Machine Redesign

**Date:** 2025-12-28
**Status:** ✅ COMPLETE

## Problem

The falcon detection system had a complex 4-state machine with 3 different timeout values:

```
ABSENT → VISITING → ROOSTING → ACTIVITY → ROOSTING → ABSENT
```

This caused:
1. **"moov atom not found" errors** — extracting clips from actively-recording files
2. **Confusing timeouts** — `exit_timeout` (5 min), `roosting_exit_timeout` (10 min), `activity_timeout` (3 min)
3. **200+ lines of ACTIVITY tracking code** — for a feature we don't actually need
4. **Difficult tuning** — 3 interdependent parameters

## Solution

Simplify to 3-state machine with single timeout:

```
ABSENT ──► VISITING ──► ROOSTING
   ▲           │            │
   └───────────┴────────────┘
          (exit_timeout: 90s)
```

**Key insight**: ROOSTING should only trigger a "settled in" notification. It shouldn't have different timeout behavior.

## What Changed

### Phase 1: State Machine Simplification

**Removed from code:**
- `ACTIVITY` state and `ACTIVITY_START`/`ACTIVITY_END` events
- `activity_timeout`, `roosting_exit_timeout`, `activity_notification` config options
- `activity_periods` tracking in state machine
- ~150 lines of ACTIVITY-related tests

**Added:**
- `TimezoneFormatter` — logs now use configured timezone, not UTC
- Simplified validation — only checks `roosting_threshold > exit_timeout`

### Phase 2: Buffer Monitor Cleanup

**Removed:**
- State change clip debounce logic (`schedule_state_change_clip`, `check_state_change_debounce`)
- `clip_state_change_before/after/cooldown` config options
- `TestStateChangeDebounce` test class (~90 lines)

**Extracted:**
- `ArrivalClipRecorder` to its own file for cleaner separation

### Phase 3: Departure Clip Fix

**The bug:** Departure clips showed empty nest instead of bird leaving.

**Root cause:** Clip extracted from end of recording file (`recording_duration - 75s`) instead of from last detection time.

**The fix:**
```python
# Before (WRONG)
start_offset = recording_duration - clip_duration  # End of file = empty nest

# After (CORRECT)
last_detection_offset = (visit_end - recording_start).total_seconds()
start_offset = last_detection_offset - clip_departure_before  # Actual departure
```

**Also added:** `recording_start` to visit metadata for accurate offset calculation.

### Phase 3b: Dead Code Removal

**Deleted files:**
- `realtime_monitor.py` (521 lines) — replaced by `buffer_monitor.py`
- `clip_manager.py` — replaced by `buffer_clip_manager.py`
- `live_tee.py` — no longer used

## New Configuration

```yaml
# Before (complex)
exit_timeout: 300              # 5 min during visit
roosting_exit_timeout: 600     # 10 min during roost
activity_timeout: 180          # 3 min triggers ACTIVITY
activity_notification: false

# After (simple)
exit_timeout: 90               # 90s for ALL states
roosting_threshold: 1800       # 30 min triggers notification only
```

## Results

| Metric | Before | After |
|--------|--------|-------|
| States | 4 | 3 |
| Timeout parameters | 3 | 1 |
| Lines of code | ~2500 | ~1560 |
| Tests | 114 passing | 114 passing |
| Coverage | 41% | 46% |

**Fixed:**
- ✅ No more "moov atom" errors
- ✅ Departure clips show bird leaving (not empty nest)
- ✅ Logs use configured timezone
- ✅ Simpler to understand and tune

## Commits

1. **Phase 1**: `refactor: simplify state machine - remove ACTIVITY state, unify exit_timeout`
2. **Phase 2**: `refactor: clean up buffer_monitor and extract ArrivalClipRecorder`
3. **Phase 3**: `fix: departure clip now extracts from last_detection offset`
4. **Phase 3b**: `chore: remove dead code from old tee architecture`

**Total: ~938 lines removed** 🎉
```
