# Ho 2: Falcon Vision - COMPLETED

## What We Actually Built

**Duration:** ~4 sessions over multiple days
**Goal:** Build complete falcon detection + clip extraction pipeline
**Deliverable:** Working system that detects falcons, extracts clips, and generates thumbnails

---

## What I Actually Learned

### 1. YOLOv8 Detection Pipeline

**The Core Pattern:**
```python
from ultralytics import YOLO

model = YOLO("yolov8n.pt")  # nano = fast + small
results = model(frame, conf=0.5, verbose=False)

for result in results:
    for box in result.boxes:
        class_id = int(box.cls[0])
        confidence = float(box.conf[0])
        bbox = box.xyxy[0]  # x1, y1, x2, y2
```

**Key Insight:** YOLO's class 14 is "bird" - works for falcons. Confidence threshold 0.5 was good starting point, but needed tuning for the specific cam.

### 2. Visit Detection with Debounce

**Problem:** Raw detections are noisy - falcon detected 30x per second while present.

**Solution:** Debounce-based visit merging:
```yaml
visit_merge_timeout: 30  # seconds of no detection = visit ended
```

**Pattern:**
- First detection → start visit
- Continuous detections → same visit
- Gap > 30s → new visit
- No detection for 30s → visit ends

### 3. Clip Extraction with ffmpeg

**The Pattern:**
```python
subprocess.run([
    "ffmpeg", "-y", "-ss", str(start_time),
    "-i", str(video_path),
    "-t", str(duration),
    "-c:v", encoder,  # h264_videotoolbox on Mac
    "-crf", "23",     # quality (lower = better)
    "-r", "30",       # output fps
    output_path
])
```

**Key Config Values:**
```yaml
clip_entrance_before: 30   # seconds before entrance
clip_entrance_after: 60    # seconds after entrance
clip_exit_before: 60       # seconds before exit
clip_exit_after: 30        # seconds after exit
clip_merge_threshold: 180  # merge events within 3 min
```

### 4. Hardware Encoder Detection

**Why It Matters:** Hardware encoding is 10-20x faster than software.

**Auto-Detection Pattern:**
```python
def detect_hardware_encoder():
    encoders = [
        ("h264_videotoolbox", "Mac"),
        ("h264_nvenc", "NVIDIA"),
        ("h264_vaapi", "Intel/AMD Linux"),
    ]
    for encoder, name in encoders:
        if test_encoder_works(encoder):
            return encoder
    return "libx264"  # software fallback
```

**Platform Notes:**
- Mac: VideoToolbox built-in, just works
- Linux NVIDIA: needs nvidia-driver
- Linux Intel: needs intel-media-va-driver + vaapi

### 5. Thumbnail Extraction

**Pattern:** Extract frame at specific time:
```python
subprocess.run([
    "ffmpeg", "-y", "-ss", str(time_secs),
    "-i", str(video_path),
    "-frames:v", "1",
    "-q:v", "2",
    output_path
])
```

**Timing Logic:**
- Enter event: thumbnail 5s after entrance
- Exit event: thumbnail 10s before exit
- Merged event: TWO thumbnails (enter + exit)

### 6. Module Organization

**Clean separation learned:**
```
src/kanyo/
├── detection/      # Video analysis, YOLO, events
│   ├── capture.py      # Stream/video capture
│   ├── detect.py       # FalconDetector class
│   ├── events.py       # Event/Visit dataclasses
│   └── realtime_monitor.py
├── generation/     # Output creation
│   ├── clips.py        # ClipExtractor class
│   └── site_generator.py
└── utils/          # Shared utilities
    ├── config.py       # YAML config loading
    ├── encoder.py      # Hardware encoder detection
    ├── logger.py       # Logging setup
    └── notifications.py
```

**Key Insight:** Split encoder.py out of clips.py because it will be reused for continuous recording.

### 7. Testing Patterns

**Fixture Pattern:**
```python
@pytest.fixture
def config():
    return {
        "clips_dir": "clips",
        "clip_entrance_before": 30,
        # ... test config
    }

@pytest.fixture
def extractor(self, config, tmp_path):
    video_path = tmp_path / "test.mp4"
    video_path.touch()
    return ClipExtractor(config, video_path)
```

**Mock Pattern:**
```python
@patch("subprocess.run")
def test_ffmpeg_not_called_in_dry_run(self, mock_run):
    extractor.extract_clips(dry_run=True)
    mock_run.assert_not_called()
```

### 8. Linting Stack

**The Commands:**
```bash
black src/ tests/           # Code formatting
isort src/ tests/           # Import sorting
flake8 src/ tests/          # Linting
mypy src/                   # Type checking
pytest tests/ -v            # Testing
```

**pyproject.toml Config:**
```toml
[tool.black]
line-length = 100

[tool.isort]
profile = "black"

[tool.mypy]
python_version = "3.10"
mypy_path = "src"
explicit_package_bases = true
ignore_missing_imports = true
```

---

## Files Created/Modified

### New Files
- `src/kanyo/utils/encoder.py` - Hardware encoder detection
- `src/kanyo/generation/clips.py` - ClipExtractor class
- `tests/test_encoder.py` - Encoder tests (8 tests)
- `tests/test_clips.py` - Clip extraction tests (23 tests)
- `docs/hardware-encoding.md` - Hardware encoding setup guide

### Modified Files
- `scripts/analyze_video.py` - Added clip extraction integration
- `src/kanyo/detection/events.py` - Visit dataclass refinements
- `config.yaml` - Added clip/thumbnail config options
- `pyproject.toml` - Added mypy config

---

## Config Values That Matter

```yaml
# Detection
detection_confidence: 0.5
frame_interval: 30
visit_merge_timeout: 30

# Clips
clip_entrance_before: 30
clip_entrance_after: 60
clip_exit_before: 60
clip_exit_after: 30
clip_merge_threshold: 180
clip_compress: true
clip_crf: 23
clip_fps: 30
clip_hardware_encoding: true

# Thumbnails
thumbnail_entrance_offset: 5
thumbnail_exit_offset: -10
```

---

## Commands I Now Know

```bash
# Test encoder detection
python -m kanyo.utils.encoder

# Analyze video and extract clips
PYTHONPATH=src python scripts/analyze_video.py data/samples/falcon_full_test.mov

# Run all quality checks
black src/ tests/ && isort src/ tests/ && flake8 src/ tests/ && mypy src/

# Run tests
PYTHONPATH=src python -m pytest tests/ -v
```

---

## Test Coverage

- **55 tests total**
- **54% overall coverage**
- clips.py: 53% covered
- encoder.py: 56% covered
- events.py: 92% covered
- detect.py: 77% covered

---

## Key Commits

1. `6538504` - fix: filter detections by target_classes
2. `f7f367c` - feat: add visit_merge_timeout for debounce
3. `4b44312` - feat: add analyze_video.py script
4. `0b97539` - feat: add clip extraction with hardware encoding
5. `cf5f592` - refactor: extract encoder module, add tests

---

## What's Actually Working Now

✅ YOLOv8 detection on video files
✅ Debounce-based visit merging
✅ Clip extraction with configurable timing
✅ Hardware-accelerated encoding (VideoToolbox on Mac)
✅ Thumbnail generation (1 for simple, 2 for merged events)
✅ Full linting/typing/testing pipeline
✅ 55 passing tests

---

## Gotchas & Lessons

### YOLOv8 Coordinate Formats

**Gotcha:** Box accessors return different formats
```python
box.xywh    # center x, center y, width, height
box.xyxy    # x1, y1, x2, y2 (corner coordinates)
box.xyxyn   # normalized (0.0-1.0)
```

Use `.xyxy[0]` for absolute pixel coordinates in video. Don't assume xywh format.

### ffmpeg Seeking Performance

**Gotcha:** `-ss` flag placement matters enormously
```bash
# SLOW - decodes entire file up to timestamp
ffmpeg -i video.mp4 -ss 00:05:30 output.mp4

# FAST - seeks to timestamp in container (keyframes)
ffmpeg -ss 00:05:30 -i video.mp4 output.mp4
```

Always put `-ss` BEFORE `-i` for clip extraction. ~100x faster.

### visit_merge_timeout Tuning

**Gotcha:** Need to understand your bird's behavior
```yaml
visit_merge_timeout: 30  # Too low = multiple visits for one perch
visit_merge_timeout: 120 # Too high = merges separate visits
```

Observe actual falcon gaps - measure the time between legitimate separate visits and set timeout below that.

### Hardware Encoder Availability

**Gotcha:** Just because ffmpeg lists encoder doesn't mean it works
```python
# This can fail silently:
ffmpeg -c:v h264_vaapi ...  # Lists as available but device missing

# Must test actual encode:
ffmpeg -vaapi_device /dev/dri/renderD128 -c:v h264_vaapi ...
```

Always test encode in detection code before using in production.

---

## Performance

### Processing Speed

**14-minute test video:**
- **With frame_interval=30:** ~45 seconds total processing
  - That's 2x realtime (14 min video in 7 seconds of processing per minute)
- **With frame_interval=1 (every frame):** ~18 minutes (runs slower than video)

**Breakdown per 1-minute video:**
- YOLO inference: ~0.3 seconds
- Frame reading: ~0.1 seconds
- Event merging: negligible

### Clip Extraction Speed

**Per clip with hardware encoding (VideoToolbox Mac):**
- 90-second clip: ~5-10 seconds
- 180-second clip: ~10-15 seconds
- **Speedup: 10-20x vs software**

**Per clip WITHOUT hardware encoding (libx264):**
- 90-second clip: ~50-90 seconds
- 180-second clip: ~100-180 seconds
- Quality same, just slow

### Thumbnail Extraction

- Per thumbnail: ~0.5-1 second
- Fast because single frame extraction

### Memory Usage

- YOLOv8 nano model: ~200MB loaded
- Video frames in memory: ~50MB (1080p RGB)
- Total per process: ~300-400MB

---

## What's Next (Ho 3?)

- **Live stream monitoring** - Connect to YouTube live stream
- **Continuous recording** - 6-hour chunk mode using encoder module
- **Event notifications** - Email/Discord alerts
- **Site generation** - Static HTML gallery of clips/thumbnails

---

## Reflection

**What worked well:**
- Iterative development - got detection working, then clips, then thumbnails
- Splitting encoder.py early - clean reusable module
- Testing as I went - caught issues immediately

**What I'd do differently:**
- Start with mypy config earlier - fewer type fixes at end
- Write tests first for complex logic like clip merging

**Confidence Level:** 4/5 - Understand the pipeline well, some ffmpeg magic still feels like incantation

---

**Completed:** December 17, 2025
**Time Spent:** ~6-8 hours across sessions
**Test Count:** 55 passing
**Coverage:** 54%

