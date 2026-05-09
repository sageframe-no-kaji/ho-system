# Ho 1: Git Good
## Establishing Proper Project Foundation

**Duration:** 2 hours
**Goal:** Create professional Python project structure with proper dependency management
**Deliverable:** Clean, well-organized kanyo repository ready for development

---

## Why This Ho Matters

Before writing detection code, we need a solid foundation. This ho sets up:
- Proper Python virtual environment (isolated dependencies)
- Project structure that scales
- Development tools (linting, formatting)
- Documentation framework
- Testing infrastructure

**Do this right now, save hours of pain later.**

---

## Prerequisites Check

From Ho 0.5, you should have:
- [ ] kanyo repo created and pushed to GitHub
- [ ] Basic README.md exists
- [ ] Comfortable with git add/commit/push
- [ ] GitHub Copilot working in VSCode
- [ ] Python 3.10+ installed

**If not:** Complete Ho 0.5 first, or ask for help.

---

## Part 1: Python Virtual Environment (20 minutes)

### Why Virtual Environments Matter

**Without venv:**
```
System Python + all projects = dependency hell
Project A needs opencv 4.5
Project B needs opencv 4.8
💥 Conflict!
```

**With venv:**
```
Each project isolated
kanyo has its own Python + packages
Other projects unaffected
```

### Create Virtual Environment

```bash
# Navigate to kanyo
cd ~/path/to/kanyo

# Create venv
python3 -m venv venv

# If you are using fish:
source .venv/bin/activate.fish

# Activate it
source .venv/bin/activate  # Mac/Linux
# OR
.venv\Scripts\activate  # Windows

# Verify (should show kanyo/.venv/bin/python)
which python3
```

**You should see `(.venv)` prefix in terminal** - this means it's active.

### Update .gitignore

**Add to `.gitignore`:**
```bash
# Python virtual environment
.venv/
env/
ENV/
```

**Commit this:**
```bash
git add .gitignore
git commit -m "Ho 1: Update gitignore for venv"
git push
```

### Always Activate Before Work

**Add this to your routine:**
```bash
# Every time you work on kanyo:
cd ~/path/to/kanyo

source .venv/bin/activate.fish #fish
#OR
source .venv/bin/activate  # Mac/Linux
# Now you're in the isolated environment
```

---

## Part 2: Project Structure (25 minutes)

### The Professional Layout

**Create this structure:**
```
kanyo/
├── README.md
├── .gitignore
├── requirements.txt          # Production dependencies
├── requirements-dev.txt      # Development dependencies
├── setup.py                  # Package configuration (optional)
├── .env.example             # Environment variables template
├── docs/
│   ├── architecture.md
│   ├── data-model.md
│   └── deployment.md
├── devlog/
│   ├── ho-00-overview.md
│   ├── ho-0.5-tool-mastery.md
│   └── ho-01-git-good.md (this file)
├── src/
│   └── kanyo/
│       ├── __init__.py
│       ├── detection/
│       │   ├── __init__.py
│       │   ├── capture.py      # Stream capture
│       │   ├── detect.py       # Falcon detection
│       │   └── events.py       # Event detection
│       ├── generation/
│       │   ├── __init__.py
│       │   └── site_generator.py
│       └── utils/
│           ├── __init__.py
│           ├── config.py
│           └── logger.py
├── tests/
│   ├── __init__.py
│   ├── test_detection.py
│   └── test_events.py
├── scripts/
│   ├── download_sample.sh    # Get test footage
│   └── run_detection.py      # Main entry point
├── data/
│   ├── .gitkeep
│   └── README.md
├── models/
│   ├── .gitkeep
│   └── README.md
└── site/
    ├── index.html
    ├── styles.css
    └── data/
        └── detections.json
```

### Create the Structure

**Use Copilot Chat (Cmd+I):**
```
@workspace /model:claude-sonnet-4.5

Create the following directory structure in my kanyo project:
- src/kanyo/detection/ with __init__.py, capture.py, detect.py, events.py
- src/kanyo/generation/ with __init__.py, site_generator.py
- src/kanyo/utils/ with __init__.py, config.py, logger.py
- tests/ with __init__.py, test_detection.py, test_events.py
- scripts/ directory
- data/ with .gitkeep and README.md
- models/ with .gitkeep and README.md
- site/data/ directory

Add brief docstrings to each __init__.py explaining the module's purpose.
```

**OR create manually:**
```bash
# From kanyo directory
mkdir -p src/kanyo/{detection,generation,utils}
mkdir -p tests scripts data models site/data
touch src/kanyo/__init__.py
touch src/kanyo/detection/{__init__.py,capture.py,detect.py,events.py}
touch src/kanyo/generation/{__init__.py,site_generator.py}
touch src/kanyo/utils/{__init__.py,config.py,logger.py}
touch tests/{__init__.py,test_detection.py,test_events.py}
touch data/.gitkeep models/.gitkeep
```

### Add Module Docstrings

**In `src/kanyo/__init__.py`:**
```python
"""
Kanyō (観鷹) - Contemplating Falcons

Automated detection and timeline generation for peregrine falcon live streams.
"""

__version__ = "0.1.0"
```

**In `src/kanyo/detection/__init__.py`:**
```python
"""
Detection module - Video capture and falcon detection using YOLOv8.
"""
```

**In `src/kanyo/generation/__init__.py`:**
```python
"""
Generation module - Static site generation from detection data.
"""
```

**In `src/kanyo/utils/__init__.py`:**
```python
"""
Utilities - Configuration, logging, and helper functions.
"""
```

### Create Data Directory READMEs

**In `data/README.md`:**
```markdown
# Data Directory

Stores temporary video files and processing artifacts.

**Contents:**
- Downloaded stream segments (*.mp4)
- Extracted frames (frames/)
- Thumbnails (thumbs/)

**Note:** This directory is ignored by git. Data is ephemeral.
```

**In `models/README.md`:**
```markdown
# Models Directory

Stores YOLOv8 model files.

**Expected files:**
- yolov8n.pt (nano model for testing)
- yolov8s.pt (small model for production)

**Note:** Model files are ignored by git due to size.
Download on first run.
```

### Commit the Structure

```bash
git add .
git status  # Review what you're adding
git commit -m "Ho 1: Create project structure"
git push
```

---

## Part 3: Dependency Management (20 minutes)

### Create requirements.txt

**Production dependencies** - what kanyo needs to run:

**Create `requirements.txt`:**
```
# Core detection
ultralytics>=8.0.0         # YOLOv8
opencv-python>=4.8.0       # Video processing
numpy>=1.24.0              # Array operations

# Stream capture
yt-dlp>=2023.10.0          # YouTube download

# Data handling
python-dateutil>=2.8.0     # Date parsing
pyyaml>=6.0                # Configuration files

# Web generation
jinja2>=3.1.0              # Template engine
markdown>=3.5.0            # Markdown processing
```

### Create requirements-dev.txt

**Development dependencies** - tools for development:

**Create `requirements-dev.txt`:**
```
# Include production requirements
-r requirements.txt

# Testing
pytest>=7.4.0
pytest-cov>=4.1.0
pytest-mock>=3.11.0

# Code quality
black>=23.0.0              # Code formatter
flake8>=6.1.0              # Linter
isort>=5.12.0              # Import sorting
mypy>=1.5.0                # Type checking

# Development tools
ipython>=8.15.0            # Better REPL
jupyter>=1.0.0             # Notebooks for experimentation
```

### Install Dependencies

```bash
# Make sure venv is activated!
# Should see (venv) in prompt

# Install dev dependencies (includes production)
pip install -r requirements-dev.txt

# This will take a few minutes...
```

**Verify installation:**
```bash
# Check key packages
python3 -c "import cv2; print(cv2.__version__)"
python3 -c "from ultralytics import YOLO; print('YOLOv8 ready')"
python3 -c "import yt_dlp; print('yt-dlp ready')"
```

### Pin Exact Versions (Optional but Recommended)

```bash
# Generate exact versions you have
pip freeze > requirements-lock.txt

# Commit this for reproducibility
git add requirements.txt requirements-dev.txt requirements-lock.txt
git commit -m "Ho 1: Add project dependencies"
git push
```

---

## Part 4: Configuration Management (20 minutes)

### Create Config System

**In `src/kanyo/utils/config.py`:**

**Use Copilot Chat:**
```
@workspace /model:claude-opus-4.5

Create a configuration management system in config.py that:
1. Loads from YAML file (config.yaml)
2. Supports environment variable overrides
3. Has sensible defaults
4. Validates required fields

Include config for:
- video_source: YouTube URL
- detection_confidence: float (0.0-1.0)
- detection_interval: seconds between checks
- output_dir: where to save results
- model_path: path to YOLOv8 model

Keep it simple - under 80 lines with comments.
```

**Review the generated code** - does it meet Tier 2 understanding?
- Can you modify a config value?
- Can you add a new config field?
- Is it overly complex?

### Create Default Config File

**Create `config.yaml`:**
```yaml
# Kanyō Configuration

# Video Source
video:
  source: "https://www.youtube.com/watch?v=glczTFRRAK4"  # Memorial Hall Peregrine Cam
  download_interval: 3600  # Check every hour (seconds)
  segment_duration: 7200   # Download 2 hours of stream

# Detection Settings
detection:
  model: "yolov8n.pt"      # Model file (n=nano, s=small, m=medium)
  confidence: 0.6          # Detection confidence threshold
  classes: [14, 15, 16]    # Bird classes in COCO (14=bird, 15=cat, 16=dog)
  frame_interval: 30       # Process every Nth frame (30 = ~1 per second)

# Event Detection
events:
  enter_threshold: 3       # Frames to confirm entrance
  exit_threshold: 5        # Frames to confirm exit
  movement_threshold: 100  # Pixel movement to trigger
  stasis_duration: 300     # Seconds of stillness before movement event

# Output Settings
output:
  data_dir: "./data"
  models_dir: "./models"
  thumbnails_dir: "./data/thumbs"
  site_dir: "./site"
  detections_file: "./site/data/detections.json"

# Logging
logging:
  level: "INFO"           # DEBUG, INFO, WARNING, ERROR
  format: "%(asctime)s - %(name)s - %(levelname)s - %(message)s"
```

### Create .env.example

**For sensitive/local config:**

**Create `.env.example`:**
```bash
# Environment Variables Template
# Copy to .env and fill in your values

# YouTube API (if using API instead of yt-dlp)
# YOUTUBE_API_KEY=your_key_here

# GitHub Actions secrets
# GITHUB_TOKEN=automatic

# Cloudflare (for deployment)
# CLOUDFLARE_API_TOKEN=your_token
# CLOUDFLARE_ACCOUNT_ID=your_account_id

# Local overrides
# KANYO_CONFIDENCE_THRESHOLD=0.7
# KANYO_OUTPUT_DIR=/custom/path
```

**Add to .gitignore:**
```bash
# Environment variables
.env
config.local.yaml
```

### Test Config Loading

**Create `scripts/test_config.py`:**
```python
"""Test configuration loading"""
from pathlib import Path
import sys

# Add src to path
sys.path.insert(0, str(Path(__file__).parent.parent / "src"))

from kanyo.utils.config import load_config

def main():
    config = load_config("config.yaml")

    print("Configuration loaded successfully!")
    print(f"Video source: {config['video']['source']}")
    print(f"Detection confidence: {config['detection']['confidence']}")
    print(f"Model: {config['detection']['model']}")
    print(f"Output directory: {config['output']['data_dir']}")

if __name__ == "__main__":
    main()
```

**Run it:**
```bash
python3 scripts/test_config.py
```

**Should print config values without errors.**

### Commit Configuration

```bash
git add config.yaml .env.example src/kanyo/utils/config.py scripts/test_config.py .gitignore
git commit -m "Ho 1: Add configuration system"
git push
```

---

## Part 5: Logging Setup (15 minutes)

### Create Logger Utility

**In `src/kanyo/utils/logger.py`:**

**Use Copilot Chat:**
```
@workspace /model:claude-sonnet-4.5

Create a logging utility that:
1. Sets up Python logging with config from config.yaml
2. Logs to both console and file
3. Includes timestamp, level, module name
4. Has convenience functions: get_logger(name)

Keep it simple - under 50 lines.
In addition to thos 50 lines ass an explanatory comment describing behavior at the top
```

### Test Logger

**Create `scripts/test_logger.py`:**
```python
"""Test logging setup"""
from pathlib import Path
import sys

sys.path.insert(0, str(Path(__file__).parent.parent / "src"))

from kanyo.utils.logger import get_logger

def main():
    logger = get_logger(__name__)

    logger.debug("Debug message")
    logger.info("Info message")
    logger.warning("Warning message")
    logger.error("Error message")

    print("\nLogger working! Check logs/kanyo.log")

if __name__ == "__main__":
    main()
```

**Run it:**
```bash
mkdir -p logs
python3 scripts/test_logger.py
cat logs/kanyo.log
```

**Should see timestamped log entries.**

### Add logs/ to .gitignore

```bash
echo "logs/" >> .gitignore
git add .gitignore src/kanyo/utils/logger.py scripts/test_logger.py
git commit -m "Ho 1: Add logging system"
git push
```

---

## Part 6: Testing Framework (20 minutes)

### Set Up pytest

**Create `pytest.ini`:**
```ini
[pytest]
testpaths = tests
python_files = test_*.py
python_classes = Test*
python_functions = test_*
addopts =
    --verbose
    --cov=src/kanyo
    --cov-report=term-missing
    --cov-report=html
```

### Create Sample Test

**In `tests/test_detection.py`:**
```python
"""Tests for detection module"""
import pytest
from pathlib import Path
import sys

# Add src to path
sys.path.insert(0, str(Path(__file__).parent.parent / "src"))

def test_imports():
    """Verify detection module imports work"""
    from kanyo.detection import capture, detect, events
    assert capture is not None
    assert detect is not None
    assert events is not None

def test_config_loads():
    """Verify configuration loads"""
    from kanyo.utils.config import load_config
    config = load_config("config.yaml")
    assert config is not None
    assert "detection" in config
    assert "video" in config

# Placeholder tests for future
@pytest.mark.skip(reason="Not implemented yet")
def test_falcon_detection():
    """Test falcon detection on sample frame"""
    pass

@pytest.mark.skip(reason="Not implemented yet")
def test_event_detection():
    """Test enter/exit event detection"""
    pass
```

### Run Tests

```bash
# Make sure venv is activated
pytest

# Should see:
# - test_imports PASSED
# - test_config_loads PASSED
# - test_falcon_detection SKIPPED
# - test_event_detection SKIPPED
```

**You should see coverage report** showing which code is tested.

### Commit Testing Setup

```bash
git add pytest.ini tests/
git commit -m "Ho 1: Set up testing framework"
git push
```

---

## Part 7: Development Tools (15 minutes)

### Configure Code Formatting

**Create `.flake8`:**
```ini
[flake8]
max-line-length = 100
exclude =
    venv,
    __pycache__,
    .git
ignore = E203, W503
```

**Create `pyproject.toml`:**
```toml
[tool.black]
line-length = 100
target-version = ['py310']
include = '\.pyi?$'
extend-exclude = '''
/(
  venv
  | __pycache__
)/
'''

[tool.isort]
profile = "black"
line_length = 100
```

### Format Your Code

```bash
# Format all Python files
black src/ tests/ scripts/

# Sort imports
isort src/ tests/ scripts/

# Check for issues
flake8 src/ tests/ scripts/

# Type checking (will show issues, that's ok for now)
mypy src/kanyo/
```

**Commit the configs:**
```bash
git add .flake8 pyproject.toml
git commit -m "Ho 1: Configure code quality tools"
git push
```

---

## Part 8: Documentation Framework (20 minutes)

### Create Architecture Doc

**In `docs/architecture.md`:**
```markdown
# Kanyō Architecture

## System Overview

```
YouTube Stream → yt-dlp → Video Segments
                            ↓
                    Frame Extraction
                            ↓
                    YOLOv8 Detection
                            ↓
                    Event Detection
                            ↓
                    JSON + Thumbnails
                            ↓
                    Static Site Generator
                            ↓
                    Cloudflare Pages
```

## Components

### Detection Pipeline
- **capture.py** - Download stream segments with yt-dlp
- **detect.py** - Run YOLOv8 on frames, return detections
- **events.py** - Process detections into events (enter/exit/movement)

### Generation Pipeline
- **site_generator.py** - Generate HTML from detection JSON

### Utilities
- **config.py** - Configuration management
- **logger.py** - Logging setup

## Data Flow

1. **Input:** YouTube live stream URL
2. **Capture:** Download recent segments (e.g., last 2 hours)
3. **Process:** Extract frames at interval (e.g., every 30 frames)
4. **Detect:** Run YOLOv8, filter for birds
5. **Events:** Identify enter/exit/movement events
6. **Output:** JSON with timestamps + thumbnail images
7. **Generate:** Create static HTML timeline
8. **Deploy:** Push to Cloudflare Pages

## Configuration

See `config.yaml` for all settings.

Key parameters:
- `detection.confidence` - Threshold for bird detection
- `detection.frame_interval` - How often to check frames
- `events.enter_threshold` - Frames to confirm entrance

## Deployment

### Local Development
```bash
python scripts/run_detection.py --video sample.mp4
```

### Automated (GitHub Actions)
- Runs hourly
- Downloads last 2 hours of stream
- Processes and deploys to Cloudflare

## Testing

```bash
pytest                          # Run all tests
pytest tests/test_detection.py  # Specific test
pytest --cov                    # With coverage
```
```

### Create Data Model Doc

**In `docs/data-model.md`:**
```markdown
# Kanyō Data Model

## Detection Event

```json
{
  "timestamp": "2024-12-15T14:23:45Z",
  "youtube_time": "3h45m23s",
  "event_type": "falcon_enters",
  "confidence": 0.94,
  "thumbnail": "thumbs/20241215_142345.jpg",
  "falcon_count": 1,
  "bbox": [120, 340, 280, 520],
  "metadata": {
    "video_segment": "segment_20241215_1200.mp4",
    "frame_number": 12450
  }
}
```

## Event Types

- **falcon_enters** - Bird appears after absence (N frames)
- **falcon_exits** - Bird disappears after presence (N frames)
- **movement_after_stasis** - Movement after 5+ minutes still
- **falcon_count_change** - Number of visible falcons changes
- **significant_activity** - High motion detected

## Detection File

**File:** `site/data/detections.json`

```json
{
  "generated_at": "2024-12-15T15:00:00Z",
  "stream_url": "https://youtube.com/watch?v=...",
  "detection_config": {
    "model": "yolov8n.pt",
    "confidence": 0.6
  },
  "events": [
    { /* event 1 */ },
    { /* event 2 */ }
  ],
  "summary": {
    "total_events": 45,
    "falcon_enters": 12,
    "falcon_exits": 12,
    "movement_events": 21,
    "time_range": {
      "start": "2024-12-15T08:00:00Z",
      "end": "2024-12-15T15:00:00Z"
    }
  }
}
```

## Configuration Schema

See `config.yaml` - all settings documented inline.
### Update Main README

**Edit `README.md` to be comprehensive:**
# Kanyō (観鷹)
**Contemplating Falcons**

Automated detection and timeline generation for the Memorial Hall Peregrine Falcon cam at Harvard.

## Origin Story

Born from a conversation with Claudia Goldin (Nobel laureate in Economics) on a flight to New York, where she expressed interest in having the live feed automatically mark timestamps when the peregrines are actually in frame.

## Project Status

🚧 **In Development** - Ho 1 complete (project structure established)

**Current Phase:** Foundation
**Next:** Falcon detection implementation (Ho 2)

## Quick Start

# Clone and setup
git clone https://github.com/YOUR_USERNAME/kanyo.git
cd kanyo

# Create virtual environment
python3 -m venv .venv
source .venv/bin/activate  # Mac/Linux

# Install dependencies
pip install -r requirements-dev.txt

# Run tests
pytest

# Test configuration
python scripts/test_config.py
```

## Project Structure

```
kanyo/
├── src/kanyo/           # Main package
│   ├── detection/       # Video capture and falcon detection
│   ├── generation/      # Static site generation
│   └── utils/           # Configuration and logging
├── tests/               # Test suite
├── scripts/             # Utility scripts
├── docs/                # Architecture and design docs
├── devlog/              # Development journal (hos)
└── site/                # Generated static site
```

## Documentation

- [Architecture](docs/architecture.md) - System design and data flow
- [Data Model](docs/data-model.md) - Event structure and schemas
- [Development Log](devlog/) - Ho-by-ho progress

## Technology Stack

- **Python 3.10+** - Core language
- **YOLOv8** - Object detection
- **OpenCV** - Video processing
- **yt-dlp** - Stream capture
- **GitHub Actions** - Automation
- **Cloudflare Pages** - Hosting

## Development

```bash
# Activate environment
source .venv/bin/activate

# Format code
black src/ tests/
isort src/ tests/

# Check code quality
flake8 src/ tests/
mypy src/kanyo/

# Run tests with coverage
pytest --cov
```

## Configuration

See `config.yaml` for all settings. Key parameters:

- **video.source** - YouTube stream URL
- **detection.confidence** - Detection threshold (0.0-1.0)
- **detection.model** - YOLOv8 model (n/s/m)
- **events.enter_threshold** - Frames to confirm entrance

## Roadmap

- [x] Ho 0: Project planning
- [x] Ho 0.5: Tool mastery
- [x] Ho 1: Project structure ← **You are here**
- [ ] Ho 2: Falcon detection
- [ ] Ho 3: Event detection
- [ ] Ho 4: Stream capture
- [ ] Ho 5: Pipeline assembly
- [ ] Ho 6: GitHub Actions automation
- [ ] Ho 7: Static site generation
- [ ] Ho 8: Cloudflare deployment
- [ ] Ho 9: User tagging system
- [ ] Ho 10-11: Polish and launch

## Contributing

This is a personal learning project, but feedback and suggestions are welcome!

## License

MIT (or your choice)

## Acknowledgments

- Claudia Goldin - For the inspiration
- Memorial Hall Falcon Cam - For the falcons
- Anthropic - For Claude (development assistant)
```

### Commit Documentation

```bash
git add docs/ README.md
git commit -m "Ho 1: Add architecture and data model documentation"
git push
```

---

## Part 9: Devlog for Ho 1 (10 minutes)

### Document Your Work

**Create `devlog/ho-01-git-good.md`:**
```markdown
# Ho 1: Git Good

**Date:** [today's date]
**Duration:** [actual time]
**Status:** Complete ✓

## What Was Built

### Project Structure
- [x] Virtual environment created and configured
- [x] Professional directory layout established
- [x] Module structure with proper `__init__.py` files

### Dependencies
- [x] requirements.txt (production dependencies)
- [x] requirements-dev.txt (development tools)
- [x] All packages installed and verified

### Configuration System
- [x] config.yaml with sensible defaults
- [x] config.py for loading and validation
- [x] .env.example for sensitive values
- [x] Tested configuration loading

### Development Infrastructure
- [x] Logging system (logger.py)
- [x] Testing framework (pytest)
- [x] Code formatting (.flake8, pyproject.toml)
- [x] Sample tests passing

### Documentation
- [x] Architecture document
- [x] Data model specification
- [x] Updated comprehensive README
- [x] Inline code documentation

## Understanding Check

**Tier 1 (Black Box):**
- pytest internals
- YOLOv8/OpenCV installation (just use them)

**Tier 2 (Functional Understanding):**
- Virtual environment concept and usage ✓
- Project structure organization ✓
- Configuration loading ✓
- Logging setup ✓
- Testing workflow ✓

**Tier 3 (Deep):**
- Not needed for this ho

## Challenges Encountered

[Any issues with venv, dependencies, or setup]

## Code Quality

**Formatting:**
- [x] Black formatting applied
- [x] isort for imports
- [x] flake8 checks passing (or documented exceptions)

**Testing:**
- Tests created: 2 (plus 2 skipped placeholders)
- Coverage: [percentage from pytest --cov]

## Key Learnings

### About Project Structure
[What did you learn about organizing Python projects?]

### About Dependencies
[Insights about virtual environments and requirements.txt]

### About Configuration
[Understanding of config management]

## Files Created/Modified

**New files:**
- requirements.txt, requirements-dev.txt
- config.yaml, .env.example
- src/kanyo/utils/config.py
- src/kanyo/utils/logger.py
- scripts/test_config.py, scripts/test_logger.py
- tests/test_detection.py
- pytest.ini, .flake8, pyproject.toml
- docs/architecture.md, docs/data-model.md

**Modified:**
- README.md (comprehensive update)
- .gitignore (venv, logs, .env)

## Next Steps

**Ready for Ho 2: Falcon Vision**
- Download sample falcon cam footage
- Implement basic YOLOv8 detection
- Test on real frames
- Save detections with timestamps

---

**Completed:** [timestamp]
**Git Commits:** [count from `git log --oneline | wc -l`]
**Tests Passing:** ✓
**Confidence Level (1-5):** ___
```

**Commit the devlog:**
```bash
git add devlog/ho-01-git-good.md
git commit -m "Ho 1: Complete devlog"
git push
```

---

## Ho 1 Completion Checklist

**Before moving to Ho 2, verify:**

### Environment
- [ ] Virtual environment created and activates properly
- [ ] All dependencies installed without errors
- [ ] Can import ultralytics, cv2, yt_dlp

### Structure
- [ ] Directory structure matches spec
- [ ] All __init__.py files have docstrings
- [ ] .gitignore properly configured

### Configuration
- [ ] config.yaml exists with all sections
- [ ] config.py loads configuration successfully
- [ ] scripts/test_config.py runs without errors

### Logging
- [ ] logger.py implemented
- [ ] scripts/test_logger.py creates log entries
- [ ] logs/ directory created and ignored by git

### Testing
- [ ] pytest.ini configured
- [ ] Can run pytest successfully
- [ ] At least 2 tests pass

### Documentation
- [ ] docs/architecture.md complete
- [ ] docs/data-model.md complete
- [ ] README.md comprehensive
- [ ] devlog/ho-01-git-good.md filled out

### Git
- [ ] All changes committed with good messages
- [ ] Pushed to GitHub
- [ ] GitHub repo looks professional

---

## Common Issues & Solutions

### "ModuleNotFoundError" when running scripts

**Problem:** Python can't find kanyo module
**Solution:**
```python
# Add this to top of scripts
import sys
from pathlib import Path
sys.path.insert(0, str(Path(__file__).parent.parent / "src"))
```

### "pip: command not found" or wrong pip

**Problem:** Using system pip, not venv pip
**Solution:**
```bash
# Activate venv first
source .venv/bin/activate
which pip  # Should show kanyo/.venv/bin/pip
```

### YOLOv8 installation fails

**Problem:** Dependency conflicts
**Solution:**
```bash
# Try updating pip first
pip install --upgrade pip
pip install ultralytics
```

### Tests not discovering

**Problem:** pytest can't find tests
**Solution:**
```bash
# Run from kanyo root directory
pytest -v  # Verbose to see what's happening
```

### GitHub push rejected

**Problem:** Remote has changes you don't have
**Solution:**
```bash
git pull --rebase
# Resolve any conflicts
git push
```

---

## Understanding Verification

**Can you answer these?**

1. **Why use a virtual environment?**
   - Isolates dependencies per project
   - Prevents version conflicts
   - Makes project portable

2. **What's in requirements.txt vs requirements-dev.txt?**
   - requirements.txt: Production dependencies (what kanyo needs to run)
   - requirements-dev.txt: Development tools (testing, formatting, etc.)

3. **How does the config system work?**
   - Loads from config.yaml
   - Can override with environment variables
   - Validates required fields

4. **What's the purpose of __init__.py?**
   - Makes directory a Python package
   - Can contain package-level imports
   - Holds package docstring and version

5. **How do you run tests?**
   - `pytest` from project root
   - Tests discovered in tests/ directory
   - Coverage report shows what's tested

**If you can answer these: You've achieved Tier 2 understanding. ✓**

---

## What's Next?

**Ho 2: "Falcon Vision"** will build on this foundation:
- Download sample footage from Memorial Hall cam
- Implement basic YOLOv8 detection
- Process frames and identify birds
- Save detections with timestamps and confidence
- Create first real data output

**Estimated time:** 2 hours
**Complexity:** Medium (using YOLOv8 API, Tier 2 understanding)

---

## Reflection

**Before Ho 2, think about:**
1. Is your project structure clear and organized?
2. Do you understand how to activate venv and install packages?
3. Can you run tests and see them pass?
4. Is your documentation accurate?

**When ready:** Return to Claude.ai and report:
> "Ho 1 complete! Structure established, dependencies installed, tests passing. Ready for Ho 2."

---

**Completed:** ___________
**Total Commits:** ___________
**Tests Passing:** ___________
**Confidence Level (1-5):** ___________
