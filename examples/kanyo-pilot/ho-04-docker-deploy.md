# Ho-04: Docker Deployment & Multi-Stream Management

**Goal:** Deploy Kanyō as a multi-container system with proper data management, running 2+ falcon cam streams simultaneously on your HP ProDesk with ZFS-backed persistence.

**Learning Focus:** This is your first Docker deployment. We'll explain every concept and command. By the end, you'll understand containerization deeply through building real infrastructure with professional-grade data governance.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Docker Fundamentals](#docker-fundamentals)
3. [Project Structure](#project-structure)
4. [Phase 1: Containerize Kanyō](#phase-1-containerize-kanyo)
5. [Phase 2: Multi-Stream Orchestration](#phase-2-multi-stream-orchestration)
6. [Phase 3: Deployment (Tier 1 - Quick Start)](#phase-3-deployment-tier-1---quick-start)
7. [Phase 4: Deployment (Tier 2 - Production/ZFS)](#phase-4-deployment-tier-2---productionzfs)
8. [Phase 5: Web Admin Panel (Optional)](#phase-5-web-admin-panel-optional)
9. [Operations Guide](#operations-guide)
10. [Troubleshooting](#troubleshooting)
11. [Success Criteria](#success-criteria)

---

## Prerequisites

### What You Need

✅ **Ho-03 Complete:**
- Kanyō detection working on Mac
- Clips being created
- Notifications working
- Code in Git

✅ **Deployment Box (HP ProDesk):**
- Debian 13 installed
- Docker installed
- ZFS installed and configured
- SSH access
- 16GB RAM, 256GB NVMe storage

✅ **Skills Required:**
- Basic Linux commands (cd, ls, cat)
- Text editing (nano, vim, or VS Code)
- SSH/SCP file transfer
- **No Docker knowledge needed** - we teach you

### Verify Docker is Installed

SSH into your HP ProDesk:
```bash
ssh atmarcus@kanyo

# Check Docker
docker --version
# Should show: Docker version 20.x or newer

# Check docker-compose
docker-compose --version
# Should show: docker-compose version 1.29+ or 2.x+

# Test Docker works
docker run hello-world
# Should download and run successfully

# Verify ZFS
zfs list
# Should show your rpool
```

**If Docker isn't installed:**
```bash
# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Add your user to docker group (avoid sudo)
sudo usermod -aG docker $USER

# Log out and back in for group to take effect
exit
ssh atmarcus@kanyo

# Verify
docker ps
# Should show empty list (not permission error)
```

---

## Docker Fundamentals

### What is Docker?

**The Problem Docker Solves:**

You built Kanyō on your Mac:
- Python 3.11
- Specific versions of OpenCV, ffmpeg, ultralytics
- macOS-specific paths and configs

**To run on HP ProDesk, you'd need to:**
1. Install Python
2. Install all dependencies (exact versions)
3. Configure paths
4. Hope nothing breaks
5. Repeat for every update

**Docker Solution:**

Package everything into a **container image** that runs identically everywhere.

---

### Key Concepts

#### 1. Image vs Container

**Image = Recipe**
- Frozen snapshot of code + dependencies
- Built once, runs anywhere
- Like a blueprint or frozen meal

**Container = Running Instance**
- Image brought to life
- Actual running process
- Like cooking from the recipe

**Analogy:**
```
Image:     yolov8n.pt model file (static, on disk)
Container: Python process using the model (running, in RAM)

Image:     kanyo:latest (built once)
Container: kanyo-harvard (running now, processing stream)
```

#### 2. Dockerfile

**What it is:** Instructions to build an image.

**Example:**
```dockerfile
FROM python:3.11              # Start with Python installed
COPY src/ /app/src/           # Add your code
RUN pip install opencv-python # Add dependencies
CMD ["python", "app.py"]      # What to run
```

**Result:** An image containing Python + your code + dependencies.

#### 3. Volumes (Bind Mounts)

**The Problem:** Containers are ephemeral. When you stop a container, everything inside disappears.

**Solution:** Bind mount host directories into the container.
```yaml
volumes:
  - /opt/services/kanyo-harvard/clips:/app/clips
  #  ↑ Host path (persistent, ZFS-backed)  ↑ Container path
```

**What this means:**
- Container writes to `/app/clips`
- Actually writes to `/opt/services/kanyo-harvard/clips` on host ZFS
- Data survives container restart/rebuild
- ZFS compression, snapshots, quotas all work

**Critical: ONLY bind mount persistent data, not code!**
- ✅ Bind mount: `/opt/services/kanyo-harvard/clips` (data)
- ✅ Bind mount: `/opt/services/kanyo-harvard/config.yaml` (config)
- ❌ Don't bind mount: `src/` (code goes IN the image)

#### 4. docker-compose

**The Problem:** Running multiple containers with Docker CLI is tedious:
```bash
docker run -d --name kanyo-harvard -v /opt/.../clips:/app/clips ...
docker run -d --name kanyo-nsw -v /opt/.../clips:/app/clips ...
docker run -d --name web -p 5000:5000 ...
# Repeat for 8+ containers...
```

**Solution:** Define all containers in one YAML file.
```yaml
services:
  harvard:
    build: .
    volumes:
      - /opt/services/kanyo-harvard/clips:/app/clips

  nsw:
    build: .
    volumes:
      - /opt/services/kanyo-nsw/clips:/app/clips
```

**Then:** `docker-compose up -d` starts everything.

---

### Docker Image Layers

**Images are built in layers:**
```dockerfile
FROM python:3.11        # Layer 1: Base Python (300MB)
RUN apt install ffmpeg  # Layer 2: Add ffmpeg (100MB)
RUN pip install opencv  # Layer 3: Add OpenCV (200MB)
COPY src/ /app/src/     # Layer 4: Add your code (5MB)
```

**Why this matters:**

1. **Caching:** If you change your code (Layer 4), Docker only rebuilds that layer. Layers 1-3 are cached.
2. **Sharing:** Multiple images can share base layers (all use same Python layer).
3. **Size:** Order matters - put least-changed things first.

**Best practice:**
```dockerfile
# ❌ BAD (rebuilds everything when code changes)
COPY src/ /app/
RUN pip install -r requirements.txt

# ✅ GOOD (only rebuilds last layer when code changes)
COPY requirements.txt /app/
RUN pip install -r requirements.txt
COPY src/ /app/
```

---

### Summary: The Stack
```
┌─────────────────────────────────────────────────────────────┐
│ Host: HP ProDesk (Debian + ZFS)                             │
│                                                             │
│  ZFS Datasets (Persistent):                                 │
│  ├─ /opt/services/kanyo-admin/    (code, compose, configs)  │
│  ├─ /opt/services/kanyo-harvard/  (clips, logs, config)     │
│  └─ /opt/services/kanyo-nsw/      (clips, logs, config)     │
│                                                             │
│  Docker Engine:                                             │
│  ├─ Image: kanyo:latest (code baked in)                     │
│  │                                                          │
│  ├─ Container: kanyo-harvard                                │
│  │  ├─ Code: /app/src/ (from image)                         │
│  │  └─ Data: /app/clips → /opt/services/kanyo-harvard/clips │
│  │                                                          │
│  └─ Container: kanyo-nsw                                    │
│     ├─ Code: /app/src/ (from image)                         │
│     └─ Data: /app/clips → /opt/services/kanyo-nsw/clips.    │
└─────────────────────────────────────────────────────────────┘
```

---

## Project Structure

### On Your Mac (Development)
```
~/Vaults/.../kanyo-contemplating-falcons-dev/
├── src/
│   └── kanyo/
│       ├── detection/
│       ├── utils/
│       └── ...
├── models/                        # Will be baked into image
│   └── yolov8n.pt
├── requirements.txt
├── config.yaml                    # Template only
├── test_config_harvard.yaml
├── test_config_nsw.yaml
├── devlog/
│   ├── ho-03-live-detection-notification.md
│   └── ho-04-docker-deployment.md (this file)
└── (new files we'll create)
    ├── Dockerfile                 # Image definition
    ├── docker-compose.yml         # Orchestration
    ├── .env.example               # Path template
    ├── .dockerignore              # Build exclusions
    └── web/                       # Admin UI
        ├── Dockerfile
        ├── app.py
        ├── requirements.txt
        └── templates/
```

### On HP ProDesk (Production - Tier 2)
```
/opt/services/
├── kanyo-admin/                        # ZFS: rpool/sage/kanyo/admin
│   ├── docker-compose.yml              # Orchestration (git-tracked)
│   ├── .env                            # Paths (gitignored)
│   ├── Dockerfile                      # Image def (git-tracked)
│   ├── .dockerignore                   # Build exclusions (git-tracked)
│   ├── .gitignore                      # Git exclusions (git-tracked)
│   ├── requirements.txt                # Python deps (git-tracked)
│   ├── src/                            # Code (git-tracked)
│   │   └── kanyo/
│   │       ├── detection/
│   │       ├── generation/
│   │       └── utils/
│   ├── configs/                        # Stream configs (git-tracked)
│   │   ├── harvard.yaml                # Harvard stream config
│   │   ├── nsw.yaml                    # NSW stream config
│   │   └── osprey.yaml                 # Osprey stream config (template)
│   └── web/                            # Admin UI (optional, future)
│       ├── Dockerfile
│       ├── app.py
│       ├── requirements.txt
│       └── templates/
│
├── kanyo-harvard/                      # ZFS: rpool/sage/kanyo/harvard
│   ├── clips/                          # Video clips (gitignored)
│   │   └── 2025-12-18/
│   │       ├── falcon_143025_arrival.mp4
│   │       └── falcon_143025_arrival.jpg
│   └── logs/                           # Logs (gitignored)
│       └── kanyo.log
│
├── kanyo-nsw/                          # ZFS: rpool/sage/kanyo/nsw
│   ├── clips/                          # Video clips (gitignored)
│   └── logs/                           # Logs (gitignored)
│
└── kanyo-osprey/                       # ZFS: rpool/sage/kanyo/osprey (future)
    ├── clips/                          # Video clips (gitignored)
    └── logs/                           # Logs (gitignored)
```

### ZFS Layout (Tier 2)
```bash
$ zfs list | grep kanyo
NAME                      USED  AVAIL  REFER  MOUNTPOINT
rpool/sage/kanyo          1.5G   366G    96K  none
rpool/sage/kanyo/admin     10M   366G    10M  /opt/services/kanyo-admin
rpool/sage/kanyo/harvard  512M   366G   512M  /opt/services/kanyo-harvard
rpool/sage/kanyo/nsw      445M   366G   445M  /opt/services/kanyo-nsw
rpool/sage/kanyo/osprey   267M   366G   267M  /opt/services/kanyo-osprey
```

**Key principle:** Each ZFS dataset is independently snapshotable, quotable, compressible.

---

## Phase 1: Containerize Kanyō

### Step 1.1: Create Dockerfile

**What this does:** Defines how to build the Kanyō image (code baked in).

**Create `Dockerfile` in project root:**

```dockerfile
# Base image: Debian with Python 3.11
FROM python:3.11-slim-bookworm

# Build args for runtime user/group (defaults to 1000:1000)
ARG APP_UID=1000
ARG APP_GID=1000

# Install system dependencies
# ffmpeg: video processing
# libgl1, libglib2.0-0: OpenCV dependencies
RUN apt-get update && apt-get install -y \
    ffmpeg \
    libgl1 \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender1 \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Create non-root user and group (before installing Python deps)
RUN groupadd -g ${APP_GID} app && \
    useradd -u ${APP_UID} -g ${APP_GID} -m -s /bin/bash app

# Create directories for runtime and YOLO cache
RUN mkdir -p /app/clips /app/logs /app/.config

# Copy requirements and install Python dependencies
# Do this BEFORE copying code (better caching)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Set YOLO cache and config directories (before model download)
ENV YOLO_CONFIG_DIR=/app/.config
ENV YOLO_CACHE_DIR=/app/.config

# Download YOLO model directly into app-owned cache directory
RUN python -c "from ultralytics import YOLO; YOLO('yolov8n.pt')"

# Copy application code INTO the image (not bind mounted)
COPY src/ ./src/

# Set ownership of runtime-writable directories only
RUN chown -R app:app /app/src /app/clips /app/logs /app/.config

# Set Python path so imports work
ENV PYTHONPATH=/app/src

# Suppress OpenCV h264 warnings
ENV OPENCV_FFMPEG_LOGLEVEL=-8

# Set safe default umask for runtime file creation
ENV UMASK=027

# Switch to non-root user (final privilege change)
USER app

# Run the detection monitor with umask applied
CMD ["/bin/sh", "-c", "umask ${UMASK} && exec python -m kanyo.detection.realtime_monitor"]
```

**Critical understanding:**
```dockerfile
COPY src/ ./src/
```

This puts your code **inside the image**. You do NOT bind mount `src/` from the host. Code is immutable, baked into the image. To update code, rebuild the image.

**Only persistent data is bind mounted:**
- `clips/` (video files)
- `logs/` (application logs)
- `config.yaml` (stream settings)

---

### Step 1.2: Create .dockerignore

**What this does:** Tells Docker which files to NOT copy into image.

**Create `.dockerignore` in project root:**
```
# Version control
.git
.gitignore

# Python
__pycache__
*.pyc
*.pyo
*.pyd
.Python
.venv/
env/
*.egg-info/
.pytest_cache/

# IDE
.vscode/
.idea/
*.swp
*.swo

# Data (huge, not needed in image - will be bind mounted)
clips/
data/
logs/
*.mp4
*.jpg
*.png

# Docs
devlog/
*.md
!README.md

# Config (will be bind mounted at runtime)
config*.yaml
.env

# Tests
tests/
```

---

### Step 1.3: Update requirements.txt

**Ensure all dependencies are listed:**
```txt
# Core detection
opencv-python-headless==4.8.1
ultralytics==8.0.200
numpy==1.24.3
Pillow==10.1.0

# Notifications
requests==2.31.0

# Configuration
pyyaml==6.0.1

# Logging
loguru==0.7.2

# Video processing (yt-dlp for YouTube)
yt-dlp==2023.11.16
```

**Note:** Use `opencv-python-headless` (not `opencv-python`) in Docker. Headless version doesn't include GUI dependencies (smaller, faster).

---

### Step 1.4: Test Build Locally

**On your Mac, build the image:**
```bash
cd ~/Vaults/.../kanyo-contemplating-falcons-dev

# Build the image
docker build -t kanyo:latest .

# Watch the build process
# First build: 5-10 minutes (downloading everything)
# Subsequent builds: 30 seconds (using cache)
```

**Verify image exists:**
```bash
docker images | grep kanyo
# kanyo  latest  abc123def456  2 minutes ago  1.2GB
```

---

### Step 1.5: Test Run Locally

**Quick test that container works:**
```bash
# Run container interactively (test mode)
docker run --rm -it \
  -v $(pwd)/test_config_nsw.yaml:/app/config.yaml:ro \
  -v $(pwd)/clips:/app/clips \
  kanyo:latest \
  python -m kanyo.detection.realtime_monitor --duration 1

# Flags explained:
# --rm:       Delete container when stopped
# -it:        Interactive (see output)
# -v:         Mount volumes (config + clips)
# :ro:        Read-only mount
# --duration: Override command (test 1 minute)
```

**If it works:** Container is good! Stop it with Ctrl+C.

---

## Phase 2: Multi-Stream Orchestration

### Step 2.1: Create docker-compose.yml

**What this does:** Defines all containers with proper bind mounts to ZFS-backed storage.

**Create `docker-compose.yml` in project root:**

```
services:
  harvard:
    build: .
    image: kanyo:latest
    container_name: kanyo-harvard
    env_file: .env.docker
    volumes:
      - ./test_config_harvard.yaml:/app/config.yaml:ro
      - ${KANYO_CAM1_ROOT:-./data/cam1}/clips:/app/clips
      - ${KANYO_CAM1_ROOT:-./data/cam1}/logs:/app/logs
    environment:
      - PYTHONUNBUFFERED=1
    restart: unless-stopped
    logging:
      driver: json-file
      options:
        max-size: 10m
        max-file: "3"

  nsw:
    build: .
    image: kanyo:latest
    container_name: kanyo-nsw
    env_file: .env.docker
    volumes:
      - ./test_config_nsw.yaml:/app/config.yaml:ro
      - ${KANYO_CAM2_ROOT:-./data/cam2}/clips:/app/clips
      - ${KANYO_CAM2_ROOT:-./data/cam2}/logs:/app/logs
    environment:
      - PYTHONUNBUFFERED=1
    restart: unless-stopped
    logging:
      driver: json-file
      options:
        max-size: 10m
        max-file: "3"
```


**Key principle:**
```yaml
volumes:
  - ${KANYO_HARVARD_DATA}/clips:/app/clips        # Bind mount: data
  - ${KANYO_HARVARD_DATA}/logs:/app/logs          # Bind mount: logs
  - ${KANYO_HARVARD_DATA}/config.yaml:/app/config.yaml:ro  # Bind mount: config
  # src/ is NOT here - it's in the image!
```

---

### Step 2.2: Create .env.example

**What this does:** Template for configuring paths (Tier 1 vs Tier 2).

**Create `.env.example`:**
```bash
# ============================================================================
# KANYO – Docker / Deployment Environment
# ============================================================================
# This file defines Docker/runtime deployment details ONLY.
#
# - Host filesystem layout (Tier 1 vs Tier 2)
# - Secrets required to enable integrations
#
# Application behavior (streams, channels, thresholds, notifications)
# lives in per-camera YAML config files.
#
# This file is loaded by docker-compose via `env_file:`
# ============================================================================


# ============================================================================
# TIER 1: QUICK START (RELATIVE PATHS)
# ============================================================================
# Use this for local testing, development, or Portainer.
# Data will be stored relative to docker-compose.yml.
# Uncomment to enable Tier 1.


# KANYO_CAM1_ROOT=./data/cam1
# KANYO_CAM2_ROOT=./data/cam2
# KANYO_CAM3_ROOT=./data/cam3



# ============================================================================
# TIER 2: PRODUCTION (ABSOLUTE PATHS)
# ============================================================================
# Use this for production deployments.
# These paths may be backed by ZFS, LVM, RAID, or any durable storage.
# ZFS examples are provided below as OPTIONAL reference.


KANYO_CAM1_ROOT=/opt/services/kanyo-cam1
KANYO_CAM2_ROOT=/opt/services/kanyo-cam2
KANYO_CAM3_ROOT=/opt/services/kanyo-cam3



# ============================================================================
# SECRETS (GLOBAL)
# ============================================================================
# Secrets required to enable integrations.
# Per-camera behavior (channels, topics, enable/disable)
# is defined in YAML config files.


TELEGRAM_BOT_TOKEN=your_bot_token_here



# ============================================================================
# OPTIONAL: ZFS EXAMPLE FOR PRODUCTION STORAGE
# ============================================================================
# Replace <pool> with your actual ZFS pool name
# (e.g., rpool, tank, data, fast, etc.)
#
#   sudo zfs create <pool>/kanyo
#   sudo zfs create -o mountpoint=/opt/services/kanyo-admin <pool>/kanyo/admin
#   sudo zfs create -o mountpoint=/opt/services/kanyo-cam1  <pool>/kanyo/cam1
#   sudo zfs create -o mountpoint=/opt/services/kanyo-cam2  <pool>/kanyo/cam2
#   sudo zfs create -o mountpoint=/opt/services/kanyo-cam3  <pool>/kanyo/cam3
#
# Optional:
#   sudo zfs set compression=lz4 <pool>/kanyo
#   sudo zfs set quota=500G <pool>/kanyo/cam1
#   sudo zfs set quota=500G <pool>/kanyo/cam2
#
# Example daily snapshots:
#   0 2 * * * /usr/sbin/zfs snapshot -r <pool>/kanyo@daily-$(date +\%Y\%m\%d)
```

---

### Step 2.3: Create .gitignore for kanyo-admin

**What goes in Git vs what doesn't:**

**Create `.gitignore` in project root:**
```
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
.venv/
env/
*.egg-info/
.pytest_cache/

# IDE
.vscode/
.idea/
*.swp
*.swo

# Environment (contains secrets)
.env

# Data (huge, ZFS-backed)
clips/
data/
logs/
*.mp4
*.jpg
*.png

# Models (huge, can be re-downloaded)
models/

# Web UI state (runtime-generated)
web/config.json
web/users.db
web/settings.yaml
*.db

# Docker
.dockerignore

# OS
.DS_Store
Thumbs.db
```

**This means:**
- ✅ Git tracks: `src/`, `docker-compose.yml`, `Dockerfile`, `.env.example`
- ❌ Git ignores: `.env`, `models/`, `clips/`, `web/*.db`

---

### Step 2.3: Create Per-Stream Configs

> Each stream needs its own config.yaml in its own data directory.

**For Tier 1 testing, create directory structure:**
```bash
mkdir -p data/harvard data/nsw data/osprey
mkdir -p data/harvard/clips data/harvard/logs
mkdir -p data/nsw/clips data/nsw/logs
mkdir -p data/osprey/clips data/osprey/logs
```

**Create `data/harvard/config.yaml`:**
```yaml
# Harvard Falcon Cam Configuration
video_source: "https://www.youtube.com/watch?v=glczTFRRAK4"

# Detection
detection_confidence: 0.5
frame_interval: 3
model_path: "/root/.u8/yolov8n.pt"  # Where YOLO downloads model in container
detect_any_animal: true
exit_timeout: 60
visit_merge_timeout: 60

animal_classes: [14, 15, 16, 17, 18, 19, 20, 21, 22, 23]

# Clips (container paths - bind mounted from host)
clips_dir: "/app/clips"
log_file: "/app/logs/kanyo.log"

clip_entrance_before: 5
clip_entrance_after: 15
clip_exit_before: 15
clip_exit_after: 5
clip_merge_threshold: 180

thumbnail_entrance_offset: 5
thumbnail_exit_offset: -10

clip_compress: true
clip_crf: 23
clip_fps: 30
clip_hardware_encoding: true

# Live stream (YouTube)
live_use_ffmpeg_tee: true
live_proxy_url: "udp://127.0.0.1:12345"
buffer_dir: "/tmp/kanyo-buffer"  # Ephemeral inside container
continuous_chunk_minutes: 10

# Notifications
ntfy_enabled: true
ntfy_topic: "kanyo_falcon_harvard"
notification_cooldown_minutes: 5

# Logging
log_level: "INFO"
```

**Create `data/nsw/config.yaml`:**
```yaml
# NSW Falcon Cam Configuration
video_source: "https://www.youtube.com/watch?v=yv2RtoIMNzA"

# Detection
detection_confidence: 0.5
frame_interval: 3
model_path: "/root/.u8/yolov8n.pt"
detect_any_animal: true
exit_timeout: 60
visit_merge_timeout: 60

animal_classes: [14, 15, 16, 17, 18, 19, 20, 21, 22, 23]

# Clips
clips_dir: "/app/clips"
log_file: "/app/logs/kanyo.log"

clip_entrance_before: 5
clip_entrance_after: 15
clip_exit_before: 15
clip_exit_after: 5
clip_merge_threshold: 180

thumbnail_entrance_offset: 5
thumbnail_exit_offset: -10

clip_compress: true
clip_crf: 23
clip_fps: 30
clip_hardware_encoding: true

# Live stream
live_use_ffmpeg_tee: true
live_proxy_url: "udp://127.0.0.1:12345"
buffer_dir: "/tmp/kanyo-buffer"
continuous_chunk_minutes: 10

# Notifications
ntfy_enabled: true
ntfy_topic: "kanyo_falcon_nsw"
notification_cooldown_minutes: 5

# Logging
log_level: "INFO"
```

---

## Phase 3: Deployment (Tier 1 - Quick Start)

**Use this for:**
- First-time Docker learning
- Testing multi-stream setup
- Portainer deployments
- Simple home setups without ZFS

### Step 3.1: Prepare Files on Mac
```bash
cd ~/Vaults/.../kanyo-contemplating-falcons-dev

# Verify structure
ls -l Dockerfile docker-compose.yml .dockerignore .env.example

# Check data structure
tree data/
# Should show harvard/, nsw/ with clips/, logs/, config.yaml

# Create empty .env (uses defaults)
touch .env
```

---

### Step 3.2: Transfer to HP ProDesk
```bash
# On Mac
cd ~/Vaults/.../kanyo-contemplating-falcons-dev

# Transfer entire project
rsync -av \
  --exclude '.venv/' \
  --exclude '__pycache__/' \
  --exclude 'clips/' \
  --exclude '.git/' \
  . atmarcus@kanyo:~/kanyo-deploy/
```

---

### Step 3.3: Build and Deploy

**On HP ProDesk:**
```bash
cd ~/kanyo-deploy

# Build images (takes 5-10 minutes first time)
docker-compose build

# Start containers
docker-compose up -d

# Verify
docker-compose ps
# Both should show "Up"
```

---

### Step 3.4: Verify Deployment
```bash
# Follow logs
docker-compose logs -f

# Check resource usage
docker stats

# Check clips being created
find data/ -name "*.mp4" -mmin -60

# Check your phone for notifications
```

---

### Tier 1 Success Criteria

After 1 hour, verify:

- [ ] Both containers running: `docker-compose ps` shows "Up"
- [ ] Clips being created in `data/harvard/clips/` and `data/nsw/clips/`
- [ ] Notifications received on phone (both topics)
- [ ] Can restart individual streams: `docker-compose restart harvard`
- [ ] Containers auto-restart after crash

**If all pass:** Move to Tier 2 for production with ZFS!

---

## Phase 4: Deployment (Tier 2 - Production/ZFS)

**Use this for:**
- Long-term production deployment
- Proper data governance
- ZFS snapshots/compression/quotas
- Your actual bird box setup

### Step 4.1: Plan ZFS Layout

**Hierarchy:**
```
rpool/sage/kanyo              # Parent (no mountpoint)
├── admin                     # Code + compose + web state
├── harvard                   # Stream 1 data
├── nsw                       # Stream 2 data
└── osprey                    # Stream 3 data (future)
```

**Mountpoints:**
```
rpool/sage/kanyo/admin    → /opt/services/kanyo-admin
rpool/sage/kanyo/harvard  → /opt/services/kanyo-harvard
rpool/sage/kanyo/nsw      → /opt/services/kanyo-nsw
rpool/sage/kanyo/osprey   → /opt/services/kanyo-osprey
```

**Benefits:**
- Parent dataset: Set shared properties (compression)
- Child datasets: Individual snapshots, quotas, backups
- Admin separate: Snapshot code/config independently from data

---

### Step 4.2: Create ZFS Datasets

**On HP ProDesk:**
```bash
# Create parent dataset (no mountpoint - just organization)
sudo zfs create rpool/sage/kanyo
sudo zfs set mountpoint=none rpool/sage/kanyo

# Create admin dataset (code, compose, web UI state)
sudo zfs create -o mountpoint=/opt/services/kanyo-admin rpool/sage/kanyo/admin

# Create per-stream datasets
sudo zfs create -o mountpoint=/opt/services/kanyo-harvard rpool/sage/kanyo/harvard
sudo zfs create -o mountpoint=/opt/services/kanyo-nsw rpool/sage/kanyo/nsw
sudo zfs create -o mountpoint=/opt/services/kanyo-osprey rpool/sage/kanyo/osprey

# Verify
zfs list | grep kanyo
# rpool/sage/kanyo           96K   164G    96K  none
# rpool/sage/kanyo/admin     96K   164G    96K  /opt/services/kanyo-admin
# rpool/sage/kanyo/harvard   96K   164G    96K  /opt/services/kanyo-harvard
# rpool/sage/kanyo/nsw       96K   164G    96K  /opt/services/kanyo-nsw
# rpool/sage/kanyo/osprey    96K   164G    96K  /opt/services/kanyo-osprey

# Verify mountpoints exist
ls -l /opt/services/
# Should show all four directories
```

---

### Step 4.3: Set ZFS Properties

**Enable compression (saves ~50% disk space):**
```bash
sudo zfs set compression=lz4 rpool/sage/kanyo
# Applies to all child datasets
```

**Set quotas:**
```bash
# Admin: small quota (just code)
sudo zfs set quota=50G rpool/sage/kanyo/admin

# Streams: large quota (video data)
sudo zfs set quota=500G rpool/sage/kanyo/harvard
sudo zfs set quota=500G rpool/sage/kanyo/nsw
sudo zfs set quota=500G rpool/sage/kanyo/osprey

# Verify
zfs get quota | grep kanyo
```

**Set recordsize (optimal for large video files):**
```bash
sudo zfs set recordsize=1M rpool/sage/kanyo
```

---

### Step 4.4: Create Directory Structure

**Admin layer:**
```bash
sudo mkdir -p /opt/services/kanyo-admin
sudo chown -R atmarcus:atmarcus /opt/services/kanyo-admin
```

**Stream layers:**
```bash
# Harvard
sudo mkdir -p /opt/services/kanyo-harvard/{clips,logs}
sudo chown -R atmarcus:atmarcus /opt/services/kanyo-harvard

# NSW
sudo mkdir -p /opt/services/kanyo-nsw/{clips,logs}
sudo chown -R atmarcus:atmarcus /opt/services/kanyo-nsw

# Osprey
sudo mkdir -p /opt/services/kanyo-osprey/{clips,logs}
sudo chown -R atmarcus:atmarcus /opt/services/kanyo-osprey
```

---

### Step 4.5: Deploy Project Files to kanyo-admin

**Transfer files from Mac:**
```
# On HP ProDesk
cd /opt/services/kanyo-admin

# First time only
git clone https://github.com/sageframe-no-kaji/kanyo-contemplating-falcons-dev.git .

# Updates
git pull

# Safety snapshot before rebuild
sudo zfs snapshot rpool/sage/kanyo/admin@pre-update-$(date +%Y%m%d-%H%M)

# Rebuild and deploy
docker-compose build
docker-compose up -d
```
---

### Step 4.6: Configure Tier 2 Paths

**Create `.env` with absolute ZFS paths:**
```bash
# On HP ProDesk
cd /opt/services/kanyo-admin

cat > .env << 'EOF'
# Tier 2: Production ZFS-backed paths
KANYO_HARVARD_DATA=/opt/services/kanyo-harvard
KANYO_NSW_DATA=/opt/services/kanyo-nsw
KANYO_OSPREY_DATA=/opt/services/kanyo-osprey
EOF

# Verify
cat .env
```

**Now docker-compose will bind mount from these ZFS-backed locations.**

---

### Step 4.7: Build and Deploy
```bash
cd /opt/services/kanyo-admin

# Build images (code baked in)
docker-compose build

# Start containers
docker-compose up -d

# Verify
docker-compose ps
zfs list | grep kanyo
# Should see admin, harvard, nsw datasets
```

---

### Step 4.8: Verify ZFS Integration

**Check data is being written to ZFS:**
```bash
# Watch ZFS space usage
zfs list -o name,used,avail,refer,mountpoint | grep kanyo

# Should show growing USED as clips accumulate
# NAME                      USED  AVAIL  REFER  MOUNTPOINT
# rpool/sage/kanyo/admin     10M  50G     10M   /opt/services/kanyo-admin
# rpool/sage/kanyo/harvard  127M  500G   127M   /opt/services/kanyo-harvard
# rpool/sage/kanyo/nsw      89M   500G    89M   /opt/services/kanyo-nsw

# Check compression ratio
zfs get compressratio rpool/sage/kanyo/harvard
# Should show >1.2x (video compresses well)
```

**Verify clips exist:**
```bash
ls -lh /opt/services/kanyo-harvard/clips/$(date +%Y-%m-%d)/
find /opt/services/kanyo-harvard/clips/ -name "*.mp4" -mmin -60
```

---

### Step 4.9: Set Up Snapshots

**Manual snapshot:**
```bash
# Snapshot all datasets
sudo zfs snapshot -r rpool/sage/kanyo@manual-$(date +%Y%m%d-%H%M)

# Verify
zfs list -t snapshot | grep kanyo
```

**Automatic snapshots (cron):**
```bash
# Edit crontab
crontab -e

# Add these lines:

# Daily snapshots at 2 AM
0 2 * * * /usr/sbin/zfs snapshot -r rpool/sage/kanyo@daily-$(date +\%Y\%m\%d)

# Keep last 7 days of daily snapshots
0 3 * * * /usr/sbin/zfs list -t snapshot -o name -s creation | grep 'rpool/sage/kanyo.*@daily' | head -n -7 | xargs -n1 /usr/sbin/zfs destroy

# Weekly snapshot of admin (before code updates)
0 2 * * 0 /usr/sbin/zfs snapshot rpool/sage/kanyo/admin@weekly-$(date +\%Y\%m\%d)

# Verify cron
crontab -l
```

**Restore from snapshot:**
```bash
# List snapshots
zfs list -t snapshot | grep harvard

# Rollback (⚠️ DESTRUCTIVE)
sudo zfs rollback rpool/sage/kanyo/harvard@daily-20251218

# Or restore specific files (safer)
cd /opt/services/kanyo-harvard/.zfs/snapshot/daily-20251218/clips/
cp -r 2025-12-18/ /opt/services/kanyo-harvard/clips/
```

---

### Step 4.10: Set Up Log Rotation

**Docker logs (already configured in docker-compose.yml):**
```yaml
logging:
  driver: "json-file"
  options:
    max-size: "10m"
    max-file: "3"
# = 30MB max per container
```

**Application logs (in ZFS-backed dirs):**
```bash
sudo nano /etc/logrotate.d/kanyo

# Add:
/opt/services/kanyo-*/logs/*.log {
    daily
    rotate 7
    compress
    delaycompress
    missingok
    notifempty
    copytruncate
}

# Save and test
sudo logrotate -d /etc/logrotate.d/kanyo
```

---

### Tier 2 Success Criteria

After 24 hours, verify:

- [ ] ZFS compression working: `zfs get compressratio` shows >1.2x
- [ ] Clips accumulating: `zfs list` shows growing USED
- [ ] Snapshots being created: `zfs list -t snapshot | grep kanyo`
- [ ] Logs rotating: check `/opt/services/kanyo-harvard/logs/`
- [ ] Containers still running: `docker-compose ps`
- [ ] Can restore from snapshot (test on non-critical stream)
- [ ] Admin and stream data separated (different ZFS datasets)

**If all pass:** Production ready! 🎉

---

## Phase 5: Web Admin Panel (Optional)

**Skip if comfortable with CLI.** For managing 10+ streams or sharing access, build the web UI.

### Step 5.1: Web UI Files Structure
```
/opt/services/kanyo-admin/
└── web/
    ├── Dockerfile
    ├── app.py
    ├── requirements.txt
    ├── templates/
    │   ├── base.html
    │   ├── dashboard.html
    │   ├── stream.html
    │   └── clips.html
    ├── config.json         # Stream registry (gitignored, persistent)
    ├── users.db            # User accounts (gitignored, persistent)
    └── settings.yaml       # UI preferences (gitignored, persistent)
```

### Step 5.2: Create Web UI Files

**(Full web UI implementation from earlier - Flask app, templates, etc.)**

I'll omit the full code here for brevity, but include in final file.

### Step 5.3: Update docker-compose.yml

**Add web service:**
```yaml
services:
  # Web Admin UI
  web:
    build: ./web
    container_name: kanyo-web
    ports:
      - "5000:5000"
    volumes:
      # Docker socket (manage containers)
      - /var/run/docker.sock:/var/run/docker.sock

      # Web UI state (persistent in kanyo-admin ZFS)
      - ./web/config.json:/app/config.json
      - ./web/users.db:/app/users.db
      - ./web/settings.yaml:/app/settings.yaml

      # Read-only access to stream data (for browsing clips)
      - ${KANYO_HARVARD_DATA}/clips:/app/data/harvard/clips:ro
      - ${KANYO_HARVARD_DATA}/logs:/app/data/harvard/logs:ro
      - ${KANYO_HARVARD_DATA}/config.yaml:/app/data/harvard/config.yaml:ro

      - ${KANYO_NSW_DATA}/clips:/app/data/nsw/clips:ro
      - ${KANYO_NSW_DATA}/logs:/app/data/nsw/logs:ro
      - ${KANYO_NSW_DATA}/config.yaml:/app/data/nsw/config.yaml:ro
    restart: unless-stopped

  # Existing stream services...
  harvard:
    # ...

  nsw:
    # ...
```

**Key points:**
- Web UI state (`config.json`, `users.db`) persists in kanyo-admin ZFS
- Stream data bind mounted read-only (web UI doesn't modify clips)
- Docker socket access allows container management

### Step 5.4: Deploy Web UI
```bash
cd /opt/services/kanyo-admin

# Create empty state files
touch web/config.json web/users.db web/settings.yaml

# Build and start
docker-compose build web
docker-compose up -d web

# Access at:
http://kanyo:5000
```

---

## Operations Guide

### Daily Operations

**Check Status:**
```bash
cd /opt/services/kanyo-admin

# Quick status
docker-compose ps

# Resource usage
docker stats

# View logs
docker-compose logs -f harvard
```

**Restart Stream:**
```bash
docker-compose restart harvard
```

**Update Code:**
```bash
# On Mac: make changes, commit
git add src/
git commit -m "Fix detection"
git push

# On HP ProDesk:
cd /opt/services/kanyo-admin

# Snapshot before update (safety)
sudo zfs snapshot rpool/sage/kanyo/admin@before-update-$(date +%Y%m%d-%H%M)

# Pull changes
git pull

# Rebuild image (new code baked in)
docker-compose build

# Restart containers
docker-compose up -d

# If broken, rollback:
# sudo zfs rollback rpool/sage/kanyo/admin@before-update-...
```

**View Clips:**
```bash
find /opt/services/kanyo-*/clips/ -name "*.mp4" -mtime -1  # Last 24 hours

# Count per stream
for stream in /opt/services/kanyo-*/clips/; do
    echo "$stream: $(find $stream -name '*.mp4' | wc -l)"
done
```

**Clean Up Old Clips:**
```bash
# Delete clips older than 30 days
find /opt/services/kanyo-*/clips/ -name "*.mp4" -mtime +30 -delete
find /opt/services/kanyo-*/clips/ -name "*.jpg" -mtime +30 -delete
```

---

### Backup & Restore

**ZFS Snapshot Backup:**
```bash
# Snapshot everything
sudo zfs snapshot -r rpool/sage/kanyo@backup-$(date +%Y%m%d)

# Send to remote server
sudo zfs send -R rpool/sage/kanyo@backup-20251218 | \
  ssh backup-server "zfs receive pool/backups/kanyo"

# Or to file
sudo zfs send -R rpool/sage/kanyo@backup-20251218 | \
  gzip > /mnt/external/kanyo-backup-20251218.zfs.gz
```

**Separate Backups (Admin vs Streams):**
```bash
# Backup admin (small, frequent - daily)
sudo zfs send rpool/sage/kanyo/admin@daily-20251218 | \
  ssh backup "zfs receive pool/backups/kanyo-admin"

# Backup stream data (large, weekly)
sudo zfs send rpool/sage/kanyo/harvard@weekly-20251218 | \
  ssh backup "zfs receive pool/backups/kanyo-harvard"
```

**Restore:**
```bash
# Restore admin (code/config)
sudo zfs rollback rpool/sage/kanyo/admin@daily-20251218

# Restore stream data
sudo zfs rollback rpool/sage/kanyo/harvard@daily-20251218

# Or restore specific files
cd /opt/services/kanyo-harvard/.zfs/snapshot/daily-20251218/
cp -r clips/2025-12-18/ /opt/services/kanyo-harvard/clips/
```

---

### Monitoring

**Resource Usage:**
```bash
# Docker stats
docker stats

# ZFS stats
zfs list | grep kanyo
zfs get compressratio,used,logicalused rpool/sage/kanyo/harvard

# Disk space
df -h /opt/services/
```

---

### Adding New Streams

**Step 1: Create ZFS dataset**
```bash
sudo zfs create -o mountpoint=/opt/services/kanyo-pelican rpool/sage/kanyo/pelican
sudo zfs set quota=500G rpool/sage/kanyo/pelican
sudo mkdir -p /opt/services/kanyo-pelican/{clips,logs}
sudo chown -R atmarcus:atmarcus /opt/services/kanyo-pelican
```

**Step 2: Create config**
```bash
cp /opt/services/kanyo-harvard/config.yaml /opt/services/kanyo-pelican/config.yaml
nano /opt/services/kanyo-pelican/config.yaml
# Edit video_source and ntfy_topic
```

**Step 3: Add to docker-compose.yml**
```yaml
  pelican:
    build: .
    image: kanyo:latest
    container_name: kanyo-pelican
    volumes:
      - ${KANYO_PELICAN_DATA}/clips:/app/clips
      - ${KANYO_PELICAN_DATA}/logs:/app/logs
      - ${KANYO_PELICAN_DATA}/config.yaml:/app/config.yaml:ro
    environment:
      - PYTHONUNBUFFERED=1
    restart: unless-stopped
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
```

**Step 4: Update .env**
```bash
echo "KANYO_PELICAN_DATA=/opt/services/kanyo-pelican" >> .env
```

**Step 5: Deploy**
```bash
docker-compose up -d pelican
```

---

## Troubleshooting

### Container Won't Start

**Check logs:**
```bash
docker-compose logs harvard
```

**Common issues:**

1. **Volume mount failed** - Path doesn't exist
```bash
   mkdir -p /opt/services/kanyo-harvard/{clips,logs}
```

2. **Permission denied** - Wrong ownership
```bash
   sudo chown -R atmarcus:atmarcus /opt/services/kanyo-harvard
```

3. **Config not found** - Missing config.yaml
```bash
   cp template/config.yaml /opt/services/kanyo-harvard/
```

---

### No Clips Being Created

**Check detection:**
```bash
docker-compose logs harvard | grep FALCON
# Should see FALCON ENTERED/EXITED
```

**Check segment files:**
```bash
docker exec kanyo-harvard ls -l /tmp/kanyo-buffer/
# Should show .ts files
```

**Check permissions:**
```bash
ls -l /opt/services/kanyo-harvard/clips/
# Should be writable by your user
```

---

### High CPU Usage

**Check stats:**
```bash
docker stats
```

**Solutions:**

1. Increase `frame_interval` (3 → 5)
2. Verify hardware encoding works
3. Check for infinite loops in logs

---

### Out of Disk Space

**Check ZFS usage:**
```bash
zfs list | grep kanyo
```

**Solutions:**

1. Clean old clips:
```bash
   find /opt/services/kanyo-*/clips/ -name "*.mp4" -mtime +30 -delete
```

2. Increase quota:
```bash
   sudo zfs set quota=1T rpool/sage/kanyo/harvard
```

3. Enable/verify compression:
```bash
   sudo zfs set compression=lz4 rpool/sage/kanyo
```

---

## Critical Bug Fix & Lessons Learned

### The std::bad_alloc Crisis (Dec 24, 2025)

**The Problem:**

After building the Docker image and deploying to the test VM (192.168.1.252), containers crashed immediately with:

```
terminate called after throwing an instance of 'std::bad_alloc'
  what():  std::bad_alloc
```

**Location:** Right after `✅ Connected to tee proxy` log message - 100% reproducible.

**Impact:** Complete deployment failure. Code that worked perfectly on macOS crashed on every system with limited RAM.

---

### Debugging Journey (6+ Hours)

#### Failed Attempts

All of these seemed logical but **did not fix the crash**:

1. **Increased VM RAM**: 3.8GB → 7.8GB
   - Result: Still crashed
   - Why it failed: Not about total RAM availability

2. **Added Swap**: 4GB swap file
   - Result: Still crashed
   - Why it failed: std::bad_alloc is heap allocation failure, not OOM

3. **Docker shm_size**: Set to `2gb` in docker-compose.yml
   ```yaml
   shm_size: '2gb'
   ```
   - Result: Still crashed
   - Why it failed: Shared memory for /dev/shm, not heap

4. **MALLOC tuning**:
   ```yaml
   environment:
     - MALLOC_ARENA_MAX=2
     - MALLOC_MMAP_THRESHOLD_=131072
     - MALLOC_TRIM_THRESHOLD_=131072
   ```
   - Result: Still crashed
   - Why it failed: Arena configuration doesn't fix fragmentation

5. **ulimits**: Attempted to increase memory limits
   - Result: Docker rejected the config
   - Why it failed: Docker doesn't support all ulimit types

#### The Breakthrough

**Key Insight:** If it's not Docker-specific, test natively!

Deployed source code to VM and ran without Docker:

```bash
# On VM at /opt/services/kanyo-admin
python -m kanyo.detection.realtime_monitor --config data/nsw/config.yaml
```

**Result:** SAME crash at SAME location! 🎯

This proved:
- **Not a Docker problem**
- **Not a container configuration issue**
- **Code bug affecting all systems with limited RAM**

---

### Root Cause Analysis

#### The Bug

In `src/kanyo/detection/detect.py` line 107:

```python
@property
def model(self):
    """Lazy-load YOLO model only when needed."""
    if self._model is None:
        logger.info(f"Loading YOLO model from {self.model_path}")
        self._model = YOLO(str(self.model_path))
        logger.info("✅ Model loaded successfully")
    return self._model
```

**Execution Order:**

1. `RealtimeMonitor.run()` starts
2. FFmpeg tee process launches via `self.capture.frames()`
3. FFmpeg allocates memory for video buffers
4. **Then** YOLO model tries to load (lazy loading)
5. YOLO needs ~200MB contiguous heap space
6. Heap is fragmented after FFmpeg allocations
7. **std::bad_alloc** - can't allocate contiguous space

#### Why It Worked on macOS

- 32GB RAM → plenty of headroom
- Better memory allocator
- Fewer fragmentation issues
- Problem hidden until deployed to constrained systems

#### Why It Failed on VM

- Limited RAM (even 7.8GB insufficient)
- Memory fragmentation critical
- YOLO allocation fails every time
- Exposes the lazy-loading timing bug

---

### The Solution

**Fix:** Pre-load YOLO model **before** starting FFmpeg stream.

**Implementation** (`src/kanyo/detection/realtime_monitor.py` lines 368-373):

```python
# Pre-load YOLO model before starting stream to avoid memory fragmentation
# Model uses lazy loading, so access it now before FFmpeg allocates memory
logger.info("Pre-loading YOLO model...")
_ = self.detector.model  # Force model load
logger.info("✅ Model loaded successfully")

# Now start FFmpeg - model is already in memory
for frame_data in self.capture.frames():
    # Detection code...
```

**Why This Works:**

1. YOLO loads first → gets contiguous memory
2. FFmpeg starts second → uses remaining memory
3. No fragmentation issues
4. Deterministic allocation order

**Git Commit:** `0c26fe1`

```bash
git log --oneline -1 0c26fe1
# fix: Pre-load YOLO model to prevent std::bad_alloc crash
```

---

### Testing & Verification

#### Native Testing on VM

```bash
# After fixing code
cd /opt/services/kanyo-admin
python -m kanyo.detection.realtime_monitor --config data/nsw/config.yaml
```

**Result:**
```
2025-12-24 05:47:23 | INFO     | Pre-loading YOLO model...
2025-12-24 05:47:24 | INFO     | ✅ Model loaded successfully
2025-12-24 05:47:25 | INFO     | ✅ Connected to tee proxy
2025-12-24 05:47:25 | INFO     | Starting falcon detection...
# System runs successfully! ✅
```

#### Detection Verification Confusion

**Initial Concern:** After lowering confidence threshold (0.5 → 0.3 → 0.1), still no detection logs.

**Added Debug Logging:**
```python
if detection.count > 0:
    logger.info(f"🦅 DETECTED {detection.count} bird(s) - State: {self.state.current_state.name}")
```

**Result:** Flooded with logs at ~30fps!

```
2025-12-24 06:23:41 | INFO | 🦅 DETECTED 1 bird(s) - State: visiting
2025-12-24 06:23:41 | INFO | 🦅 DETECTED 1 bird(s) - State: visiting
2025-12-24 06:23:41 | INFO | 🦅 DETECTED 1 bird(s) - State: visiting
# ... hundreds of lines per second
```

**Realization:** System was working perfectly all along!

**Design Intent:**
- **Silent during continuous presence** (by design)
- Only logs **state transitions**:
  - `ARRIVED` - falcon enters frame
  - `DEPARTED` - falcon exits frame
  - `ROOSTING` - settled for extended period
  - `ACTIVITY_START/END` - movement during roost

**Configuration Reset:** Reverted to production settings:
```yaml
detection_confidence: 0.5
frame_interval: 3
```

---

### Lessons Learned

#### 1. Memory Allocation Order Matters

**Takeaway:** Lazy loading is great for startup time, but **allocation order** is critical on constrained systems.

**Pattern:**
- Load large models **first**
- Start memory-hungry processes **second**
- Don't rely on allocator to handle fragmentation

#### 2. Isolate Docker vs Code Issues

**Debugging Strategy:**
1. Test in Docker → crashes
2. Test natively on same system → crashes
3. **Conclusion:** Code bug, not Docker

**Saved Hours:** Don't tune Docker configs when it's a code issue.

#### 3. Silent Operation is a Feature

**Design Pattern:** Event-driven logging

- **Don't** log every frame/detection (noise)
- **Do** log state changes (signal)
- Use debug logging sparingly (verify, then remove)

**Result:** Clean production logs showing meaningful events only.

#### 4. Cross-Platform Considerations

**macOS != Linux** when it comes to:
- Memory allocators (macOS more forgiving)
- Resource constraints (dev machines != production)
- Library implementations (YOLO, OpenCV, FFmpeg)

**Best Practice:** Test on target deployment platform early!

#### 5. Git Commit Hygiene

**Initial Mistake:** Mashed 4 unrelated changes into one commit:
- Critical bug fix (6 lines)
- Env file consolidation (73 lines deleted, 7 added)
- Documentation (1,747 lines)
- Config changes (not even committed!)

**Fix:** Used `git reset --soft HEAD~1` and re-committed as:
1. `0c26fe1` - Bug fix (isolated)
2. `508f14a` - Env consolidation
3. `97851ba` - Documentation
4. `b81894d` - Config cleanup

**Takeaway:** One logical change per commit. Makes history readable and debugging easier.

---

### Current Status (As of Dec 24, 2025)

#### ✅ Native Deployment: WORKING

Fix validated on test VM (192.168.1.252) running natively:

```bash
cd /opt/services/kanyo-admin
python -m kanyo.detection.realtime_monitor --config data/nsw/config.yaml
# ✅ Runs successfully - no std::bad_alloc crash
```

**Commits:**
- `0c26fe1` - Critical bug fix (model pre-loading)
- `508f14a` - Env consolidation
- `97851ba` - Documentation
- `b81894d` - Config cleanup
- `2301380` - Updated documentation

**Tests:** 84/84 passing ✅

#### ⚠️ Docker Deployment: PENDING

**Status:** Image rebuild in progress (GitHub Actions)

**Docker Image Variants:**

The project builds **three** Docker images for different hardware:

1. **`ghcr.io/sageframe-no-kaji/kanyo-contemplating-falcons-dev:cpu`**
   - Software encoding only (libx264)
   - Works on any system
   - **Slower** clip compression
   - Use for: Testing, low-power devices

2. **`ghcr.io/sageframe-no-kaji/kanyo-contemplating-falcons-dev:vaapi`**
   - Intel Quick Sync Video (VA-API)
   - **Fastest** for Intel GPUs
   - Use for: Intel systems with iGPU (like your HP ProDesk)

3. **`ghcr.io/sageframe-no-kaji/kanyo-contemplating-falcons-dev:nvidia`**
   - NVIDIA NVENC hardware encoding
   - Use for: Systems with NVIDIA GPUs

**Next Steps:**

1. Wait for GitHub Actions build completion
2. Pull updated image to test VM
3. Deploy with docker-compose
4. Verify fix works in containers
5. Monitor for stability

**docker-compose.yml Configuration:**

```yaml
services:
  harvard:
    # Change image tag based on your hardware:
    # - :cpu    → software encoding (any system)
    # - :vaapi  → Intel GPU hardware encoding (recommended for HP ProDesk)
    # - :nvidia → NVIDIA GPU hardware encoding
    image: ghcr.io/sageframe-no-kaji/kanyo-contemplating-falcons-dev:vaapi
    container_name: kanyo-harvard
    # ... rest of config
```

**Hardware Encoding Detection:**

The system auto-detects available encoders:

```bash
# Check what's available in container
docker compose exec harvard python -m kanyo.utils.encoder --detect

# Expected output on Intel system:
# ✓ h264_vaapi (Intel Quick Sync)
```

---

### Docker Deployment Procedure (UNTESTED WITH FIX)

When GitHub Actions completes the rebuild:

#### 1. Verify Image Build

```bash
# Check GitHub Actions status
# https://github.com/sageframe-no-kaji/kanyo-contemplating-falcons-dev/actions

# Look for successful "Docker Build and Push" workflow
```

#### 2. Deploy to Test VM

```bash
# On test VM (192.168.1.252)
cd /opt/services/kanyo-admin

# Pull new image with fix
sudo docker compose pull

# Restart containers
sudo docker compose up -d

# Verify both streams running
sudo docker compose ps
# Expected:
#   kanyo-harvard   Up
#   kanyo-nsw       Up
```

#### 3. Validate Fix in Containers

```bash
# Check Harvard stream logs
sudo docker compose logs -f harvard

# Look for these log lines in order:
# 1. "Pre-loading YOLO model..."
# 2. "✅ Model loaded successfully"
# 3. "✅ Connected to tee proxy"
# 4. "Starting falcon detection..."

# If crash occurs:
# - Check memory: docker stats
# - Check logs: docker compose logs harvard
# - Fall back to native deployment
```

#### 4. Monitor for Stability

```bash
# Watch both containers
watch -n 5 'sudo docker compose ps'

# Check resource usage
docker stats kanyo-harvard kanyo-nsw

# Monitor logs for state transitions
sudo docker compose logs -f | grep -E "(ARRIVED|DEPARTED|ROOSTING)"

# Verify clips being created
ls -lh /opt/services/kanyo-harvard/clips/$(date +%Y-%m-%d)/
```

#### 5. Production Validation Checklist

- [ ] Both containers stay running (24+ hours)
- [ ] No std::bad_alloc crashes
- [ ] State transitions logged correctly
- [ ] Clips created in expected directories
- [ ] Telegram notifications received
- [ ] Memory usage stable (<4GB per container)
- [ ] CPU usage reasonable (<50% avg)

---

### Troubleshooting Quick Reference

| Symptom | Likely Cause | Solution |
|---------|--------------|----------|
| std::bad_alloc crash | Memory allocation order | Ensure YOLO loads before FFmpeg |
| Container exits immediately | Model not pre-loaded | Check logs for "Pre-loading YOLO model..." |
| No detection logs | Working correctly! | Only state transitions logged |
| Excessive logging | Debug logging enabled | Remove debug `logger.info()` calls |
| Docker build fails | GitHub Actions issue | Check Actions tab on GitHub |
| Image pull fails | Not logged into GHCR | `docker login ghcr.io` |

---

## Success Criteria

### Phase 1: Containerization ✓

- [ ] Dockerfile builds successfully
- [ ] Image contains code (not bind mounted)
- [ ] Container runs locally on Mac
- [ ] Only data bind mounted (clips, logs, config)

### Phase 2: Multi-Stream ✓

- [ ] docker-compose.yml defines 2+ streams
- [ ] Each stream has separate ZFS dataset
- [ ] Bind mounts point to ZFS-backed paths
- [ ] Code is in image, data is bind mounted

### Phase 3: Tier 1 Deployment ✓

- [ ] Deployed to HP ProDesk with relative paths
- [ ] 2 streams running 24+ hours
- [ ] Notifications working

### Phase 4: Tier 2 Production ✓

- [ ] ZFS datasets created (admin + streams)
- [ ] Admin has code/compose/web state
- [ ] Streams have clips/logs/configs
- [ ] Compression enabled (>1.2x ratio)
- [ ] Daily snapshots working
- [ ] Can restore from snapshot
- [ ] Admin and stream data independent

### Phase 5: Web UI (Optional) ✓

- [ ] Web UI accessible
- [ ] UI state persists in kanyo-admin ZFS
- [ ] Can manage streams from browser

---

## What You Learned

### Docker Concepts

✅ **Images vs Containers**
- Images = code baked in (immutable)
- Containers = running instances
- Update code = rebuild image

✅ **Volumes (Bind Mounts)**
- Only mount persistent data
- Don't mount code (it's in the image)
- ZFS datasets as bind mount sources

✅ **docker-compose**
- Orchestrate multiple containers
- Environment variables for paths
- Two-tier deployment (dev vs prod)

### Infrastructure Skills

✅ **ZFS Integration**
- Separate datasets for admin vs streams
- Independent snapshots
- Compression, quotas
- Auditability

✅ **Data Governance**
- Admin: code + config + web state
- Streams: clips + logs + stream config
- Clear separation of concerns
- Independent lifecycle

### Professional Patterns

✅ **Immutable Infrastructure**
- Code in images (immutable)
- Data on ZFS (mutable, snapshotted)
- Rebuild to update code
- Rollback data with snapshots

✅ **Production Operations**
- Multi-container orchestration
- Independent stream management
- Snapshot-based backups
- Quota-based capacity planning

---

## Next Steps

### Week 1

1. Monitor stability (7 days)
2. Tune resources
3. Test snapshot restore
4. Document issues

### Month 1

1. Add 3rd stream
2. Automate clip cleanup
3. Add monitoring alerts
4. Share web UI access

### Future

1. Scale to 8 streams
2. Advanced features:
   - Clip highlights
   - Time-lapses
   - Analytics
3. Multi-host deployment
4. Cloud backup

---

## Resources

- **Docker:** https://docs.docker.com/
- **docker-compose:** https://docs.docker.com/compose/
- **ZFS:** https://openzfs.github.io/openzfs-docs/
- **Kanyō Project:** [Your GitHub repo]

---

**Congratulations!** 🎉

You've built a production-grade, multi-container, ZFS-backed falcon detection system with:
- Immutable code (in images)
- Persistent data (on ZFS)
- Independent management (admin vs streams)
- Professional operations (snapshots, quotas, compression)

This is how real infrastructure is built.

---

*End of Ho-04*
