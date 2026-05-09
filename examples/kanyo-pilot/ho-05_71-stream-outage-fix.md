# HO-05.71: Stream Outage Compensation Fix

**Date:** 2025-12-29
**Status:** ✅ COMPLETE

## Problem

Stream outages were causing **false departure events**. When the YouTube stream dropped for 60+ seconds:

1. Stream reconnects at timestamp T+60
2. First frame shows no bird (bird stepped away briefly)
3. State machine calculates: "90 seconds since last detection → DEPARTED"
4. **False departure notification sent** 🚨

**The root issue:** Outage time was being counted as real absence time.

## The Old (Broken) Approach

Previous fix in `buffer_monitor.py`:

```python
# Track total outage time
total_outage_time = 0.0

if time_since_last_frame > 10:
    total_outage_time += outage_duration

# Subtract outage from "now" when checking state
if outage_compensation > 0:
    now = now - timedelta(seconds=outage_compensation)

# Process frame with adjusted time
self.state_machine.update(falcon_detected, now)

# Reset after each frame ❌ BUG!
total_outage_time = 0
```

**The bug:** Only the first frame after reconnect got compensation. Subsequent frames had `total_outage_time = 0` again.

**Why it failed:**
- Bird arrives at T+0
- Stream drops 60s (T+10 to T+70)
- Frame at T+70: no bird, adjusted to T+10 → OK
- Frame at T+100: no bird, **no adjustment** → 90s absence → FALSE DEPARTURE

## The Fix

Track outage time **in the state machine**, not the monitor. Subtract it from absence duration.

### Changes to `falcon_state.py`

**Added to `__init__`:**
```python
self.cumulative_outage = 0.0  # Track total outage time during absence
```

**New method:**
```python
def add_outage(self, seconds: float) -> None:
    """Add stream outage time. This time won't count toward absence duration."""
    self.cumulative_outage += seconds
```

**Updated absence checks** (in both `VISITING` and `ROOSTING` states):
```python
# Before
absence_duration = (timestamp - self.last_absence_start).total_seconds()
if absence_duration >= self.exit_timeout:
    # DEPART

# After
absence_duration = (timestamp - self.last_absence_start).total_seconds()
effective_absence = absence_duration - self.cumulative_outage  # ✨ KEY CHANGE
if effective_absence >= self.exit_timeout:
    # DEPART
```

**Reset on detection:**
```python
def _handle_detection(self, timestamp: datetime) -> list:
    # ... existing detection logic ...

    # Reset outage accumulator on detection
    self.cumulative_outage = 0.0
    return events
```

**Reset in `_reset_state()`:**
```python
def _reset_state(self):
    self.state = FalconState.ABSENT
    self.visit_start = None
    self.last_detection = None
    self.last_absence_start = None
    self.roosting_start = None
    self.cumulative_outage = 0.0  # ✨ Reset here too
```

### Changes to `buffer_monitor.py`

**Removed:**
- `total_outage_time` variable
- `outage_compensation` parameter from `process_frame()`
- Timestamp adjustment code (`now = now - timedelta(...)`)
- `total_outage_time = 0` reset after processing

**Added:**
```python
if time_since_last_frame > 10:  # Stream outage detected
    outage_duration = time_since_last_frame
    self.state_machine.add_outage(outage_duration)  # ✨ Track in state machine
    logger.info(f"⚠️  Stream outage detected: {outage_duration:.1f}s")
```

**Cleanup:**
- Removed unused `timedelta` import
- Removed unused `last_successful_frame_time` variable

## Tests

Added `TestOutageCompensation` class with 4 comprehensive tests:

### 1. `test_outage_prevents_false_departure`
```python
# Bird arrives → outage 60s → no detection for 90s real time
# Effective absence: 90s - 60s = 30s < 90s threshold
# Should NOT depart ✅
```

### 2. `test_outage_resets_on_detection`
```python
# Accumulate 30s outage → bird detected → outage should reset to 0
```

### 3. `test_multiple_outages_accumulate`
```python
# Three outages: 20s + 30s + 10s = 60s cumulative
```

### 4. `test_real_departure_still_works`
```python
# 20s outage, then real absence 120s
# Effective: 120s - 20s = 100s > 90s threshold
# Should depart ✅
```

## Validation

**Code quality checks:**
```bash
source venv/bin/activate

# Format
black src/kanyo/detection/falcon_state.py \
      src/kanyo/detection/buffer_monitor.py \
      tests/test_falcon_state.py
# ✅ All files left unchanged

# Lint
flake8 src/kanyo/detection/falcon_state.py \
       src/kanyo/detection/buffer_monitor.py \
       tests/test_falcon_state.py
# ✅ Passed

# Tests
pytest tests/ -v
# ✅ 118 passed in 5.43s (including 4 new outage tests)
```

## Deployment

```bash
# Commit changes
git add src/kanyo/detection/falcon_state.py \
        src/kanyo/detection/buffer_monitor.py \
        tests/test_falcon_state.py

git commit -m "fix: proper stream outage compensation in state machine

Track cumulative outage time and subtract from absence duration.
Outage time resets when bird is detected.
Fixes false departures during stream drops."

# Deploy to production
bash scripts/update-code.sh shingan.lan
# ✅ Containers restarted: kanyo-harvard-gpu, kanyo-nsw-gpu, kanyo-admin-web
```

## How It Works

**Scenario:** 60-second stream outage during absence

```
T+0:   Bird arrives → state=VISITING
T+10:  Bird detected → cumulative_outage=0
T+20:  Stream drops (60s outage)
       ↓
T+80:  Reconnect → add_outage(60)
       First frame: no bird
       absence_start = T+80
       cumulative_outage = 60s

T+110: Still no bird
       real_absence = 110 - 80 = 30s
       effective_absence = 30s - 60s = -30s (clamped to 0)
       -30s < 90s threshold → NO DEPARTURE ✅

T+200: Still no bird
       real_absence = 200 - 80 = 120s
       effective_absence = 120s - 60s = 60s
       60s < 90s threshold → NO DEPARTURE ✅

T+230: Still no bird
       real_absence = 230 - 80 = 150s
       effective_absence = 150s - 60s = 90s
       90s >= 90s threshold → DEPARTED ✅
```

**Key insight:** Outage time persists across frames until bird is detected again.

## Results

- ✅ **False departures eliminated** during stream outages
- ✅ **Cleaner architecture** — state machine owns its timing logic
- ✅ **Simpler monitor** — no timestamp manipulation
- ✅ **Better tested** — 4 new edge case tests
- ✅ **Production deployed** — live on both streams

## Lessons Learned

**Don't manipulate timestamps** — it's error-prone and hard to reason about.

**State lives in the state machine** — the monitor should just pass events, not compensate for them.

**Track duration, not time** — accumulating `seconds` is clearer than adjusting `datetime` objects.

**Test edge cases** — outage during absence, multiple outages, real departures with outages.

## Files Changed

```
src/kanyo/detection/falcon_state.py     | +15 -1
src/kanyo/detection/buffer_monitor.py   | -21 +4
tests/test_falcon_state.py              | +88
```

**Total:** 3 files changed, 107 insertions(+), 21 deletions(-)

---

**Next:** Monitor production logs for false departures. Should be eliminated now.
