# Ho 3: Real-Time Falcon Detection with Notifications

**Duration:** 2-3 hours
**Goal:** Connect to live falcon cam, detect arrivals in real-time, send notifications
**Deliverable:** Working script that monitors 24/7 and alerts you when falcon appears

---

## Why This Ho Matters

**Before Ho 3:** You can analyze pre-recorded video and find falcons after-the-fact

**After Ho 3:** You get notified THE MOMENT a falcon arrives at the nest

This is the transition from batch processing to real-time monitoring. You're building something that runs continuously and responds to events as they happen.

---

## Core Intentions

### 1. **Real-Time Stream Connection**

**Intention:** Continuously pull frames from YouTube live stream without downloading the entire thing

**What this means:**
- Not downloading 12-hour DVR window
- Not saving video to disk first
- Grabbing frames as they become available
- Handling stream disconnections/reconnections

**Why it's different from Ho 2:**
- Ho 2: Open file, process all frames, done
- Ho 3: Open stream, process forever, handle errors

---

### 2. **Continuous Detection Loop**

**Intention:** Run YOLOv8 detection on frames continuously without overwhelming the system

**What this means:**
- Process every Nth frame (not every frame - too slow)
- Keep detection running indefinitely
- Don't accumulate memory (process and discard frames)
- Balance detection frequency vs CPU usage

**Key question to answer:** How often do you need to check? Once per second? Twice? Five times?

---

### 3. **Event Detection with State**

**Intention:** Track whether falcon is "present" or "absent" and detect state changes

**What this means:**
- Maintain state: "Is falcon currently present?"
- Detect transitions: absent → present (ARRIVAL), present → absent (DEPARTURE)
- Use debouncing from Ho 2 (don't declare exit until N seconds of no detection)

**This is a state machine:**
```
ABSENT ──[detect bird]──> PRESENT
   ↑                          │
   └───[no bird for 30s]──────┘
```

---

### 4. **Notification with Cooldown**

**Intention:** Alert you when falcon arrives, but don't spam you if it keeps coming and going

**What this means:**
- Send notification on ARRIVAL event
- Don't send another notification for 1 hour (cooldown period)
- Use ntfy.sh (simple HTTP POST, no auth needed)
- Include timestamp and maybe confidence in notification

**Why cooldown matters:** Falcon might leave for 5 minutes and come back. You don't want 20 notifications per day.

---

### 5. **Graceful Operation**

**Intention:** Run for hours/days without crashing or leaking resources

**What this means:**
- Handle stream disconnections (retry automatically)
- Handle detection failures (log and continue)
- Handle keyboard interrupt cleanly (Ctrl+C to stop)
- Log everything so you can debug issues

**This is about robustness:** System should recover from errors and keep running.

---

## Major Components & Concepts

### Component 1: Stream Capture (Already exists!)

**You already have:** `StreamCapture` class from Ho 2

**What it does:**
- Connects to YouTube stream
- Yields frames continuously
- Handles reconnection automatically

**What you'll use:**
```python
capture = StreamCapture(stream_url)
for frame in capture.frames(skip=30):
    # Process this frame
    pass
```

**Concept:** Infinite iterator pattern - frames keep coming until you stop the loop

---

### Component 2: Falcon Detector (Already exists!)

**You already have:** `FalconDetector` class from Ho 2

**What it does:**
- Runs YOLOv8 on a frame
- Returns list of detections
- Filters for animals (birds, cats, etc.)

**What you'll use:**
```python
detector = FalconDetector(confidence_threshold=0.5)
detections = detector.detect_birds(frame)
falcon_present = len(detections) > 0
```

**Concept:** Reusing existing detection logic - no need to rewrite

---

### Component 3: Visit Tracker (New - needs building)

**Intention:** Track current visit state and detect enter/exit events

**What it needs to do:**
- Remember: "Is falcon currently present?"
- Remember: "When did we last see falcon?"
- Decide: "Has falcon just arrived?" (absent → present)
- Decide: "Has falcon just left?" (present → absent for 30+ seconds)

**State to maintain:**
- `current_visit: FalconVisit | None` - Active visit or None
- `last_detection_time: datetime | None` - When we last saw falcon

**Concept:** State machine with timeout - exit only declared after debounce period

---

### Component 4: Notification Sender (New - needs building)

**Intention:** Send push notification via ntfy.sh with cooldown

**What it needs to do:**
- POST to ntfy.sh when falcon enters
- Include title, message, timestamp
- Track last notification time
- Skip notification if < 1 hour since last one

**API pattern:**
```python
import requests

requests.post(
    "https://ntfy.sh/your-topic-name",
    data="🦅 Falcon arrived at 2:30 PM!",
    headers={"Title": "Falcon Alert"}
)
```

**State to maintain:**
- `last_notification_time: datetime | None`

**Concept:** Simple HTTP POST with rate limiting

---

### Component 5: Main Loop Orchestration (New - needs building)

**Intention:** Tie everything together in a continuous loop

**Pseudocode flow:**
```
1. Connect to stream
2. Load detector
3. Initialize visit tracker
4. FOR EACH FRAME from stream:
   a. Run detection
   b. Update visit state
   c. If ARRIVAL detected:
      - Send notification (if cooldown allows)
      - Log event
   d. If DEPARTURE detected:
      - Log event
   e. Sleep briefly to prevent CPU spin
5. Handle Ctrl+C gracefully
```

**Concept:** Main event loop pattern - orchestrate components in infinite loop

---

## Configuration Values to Add

```yaml
# Real-time monitoring
stream_url: "https://www.youtube.com/watch?v=glczTFRRAK4"
frame_skip: 30                # Process every 30th frame
visit_timeout: 30             # Seconds of no detection = visit ended

# Notifications
ntfy_topic: "kanyo-falcon"    # Your ntfy.sh topic name
notification_cooldown: 3600   # Seconds (1 hour)
notification_enabled: true    # Easy on/off switch
```

---

## Key Decisions to Make

### Decision 1: Frame Processing Rate

**Question:** How often should we check for falcons?

**Options:**
- Every frame (30 fps) = too slow, CPU overload
- Every 15th frame (2 fps) = good balance
- Every 30th frame (1 fps) = fast, might miss brief appearances

**Recommendation:** Start with 30 (1 fps), tune if needed

---

### Decision 2: Visit Timeout

**Question:** How long should falcon be gone before we declare "visit ended"?

**Options:**
- 10 seconds = too short, creates spam from brief occlusions
- 30 seconds = balanced (from Ho 2 testing)
- 60 seconds = might miss quick visits

**Recommendation:** Use 30 seconds (proven in Ho 2)

---

### Decision 3: Notification Content

**Question:** What should the notification say?

**Options:**
- Minimal: "Falcon detected"
- Detailed: "Falcon arrived at 2:30 PM (confidence: 87%)"
- Link: Include YouTube stream link

**Recommendation:** Title + timestamp + link to watch live

---

### Decision 4: Error Handling Strategy

**Question:** What happens when something breaks?

**Stream disconnects:**
- Retry automatically (StreamCapture handles this)
- Log reconnection attempts

**Detection fails:**
- Log error
- Skip frame and continue
- Don't crash entire system

**Keyboard interrupt:**
- Clean shutdown
- Save any ongoing visit
- Close stream gracefully

---

## Success Criteria

**At the end of Ho 3, you should be able to:**

✅ Run `python realtime_monitor.py` on your Mac
✅ See it connect to falcon cam
✅ Watch log messages as it processes frames
✅ Get a notification on your phone when falcon detected
✅ Not get spammed (cooldown works)
✅ Stop it cleanly with Ctrl+C
✅ Leave it running for an hour without crashes

**You should understand:**
- How infinite loops work for monitoring
- How state machines track presence/absence
- How to send HTTP notifications
- How to handle errors gracefully
- How frame skipping affects detection

---

## What You're NOT Doing (Save for Later)

❌ **Saving clips from live stream** - Ho 2 clips were from files, live stream clip extraction is complex (need buffer)
❌ **Docker deployment** - Ho 4
❌ **Running on bird box** - Ho 4
❌ **Web interface** - Future Ho
❌ **Historical archive** - Future Ho

---

## Testing Strategy

**Phase 1: Sanity check (5 minutes)**
- Connect to stream
- Print "Frame received" for each frame
- Verify it doesn't crash

**Phase 2: Detection test (10 minutes)**
- Add detection
- Print when bird detected
- Verify confidence threshold works

**Phase 3: State machine test (15 minutes)**
- Add visit tracking
- Print ENTER/EXIT events
- Verify debouncing works

**Phase 4: Notification test (10 minutes)**
- Add ntfy.sh calls
- Send test notification
- Verify cooldown works

**Phase 5: Endurance test (60+ minutes)**
- Let it run for an hour
- Check logs for errors
- Verify no memory leaks

---

## File Structure

**New file:**
- `src/kanyo/detection/realtime_monitor.py` - Main monitoring loop

**Modified files:**
- `config.yaml` - Add notification settings
- `requirements.txt` - Add `requests` for ntfy.sh

**Script to run:**
- `scripts/start_monitoring.py` - Entry point with config loading

---

## Major Concepts You'll Learn

1. **Infinite loops for monitoring** - How to structure code that runs forever
2. **State machines** - Tracking presence/absence transitions
3. **Debouncing** - Ignoring brief gaps in detection
4. **Rate limiting** - Cooldown periods for notifications
5. **Error recovery** - Handling failures without crashing
6. **Graceful shutdown** - Cleaning up resources on exit

---

## Potential Gotchas

**Stream lag:** Live stream might be 10-30 seconds behind real-time
**Frame drops:** Network hiccups cause missed frames
**CPU usage:** Detection is intensive, need to balance frequency
**Memory leaks:** Must release frames after processing
**Notification failures:** ntfy.sh might be down, need fallback

---

## When You're Stuck

**Ask yourself:**
1. Is the stream connecting? (Check logs)
2. Are frames being received? (Add print statements)
3. Is detection running? (Log detection results)
4. Is state updating correctly? (Log state changes)
5. Are notifications sending? (Check ntfy.sh app)

**Debug incrementally:** Add one piece at a time, verify it works, then add next piece.

---

## End State

**You'll have a script that:**
- Monitors falcon cam 24/7
- Detects when falcon arrives
- Sends you a push notification
- Logs all events
- Handles errors gracefully
- Can run on your Mac while you're away

**You'll understand:**
- Real-time monitoring patterns
- State machines for event detection
- HTTP notification APIs
- Graceful error handling

---

# IMPLEMENTATION NOTES

*Everything below documents what we actually built, decisions made, pitfalls encountered, and lessons learned.*

---

## What We Built

### NotificationManager Class

Refactored all notification logic into a clean, encapsulated class in `src/kanyo/utils/notifications.py`:

```python
class NotificationManager:
    """Encapsulates all ntfy.sh notification logic with smart cooldown."""

    def __init__(self, config: dict):
        """Initialize from config, validate settings."""

    def send_arrival(self, timestamp: datetime, thumbnail_path: Path | None) -> bool:
        """Send arrival notification if not in cooldown. Returns True if sent."""

    def send_departure(self, timestamp: datetime, thumbnail_path: Path | None,
                       visit_duration_str: str) -> bool:
        """Always send departure notification. Updates cooldown timer."""

    def _send_ntfy(self, title: str, message: str, thumbnail_path: Path | None) -> bool:
        """Actually send HTTP POST to ntfy.sh with optional image attachment."""
```

**Key Design Decisions:**

1. **Clean API for realtime_monitor.py:**
   ```python
   self.notifications = NotificationManager(config)
   # Later:
   self.notifications.send_arrival(now, thumbnail_path)
   self.notifications.send_departure(exit_time, thumb_path, duration)
   ```

2. **Internal state management** - Cooldown tracking is internal, caller doesn't need to know

3. **Config validation on init** - Logs warnings if ntfy_topic missing, fails gracefully

---

## Smart Cooldown Logic

### The Problem We Solved

Original concept: "Simple cooldown after any notification"

But this creates a problem:
- Falcon arrives → notification sent → cooldown starts
- Falcon leaves 5 min later → departure suppressed by cooldown!
- User knows falcon arrived but never hears it left

### The Solution: Cooldown Starts After Departure

**Logic:**
1. **Arrival during cooldown** → SUPPRESS (we know bird is around)
2. **Departure** → ALWAYS SEND (critical info)
3. **Cooldown timer resets** after departure

This ensures:
- Each "visit" gets both arrival AND departure notifications
- Repeat visits within cooldown only get departure
- User always knows when falcon leaves

**Implementation:**
```python
def send_arrival(self, timestamp, thumbnail_path):
    if self._in_cooldown(timestamp):
        logger.debug("Suppressing arrival - still in cooldown")
        return False
    return self._send_ntfy("Falcon Arrived", ...)

def send_departure(self, timestamp, thumbnail_path, visit_duration_str):
    # Always send, always update cooldown
    self.last_departure_time = timestamp
    return self._send_ntfy("Falcon Departed", ...)
```

---

## ntfy.sh Integration

### API Details

**Basic text notification:**
```bash
curl -d "Message body" -H "Title: My Title" https://ntfy.sh/topic
```

**With image attachment:**
```bash
curl -T image.jpg -H "Title: My Title" -H "Filename: image.jpg" https://ntfy.sh/topic
```

### Python Implementation

```python
def _send_ntfy(self, title: str, message: str, thumbnail_path: Path | None) -> bool:
    url = f"https://ntfy.sh/{self.ntfy_topic}"
    headers = {"Title": title}

    if thumbnail_path and thumbnail_path.exists():
        headers["Filename"] = thumbnail_path.name
        with open(thumbnail_path, "rb") as f:
            data = f.read()
    else:
        data = message.encode("utf-8")

    response = requests.post(url, data=data, headers=headers)
    return response.ok
```

---

## Pitfall: Emoji Encoding Error

### The Problem

First version used emoji in titles:
```python
title = "🦅 Falcon Arrived"
```

But ntfy.sh sends headers with latin-1 encoding. Result:
```
UnicodeEncodeError: 'latin-1' codec can't encode character '\U0001f985' in position 0
```

### The Fix

Simple: Don't use emoji in HTTP headers. Plain text works fine:
```python
title = "Falcon Arrived"
title = "Falcon Departed"
```

**Lesson:** HTTP headers have encoding constraints. Keep them ASCII-safe.

---

## Frame Rate Decision

### Original Planning

Document said: "Start with frame_skip: 30 (1 fps), tune if needed"

### What We Actually Did

**Changed to frame_interval: 3 (10 fps)**

**Reasoning:**
- 1 fps = only 1 frame per second = 3.3% of frames analyzed
- Quick movements could be missed entirely
- 10 fps = much better coverage, still manageable CPU load

**Config setting:**
```yaml
frame_interval: 3  # Process every 3rd frame = ~10fps at 30fps source
```

### Removed detection_interval

Originally had `detection_interval: 60` in configs, but grep search confirmed it was never used anywhere. Removed to avoid confusion.

---

## Topic Separation

### Why Two Topics?

Running monitoring on multiple streams simultaneously. Need separate notification channels:

| Stream | Topic | Use Case |
|--------|-------|----------|
| NSW Falcons | `kanyo_falcon_cam_nsw` | Primary overnight monitoring |
| Harvard Falcons | `kanyo_falcon_cam_fas` | Testing, backup stream |

### Config Setup

**config.yaml (NSW - primary):**
```yaml
video_source: "https://www.youtube.com/watch?v=yv2RtoIMNzA"
ntfy_topic: "kanyo_falcon_cam_nsw"
```

**test_config_harvard.yaml:**
```yaml
video_source: "https://www.youtube.com/watch?v=glczTFRRAK4"
ntfy_topic: "kanyo_falcon_cam_fas"
```

---

## Testing Results

### Real Stream Testing

1. **Harvard stream** - Connected successfully, no falcon present at time of testing
2. **NSW stream** - FALCON DETECTED! Multiple visits observed, clips created successfully

### End-to-End Flow Verified

1. ✅ Stream connects via yt-dlp
2. ✅ Frames extracted at 10fps
3. ✅ YOLOv8 detection runs
4. ✅ Bird presence tracked (state machine)
5. ✅ Visit events logged to `clips/YYYY-MM-DD/events_YYYY-MM-DD.json`
6. ✅ Thumbnails saved (first frame of visit)
7. ✅ Video clips created (post-visit)
8. ✅ ntfy.sh notifications sent with images
9. ✅ Notifications received on phone

### Test Notification

```bash
curl -d "Test from Kanyō - if you see this, notifications are working" \
     https://ntfy.sh/kanyo_falcon_cam_nsw
```
Result: Received on phone within seconds ✅

---

## Final Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     realtime_monitor.py                         │
│                                                                 │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │  StreamCapture  │  │  FalconDetector │  │ NotificationMgr │ │
│  │  (yt-dlp/ffmpeg)│  │  (YOLOv8)       │  │ (ntfy.sh)       │ │
│  └────────┬────────┘  └────────┬────────┘  └────────┬────────┘ │
│           │                    │                    │           │
│           ▼                    ▼                    ▼           │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                     Main Event Loop                         ││
│  │                                                             ││
│  │  1. Capture frame from stream                               ││
│  │  2. Run detection (every frame_interval frames)             ││
│  │  3. Update visit state machine                              ││
│  │  4. On ENTERED: save thumbnail, send_arrival()              ││
│  │  5. On EXITED: create clip, send_departure()                ││
│  │  6. Log everything to EventStore                            ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘

Output Files:
  clips/2025-MM-DD/
    ├── events_2025-MM-DD.json      # All detection events
    ├── visit_123456_thumb.jpg      # Thumbnail per visit
    └── visit_123456.mp4            # Clip per visit
```

---

## Git Commits

1. **realtime: Save events to date-organized path and tidy data folder**
   - EventStore now saves to `clips/YYYY-MM-DD/events_YYYY-MM-DD.json`
   - Cleaned up data folder organization

2. **refactor: Extract notification logic to NotificationManager class**
   - New `src/kanyo/utils/notifications.py` module
   - Clean API: `send_arrival()`, `send_departure()`
   - Internal cooldown state management

3. **config: Fix frame rate and setup NSW/Harvard topics**
   - frame_interval: 30 → 3 (10fps detection)
   - Removed unused detection_interval
   - Separate topics: kanyo_falcon_cam_nsw, kanyo_falcon_cam_fas

---

## How to Run

### Subscribe to notifications first:
1. Install ntfy app on phone
2. Subscribe to `kanyo_falcon_cam_nsw` topic

### Start monitoring:
```bash
cd kanyo-contemplating-falcons-dev
PYTHONPATH=src python -m kanyo.detection.realtime_monitor
```

### Background monitoring (overnight):
```bash
nohup PYTHONPATH=src python -m kanyo.detection.realtime_monitor > logs/nsw_monitor.log 2>&1 &
```

### Using different config:
```bash
PYTHONPATH=src python -m kanyo.detection.realtime_monitor --config test_config_harvard.yaml
```

---

## Lessons Learned

1. **Cooldown timing matters** - Start cooldown after departure, not arrival
2. **HTTP headers need ASCII** - No emoji in ntfy titles (latin-1 encoding)
3. **Frame rate affects detection quality** - 10fps much better than 1fps
4. **Separate config per stream** - Different topics for different cameras
5. **Test with real streams early** - Synthetic tests can't catch all issues
6. **Refactor for clarity** - NotificationManager made code much cleaner
7. **Container formats matter for streaming** - MPEG-TS for segments, MP4 for final clips

---

## Troubleshooting: The "moov atom not found" Saga

### Problem: Empty Video Clips (261 bytes)

**Symptoms:**
- Notifications working perfectly with images
- Clips folder created but files only 261 bytes
- Error: `[mov,mp4,m4a,3gp,3g2,mj2] moov atom not found`
- Error: `Impossible to open '/tmp/kanyo-buffer/segment_YYYYMMDD_HHMMSS.mp4'`

**Timeline of failed fixes:**
1. ❌ **Attempt 1**: Added `-movflags +faststart+frag_keyframe+empty_moov`
   - Theory: Create fragmented MP4 with early moov atom
   - Result: Failed - VideoToolbox encoder doesn't support proper fragmentation

2. ❌ **Attempt 2**: Cleaned buffer and reordered ffmpeg command
   - Theory: Old segments causing issues, command order matters
   - Result: Failed - new segments still unreadable

3. ❌ **Attempt 3**: Added 2-second delay before extraction
   - Theory: Need time for buffer flush
   - Result: Failed - 2 seconds doesn't help when segment needs 10 minutes to finalize

### Root Cause Analysis

**The fundamental problem:** MP4 container format architecture is incompatible with reading incomplete files.

**MP4 structure requirements:**
- **moov atom** (movie metadata): Written at END of file during finalization
- **mdat atom** (media data): Contains video/audio frames
- File is unreadable until moov atom exists

**Our workflow creates a timing conflict:**
1. ffmpeg starts 10-minute segment at 11:33:35
2. Falcon arrives at 11:33:55 (20 seconds into segment)
3. Clip extraction needs timespan: 11:33:50 to 11:34:10
4. Extraction attempts to read segment at 11:34:10 (35 seconds elapsed)
5. **Problem**: Segment won't be finalized until 11:43:35 (10 minutes)
6. **Result**: No moov atom exists → file unreadable → clip extraction fails

**Why movflags didn't help:**
- `+faststart`: Only moves moov to beginning AFTER finalization
- `+frag_keyframe`: Creates fragmented MP4 with multiple moov atoms
- `+empty_moov`: Creates initial empty moov structure
- **Critical failure**: VideoToolbox hardware encoder doesn't properly support fragmented MP4. Even with these flags, it still finalizes moov at the end.

### The Solution: Switch to MPEG-TS Format

**MPEG-TS (MPEG Transport Stream) characteristics:**
1. **Stream-oriented, not file-oriented** - Designed for broadcast TV and live streaming
2. **No finalization needed** - Each 188-byte packet is self-contained
3. **Partial file reading** - Can read any portion while still being written
4. **PAT/PMT repeated** - Program tables repeated periodically for stream synchronization
5. **No moov atom** - No global metadata structure required

**Implementation changes in `live_tee.py`:**

```python
# Line 73: Segment filename pattern
segment_pattern = str(self.buffer_dir / "segment_%Y%m%d_%H%M%S.ts")  # Changed from .mp4

# Line 291: Regex pattern for parsing filenames
pattern = r"segment_(\d{8})_(\d{6})\.ts"  # Changed from \.mp4

# Lines 243, 261, 326: Glob patterns for finding segments
segments = sorted(self.buffer_dir.glob("segment_*.ts"))  # Changed from segment_*.mp4

# Removed movflags entirely (not applicable to TS format)
```

**Why this works:**

| Scenario | MP4 Behavior | MPEG-TS Behavior |
|----------|--------------|------------------|
| Read at 20 seconds | ❌ moov atom missing | ✅ Packets available immediately |
| Read at 5 minutes | ❌ Still not finalized | ✅ Read first 5 minutes fine |
| Segment rotation | ✅ Complete file | ✅ Complete file |
| Final clip output | ✅ Use MP4 for compatibility | ✅ Convert TS→MP4 for delivery |

**The complete flow now:**

```
1. ffmpeg records to: segment_20251218_113335.ts (MPEG-TS format)
   └─ Packets written immediately as encoded

2. Falcon arrives at 11:33:55
   └─ Clip scheduled for 15 seconds later

3. At 11:34:10, clip extraction runs:
   ├─ Time range: 11:33:50 to 11:34:10 (20 seconds)
   ├─ Source: segment_20251218_113335.ts (only 35 seconds recorded)
   ├─ ✅ CAN READ: TS format allows reading incomplete segments
   └─ ffmpeg concat extracts 20-second span from .ts segment

4. Output: falcon_113355_arrival.mp4 (9.2MB valid video)
   └─ Final clip converted to MP4 for compatibility
```

**Verification of fix:**
```
Before: -rw-r--r--  1 user  staff   261B Dec 18 11:35 falcon_113500_arrival.mp4
After:  -rw-r--r--  1 user  staff   9.2M Dec 18 11:41 falcon_114059_arrival.mp4
```

### Why This Is The Right Solution

**Professional video systems use the same approach:**
- **Live TV broadcasts** → MPEG-TS
- **HLS streaming** → MPEG-TS segments (.ts files)
- **DVR systems** → MPEG-TS for recording, MP4 for playback
- **YouTube live** → MPEG-TS internally, transcoded for delivery

**Key insight:** Use the right container format for the right stage:
- **Recording buffer** → MPEG-TS (stream-oriented, partial reads)
- **Final delivery** → MP4 (broad compatibility, smaller size)

### Implementation Details

**Buffer segments:**
```
/tmp/kanyo-buffer/
├── segment_20251218_114054.ts  # 10-minute rolling segments
├── segment_20251218_115054.ts  # MPEG-TS format
└── segment_20251218_120054.ts  # Can read while writing
```

**Extraction process:**
```python
# live_tee.py extract_clip() method
1. Find overlapping segments: glob("segment_*.ts")
2. Parse timestamps from filenames: segment_(\d{8})_(\d{6})\.ts
3. Build concat file with segments covering time range
4. ffmpeg concat protocol extracts span from TS segments
5. Output as MP4: falcon_HHMMSS_arrival.mp4
```

**Edge cases handled:**
- ✅ Clip spans multiple segments → concat handles multiple .ts files
- ✅ Reading incomplete segment → TS format allows it
- ✅ Segment rotation during extraction → segments locked while reading
- ✅ Final output compatibility → converted to MP4 for broad playback support

### Lessons Learned

1. **Container format = architectural choice** - Not just file extension
2. **MP4 is file-oriented** - Designed for complete, finalized files
3. **MPEG-TS is stream-oriented** - Designed for partial, live access
4. **Hardware encoder limitations** - VideoToolbox doesn't properly fragment MP4
5. **Right tool for the job** - TS for buffer, MP4 for delivery
# Remove old MP4 segments
rm /tmp/kanyo-buffer/*.mp4

# Check what's left
ls -lh /tmp/kanyo-buffer/

# Watch new segments being created
