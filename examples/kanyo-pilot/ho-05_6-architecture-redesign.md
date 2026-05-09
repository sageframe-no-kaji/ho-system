# Ho-05.5: Architecture Redesign — Tee to Buffer

**Date:** December 26, 2025
**Duration:** ~4 hours of deep thinking + implementation
**Outcome:** Replaced complex tee-based clip extraction with simpler buffer-based architecture

---

## The Problem

The original "tee" architecture used FFmpeg to split the incoming video stream:

```
YouTube Stream
      │
      ▼
┌─────────────┐
│   FFmpeg    │
│   (tee)     │
└──────┬──────┘
       │
   ┌───┴───┐
   ▼       ▼
Detection  Rolling Segments
(UDP)      (10-min .ts files)
```

**What went wrong:**

1. **Subprocess fragility** — FFmpeg running as a child process, prone to zombie processes, signal handling issues
2. **Timing synchronization** — Detection timestamps had to align with segment file timestamps. Off-by-one-segment errors meant clips could miss the actual event
3. **Segment boundary edge cases** — What if the falcon arrives 2 seconds before a segment boundary? Now the "15 seconds before arrival" clip needs to span TWO segment files
4. **Disk churn** — Constantly writing 10-minute segments, then deleting old ones, on a loop
5. **Complexity** — ~800 lines of code across `live_tee.py`, `clip_manager.py`, and `realtime_monitor.py` just to manage this

When I tried to run it live, I hit issues immediately:

- Detection was happening but clips weren't being created
- Timestamp misalignment between what the detector saw and what was in the segment files
- The system "worked" in isolation but fell apart when integrated

---

## The Thinking Process

### Why Not Just Fix the Tee?

I could have debugged the segment timing. But stepping back:

> "What are we actually trying to do?"

**Goal:** When a falcon arrives, save a video clip that includes footage from _before_ the detection happened.

The tee approach treats this as a **storage problem**: constantly write everything, then extract what you need later.

But there's another way to think about it: a **memory problem**. Keep recent frames in RAM, and only write to disk when something interesting happens.

### The Alternatives

| Approach                 | Pros                                    | Cons                                                      |
| ------------------------ | --------------------------------------- | --------------------------------------------------------- |
| **Fix the tee**          | Already written, just needs debugging   | Complex, subprocess management, segment edge cases        |
| **Frame buffer**         | Simple, no subprocesses, precise timing | RAM usage (~500MB for 60s buffer), must re-encode on save |
| **Keyframe-only buffer** | Lower RAM (~50MB)                       | Complex seeking, quality loss                             |
| **HLS sliding window**   | Industry standard                       | Still has segment boundaries, overkill                    |

### The Decision

**Buffer-based wins** because:

1. **Simplicity** — No subprocess management. Frames go in a ring buffer, come out when needed.
2. **Precision** — We have exact timestamps for every frame. No segment boundary guessing.
3. **Debuggability** — `len(buffer)` tells you exactly how much history you have. No parsing segment filenames.
4. **RAM is cheap** — 500MB for 60 seconds of 720p frames. Modern systems have 8-16GB+.

The re-encoding cost is acceptable because:

- We're only encoding when a visit ends (rare event)
- We'd have to re-encode anyway to make clips from segments
- Visit videos are valuable; worth the CPU time

---

## What We Built

### New Architecture

```
┌───────────────────────────────────────────────────────────────────────────┐
│                         BUFFER ARCHITECTURE                               │
├───────────────────────────────────────────────────────────────────────────┤
│                                                                           │
│   ┌─────────────────┐                                                     │
│   │  StreamCapture  │                                                     │
│   │  (capture.py)   │                                                     │
│   └────────┬────────┘                                                     │
│            │                                                              │
│            ▼  every frame                                                 │
│   ┌─────────────────┐     ┌──────────────────┐     ┌──────────────────┐  │
│   │   FrameBuffer   │     │  FalconDetector  │     │   StateMachine   │  │
│   │                 │     │                  │     │                  │  │
│   │  Ring buffer    │     │  YOLO inference  │ ──▶ │  ARRIVED/DEPARTED│  │
│   │  (60s frames)   │     │  on each frame   │     │  events          │  │
│   └────────┬────────┘     └──────────────────┘     └────────┬─────────┘  │
│            │                                                 │            │
│            │ on ARRIVED: get pre-event frames               │            │
│            ▼                                                 ▼            │
│   ┌─────────────────┐     ┌──────────────────┐     ┌──────────────────┐  │
│   │  VisitRecorder  │ ◀── │ BufferClipManager│ ◀── │     Events       │  │
│   │                 │     │                  │     │                  │  │
│   │  • Write visit  │     │  • Orchestrates  │     │  ARRIVED triggers│  │
│   │    video file   │     │    recording     │     │  start recording │  │
│   │  • Log events   │     │  • Extract clips │     │  DEPARTED triggers│ │
│   │  • JSON metadata│     │    on departure  │     │  stop + extract  │  │
│   └─────────────────┘     └──────────────────┘     └──────────────────┘  │
│                                                                           │
└───────────────────────────────────────────────────────────────────────────┘
```

### The Components

**FrameBuffer** (`frame_buffer.py`)

- Thread-safe ring buffer using `collections.deque`
- Stores `(timestamp, frame)` tuples
- Configurable duration (default 60s) and max frames
- `get_frames(start_time, end_time)` retrieves frames by time range

**VisitRecorder** (`visit_recorder.py`)

- Opens a VideoWriter when recording starts
- Writes frames with timestamps
- Logs detection events relative to video start time
- On stop: finalizes video, writes JSON metadata

**BufferClipManager** (`buffer_clip_manager.py`)

- Listens for state machine events
- On ARRIVED: pulls pre-event frames from buffer, starts VisitRecorder
- During visit: feeds frames to VisitRecorder
- On DEPARTED: stops recording, extracts arrival/departure clips using ffmpeg

**BufferMonitor** (`buffer_monitor.py`)

- Main entry point, combines all components
- The only file you run: `python -m kanyo.detection.buffer_monitor --config ...`

### Data Flow

```
1. Frame captured from stream
2. Frame pushed to FrameBuffer (ring buffer, 60s history)
3. Frame analyzed by YOLO detector
4. Detection result fed to StateMachine
5. If ARRIVED event:
   - Get last 15s of frames from buffer
   - Start VisitRecorder with those frames
6. While visiting:
   - Each frame written to visit video file
   - Detection events logged with relative timestamps
7. If DEPARTED event:
   - Stop VisitRecorder → finalizes visit_HHMMSS.mp4
   - Extract arrival clip (offset 0 to 45s)
   - Extract departure clip (last 45s of visit)
   - Write events JSON
```

### Output Structure

```
clips/2025-12-26/
├── visit_08-15-30.mp4           # Complete visit recording
├── arrival_08-15-30.mp4         # 15s before + 30s after arrival
├── departure_08-47-22.mp4       # 30s before + 15s after departure
└── events_2025-12-26.json       # All events with metadata
```

---

## What We Removed

| File                  | Lines    | Purpose                       |
| --------------------- | -------- | ----------------------------- |
| `live_tee.py`         | ~350     | FFmpeg subprocess management  |
| `clip_manager.py`     | ~250     | Segment-based clip extraction |
| `realtime_monitor.py` | ~200     | Tee-based monitoring loop     |
| `test_live_tee.py`    | ~200     | Tests for tee system          |
| **Total removed**     | **~800** |                               |

**Tests:** 155 → 124 (31 tee-specific tests removed)

---

## Git Safety Net

Before deleting anything, we created a tag:

```bash
git tag -a v0.5-with-tee -m "Pre-cleanup: tee architecture preserved"
git push origin --tags
```

If we ever need the tee code back:

```bash
git checkout v0.5-with-tee -- src/kanyo/utils/live_tee.py
```

---

## Lessons Learned

### 1. Step Back Before Debugging

When the tee wasn't working, the instinct was to debug it. Instead, asking "what are we actually trying to solve?" led to a fundamentally simpler approach.

### 2. RAM is Cheap, Complexity is Expensive

500MB of RAM for a 60-second buffer seems like a lot. But:

- Modern systems have 8-16GB+
- The alternative was 800 lines of subprocess management
- Debugging subprocess timing issues costs hours; RAM costs nothing

### 3. The "Boring" Solution is Usually Right

Frame buffer → write on event. That's it. No clever segment stitching, no UDP proxying, no timestamp reconciliation. Just... store frames, write when needed.

### 4. Tag Before You Delete

Even when you're confident the new approach is better, `git tag` costs nothing and saves everything. We can always recover the tee code if buffer-based has unforeseen issues.

---

## Performance Notes

**RAM usage (60s buffer @ 720p, 30fps):**

- ~1800 frames × ~300KB/frame = ~500MB
- Acceptable for dedicated monitoring system

**CPU (on visit end):**

- Re-encoding visit video: ~10-20s for a 30-minute visit
- Clip extraction via ffmpeg: ~2-3s per clip
- Happens rarely (only when falcon departs), so no performance concern

**Disk:**

- Only writes when visits happen
- No constant segment churn
- Much gentler on SSDs

---

## Live Test Results

Ran BufferMonitor for 2 minutes on Harvard falcon cam:

```
✓ Stream connected
✓ Frames buffering (1748 frames in 60s buffer)
✓ Detection running (saw false positive at 0.41 confidence - no falcon present)
✓ Visit recording created (5MB, 91s, 1473 frames)
✓ Clips extracted successfully
```

The false positive revealed our confidence threshold was too low (0.3). Bumped to 0.5.

---

## Summary

| Before                  | After                                     |
| ----------------------- | ----------------------------------------- |
| FFmpeg tee subprocess   | Direct OpenCV capture                     |
| Rolling 10-min segments | 60s in-memory ring buffer                 |
| Segment timestamp math  | Precise per-frame timestamps              |
| ~800 lines of tee code  | ~400 lines of buffer code                 |
| 155 tests               | 124 tests (simpler surface area)          |
| Hard to debug           | `print(len(buffer))` tells you everything |

**The buffer architecture is simpler, more reliable, and easier to reason about.**

---

## Files Changed

**Added:**

- `src/kanyo/utils/frame_buffer.py` — Ring buffer for frames
- `src/kanyo/utils/visit_recorder.py` — Records visits to video files
- `src/kanyo/detection/buffer_clip_manager.py` — Orchestrates buffer-based clips
- `src/kanyo/detection/buffer_monitor.py` — Main entry point
- `tests/test_frame_buffer.py` — 20 tests
- `tests/test_visit_recorder.py` — 20 tests

**Removed:**

- `src/kanyo/utils/live_tee.py`
- `src/kanyo/detection/clip_manager.py`
- `src/kanyo/detection/realtime_monitor.py`
- `tests/test_live_tee.py`

**Modified:**

- `src/kanyo/detection/capture.py` — Removed tee imports
- `docs/sensing-logic.md` — Updated for buffer architecture
- `configs/harvard/config.yaml` — Raised detection_confidence to 0.5

---

_This was a satisfying refactor. The new system does exactly what we need with half the code and none of the subprocess complexity._
