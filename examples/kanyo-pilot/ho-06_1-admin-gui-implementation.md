# Ho 6.1: Admin GUI Implementation

**Date:** 2025-12-27 **Status:** Ready to Build **Prerequisite:** [[ho-06-gui-architecture-planning]] (architecture decisions) **Objective:** Build working Admin UI for stream management

---

## Overview

This Ho delivers a fully functional Admin interface that lets you:

- See all streams at a glance
- Edit configurations
- Restart containers
- Browse clips and logs

**Time Estimate:** 3-4 hours **Tech Stack:** FastAPI + Jinja2 + HTMX + Tailwind CSS

---

## Development Workflow

**Development:** Mac (local repo) **Production:** shingan.lan (Docker deployment)

```
Mac (development)                    shingan (production)
─────────────────                    ────────────────────
kanyo-contemplating-falcons-dev/
├── src/kanyo/        ←── Detection code (existing)
├── admin/            ←── Admin GUI (NEW)
│   └── web/
│
└── git push ──────────────────────► git pull
                                     docker compose up
```

**Why separate `admin/` from `src/kanyo/`?**

|Component|Purpose|Docker Image|Dependencies|
|---|---|---|---|
|Detection (`src/kanyo/`)|Watch streams, detect falcons|GPU, YOLO, ffmpeg|Heavy|
|Admin (`admin/`)|Manage containers, view clips|Docker socket, web framework|Light|

They don't share code. Admin talks to Docker and reads files — it doesn't import detection code.

---

## Part 1: Technology Choices

### Why FastAPI?

FastAPI is a modern Python web framework:

```python
from fastapi import FastAPI
app = FastAPI()

@app.get("/api/streams")
async def list_streams():
    return {"streams": [...]}
```

**Key Benefits:**

1. **Async native** - Log streaming via WebSockets is trivial
2. **Type hints** - Pydantic models catch errors early
3. **Auto-docs** - `/docs` gives you Swagger UI for free
4. **Python ecosystem** - Docker SDK, PyYAML, etc. just work
5. **Fast** - Performance comparable to Node.js/Go

### Why Jinja2 Templates?

Jinja2 generates HTML on the server:

```html
{% for stream in streams %}
  <div class="stream-card">
    <h3>{{ stream.name }}</h3>
    <span class="status">{{ stream.status }}</span>
  </div>
{% endfor %}
```

**Key Benefits:**

1. **No build step** - Edit template, refresh browser, see changes
2. **Server-side rendering** - Works without JavaScript
3. **FastAPI integration** - First-class support via `Jinja2Templates`

### Why HTMX?

HTMX adds AJAX behavior with HTML attributes:

```html
<button hx-post="/api/streams/harvard/restart" 
        hx-target="#harvard-status"
        hx-swap="innerHTML">
  Restart
</button>
```

**Key Benefits:**

1. **SPA-like feel** without JavaScript framework
2. **Small** - 14KB minified
3. **Server-driven** - Logic stays in Python

### Why Tailwind CSS?

Utility classes for styling:

```html
<div class="p-4 bg-zinc-800 rounded-lg">...</div>
```

**For Admin UI:** Tailwind via CDN (no build step):

```html
<script src="https://cdn.tailwindcss.com"></script>
```

---

## Part 2: Stream Discovery

The admin GUI **auto-discovers streams** by scanning `/data/*/config.yaml`. No separate registry needed.

### How It Works

```
/data/
├── harvard/
│   ├── config.yaml    ← Admin reads this
│   └── clips/
└── nsw/
    ├── config.yaml    ← Admin reads this
    └── clips/
```

Admin scans `/data/`, finds folders, reads each `config.yaml` to get:

- `stream_name` — Display name for UI
- `video_source` — YouTube URL
- `timezone` — For timestamps
- Everything else needed

### Required Config Field

Each stream's `config.yaml` must have:

```yaml
stream_name: "Harvard Falcon Cam"  # Human-readable name for admin UI
```

### Container Name Convention

Admin derives container name from folder:

```
/data/harvard/  →  kanyo-harvard-gpu
/data/nsw/      →  kanyo-nsw-gpu
```

### Adding a New Stream

1. Deploy detection container with config at `/opt/services/kanyo-{name}/`
2. Ensure `config.yaml` has `stream_name` field
3. Add volume mount to admin's `docker-compose.yml`:
    
    ```yaml
    - /opt/services/kanyo-{name}:/data/{name}:ro
    ```
    
4. Restart admin — new stream appears automatically

---

## Part 3: Project Structure

```
kanyo-contemplating-falcons-dev/
├── src/kanyo/                      # Detection (existing)
├── configs/                        # Stream configs (existing)
├── docker/                         # Detection Dockerfiles (existing)
│
└── admin/                          # Admin GUI (NEW)
    ├── docker-compose.yml
    ├── .env
    └── web/
        ├── Dockerfile
        ├── requirements.txt
        │
        └── app/
            ├── __init__.py
            ├── main.py             # FastAPI app
            ├── config.py           # Settings
            │
            ├── routers/
            │   ├── __init__.py
            │   ├── api.py          # JSON API endpoints
            │   └── pages.py        # HTML page routes
            │
            ├── services/
            │   ├── __init__.py
            │   ├── docker_service.py
            │   ├── config_service.py
            │   ├── clip_service.py
            │   └── stream_service.py
            │
            ├── templates/
            │   ├── base.html
            │   ├── overview.html
            │   ├── stream/
            │   │   ├── detail.html
            │   │   ├── config.html
            │   │   └── logs.html
            │   └── components/
            │       ├── stream_card.html
            │       ├── clip_grid.html
            │       ├── event_list.html
            │       ├── config_form.html
            │       └── log_viewer.html
            │
            └── static/
                ├── css/
                │   └── custom.css
                └── js/
                    └── app.js
```

---

## Part 4: Wireframes

### Overview Page (`/`)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                                                                              │
│   🦅 KANYO ADMIN                                            shingan.lan     │
│                                                                              │
│   STREAMS                                                                    │
│   ┌────────────────────────────────┐  ┌────────────────────────────────┐    │
│   │  ┌──────────────────────────┐  │  │  ┌──────────────────────────┐  │    │
│   │  │    [Latest Thumbnail]    │  │  │  │    [Latest Thumbnail]    │  │    │
│   │  └──────────────────────────┘  │  │  └──────────────────────────┘  │    │
│   │                                │  │                                │    │
│   │  Harvard Peregrine Cam         │  │  NSW Falcon Cam                │    │
│   │  🟢 Running                    │  │  🟢 Running                    │    │
│   │                                │  │                                │    │
│   │  Today: 5 visits               │  │  Today: 2 visits               │    │
│   │  Last event: 2h ago (depart)   │  │  Last event: 30m ago (arrive)  │    │
│   │                                │  │                                │    │
│   │  ┌────────┐ ┌────────┐ ┌────┐  │  │  ┌────────┐ ┌────────┐ ┌────┐  │    │
│   │  │ View   │ │ Config │ │ ⟳  │  │  │  │ View   │ │ Config │ │ ⟳  │  │    │
│   │  └────────┘ └────────┘ └────┘  │  │  └────────┘ └────────┘ └────┘  │    │
│   └────────────────────────────────┘  └────────────────────────────────┘    │
│                                                                              │
│   SYSTEM STATUS                                                              │
│   ┌──────────────────────────────────────────────────────────────────────┐  │
│   │  GPU: NVIDIA RTX 3050  │  325 MB / 8192 MB  │  Containers: 2 running │  │
│   └──────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

### Stream Detail Page (`/streams/{id}`)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                                                                              │
│   ← Back    Harvard Peregrine Cam                     🟢 Running    ⟳  ⏹    │
│                                                                              │
│   ┌─────────────────────────────────────────────┐  ┌───────────────────────┐│
│   │                                             │  │  EVENTS - Dec 27      ││
│   │              [Video Player]                 │  │  ─────────────────────││
│   │                                             │  │  07:23  🔵 Arrival    ││
│   │                                             │  │  07:45  🔴 Departure  ││
│   └─────────────────────────────────────────────┘  │  09:12  🔵 Arrival    ││
│                                                    │  11:30  🔴 Departure  ││
│   ◀ Dec 26  ════════════════════════════ Dec 27 ▶  │                       ││
│   ░░░░░░▓▓▓▓░░░░░░░░░░▓▓░░░░░░░▓▓▓▓░░░░░░░░░░░░   │  [🔴 Watch Live]      ││
│                                                    └───────────────────────┘│
│   RECENT CLIPS                                                               │
│   ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐        │
│   │  arr   │ │  dep   │ │  arr   │ │ visit  │ │  arr   │ │  dep   │        │
│   │ 07:23  │ │ 07:45  │ │ 09:12  │ │ 09:12  │ │ 11:30  │ │ 11:45  │        │
│   └────────┘ └────────┘ └────────┘ └────────┘ └────────┘ └────────┘        │
│                                                                              │
│   ┌──────────┐  ┌──────────┐  ┌──────────┐                                  │
│   │ ⚙ Config │  │ 📋 Logs  │  │ 📁 Files │                                  │
│   └──────────┘  └──────────┘  └──────────┘                                  │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

### Config Editor Page (`/streams/{id}/config`)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│   ← Back to Harvard                                                          │
│                                                                              │
│   STREAM CONFIGURATION                                                       │
│   ════════════════════════════════════════════════════════════════════════   │
│                                                                              │
│   Stream & Detection                                                         │
│   ┌──────────────────────────────────────────────────────────────────────┐  │
│   │  video_source                                                        │  │
│   │  ┌────────────────────────────────────────────────────────────────┐  │  │
│   │  │ https://www.youtube.com/watch?v=glczTFRRAK4                    │  │  │
│   │  └────────────────────────────────────────────────────────────────┘  │  │
│   │                                                                      │  │
│   │  detection_confidence              frame_interval                    │  │
│   │  ┌─────────────────────────┐      ┌─────────────────────────┐       │  │
│   │  │ 0.35                    │      │ 3                       │       │  │
│   │  └─────────────────────────┘      └─────────────────────────┘       │  │
│   └──────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│   State Machine                                                              │
│   ┌──────────────────────────────────────────────────────────────────────┐  │
│   │  exit_timeout                      roosting_threshold                │  │
│   │  ┌─────────────────────────┐      ┌─────────────────────────┐       │  │
│   │  │ 90                      │ sec  │ 1800                    │ sec   │  │
│   │  └─────────────────────────┘      └─────────────────────────┘       │  │
│   └──────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│   Notifications                                                              │
│   ┌──────────────────────────────────────────────────────────────────────┐  │
│   │  [✓] telegram_enabled                                                │  │
│   │  telegram_channel: @kanyo_harvard_falcon_cam                         │  │
│   └──────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│   ┌───────────────────┐                    ┌─────────────┐ ┌─────────────┐  │
│   │ View Raw YAML     │                    │   Cancel    │ │    Save     │  │
│   └───────────────────┘                    └─────────────┘ └─────────────┘  │
│                                            ┌─────────────────────────────┐  │
│                                            │ Save & Restart Stream       │  │
│                                            └─────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────────────┘
```

### Logs Page (`/streams/{id}/logs`)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│   ← Back to Harvard                                           [Clear] [⟳]   │
│                                                                              │
│   LOGS                                                                       │
│   ════════════════════════════════════════════════════════════════════════   │
│                                                                              │
│   Level: [INFO ▼]  Search: [________________] [🔍]                           │
│                                                                              │
│   ┌──────────────────────────────────────────────────────────────────────┐  │
│   │ 2025-12-27 07:23:15 INFO  🦅 FALCON ARRIVED at 07:23:15 AM           │  │
│   │ 2025-12-27 07:23:16 INFO  Creating arrival clip...                   │  │
│   │ 2025-12-27 07:23:18 INFO  ✅ Arrival clip complete                   │  │
│   │ 2025-12-27 07:23:18 INFO  📧 Telegram sent                           │  │
│   │ 2025-12-27 07:45:30 INFO  🦅 FALCON DEPARTED (22m 15s visit)         │  │
│   │ ...                                                                  │  │
│   └──────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│   [✓] Auto-scroll    [✓] Live updates                   Showing 100 lines   │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## Part 5: API Specification

### Stream Endpoints

```
GET  /api/streams                    → List all streams with status
GET  /api/streams/{id}               → Stream detail
PUT  /api/streams/{id}/config        → Update config.yaml
POST /api/streams/{id}/restart       → Restart container
POST /api/streams/{id}/stop          → Stop container
POST /api/streams/{id}/start         → Start container
```

### Clip Endpoints

```
GET  /api/streams/{id}/clips?date=   → List clips for date
GET  /api/streams/{id}/thumbnail     → Latest thumbnail
```

### Log Endpoints

```
GET  /api/streams/{id}/logs?lines=   → Recent log lines
WS   /api/streams/{id}/logs/stream   → WebSocket for live tail
```

### System Endpoints

```
GET  /api/system/status              → GPU, disk, container stats
GET  /api/system/health              → Health check
```

---

## Part 6: Implementation Steps

### Step 1: Project Setup (You do this on Mac)

```bash
# Navigate to kanyo repo
cd ~/Vaults/sageframe-no-kaji-dev/kanyo-contemplating-falcons-dev

# Create structure
mkdir -p admin/web/app/{routers,services}
mkdir -p admin/web/app/templates/{stream,components}
mkdir -p admin/web/app/static/{css,js}

# Create __init__.py files
touch admin/web/app/__init__.py
touch admin/web/app/routers/__init__.py
touch admin/web/app/services/__init__.py

# Create requirements.txt
cat > admin/web/requirements.txt << 'EOF'
fastapi==0.109.0
uvicorn[standard]==0.27.0
jinja2==3.1.3
python-multipart==0.0.6
pyyaml==6.0.1
docker==7.0.0
aiofiles==23.2.1
websockets==12.0
EOF

# No streams.yaml needed - admin auto-discovers from /data/*/config.yaml

# Create Dockerfile
cat > admin/web/Dockerfile << 'EOF'
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app/ ./app/

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "5000", "--reload"]
EOF

# Create docker-compose.yml (for LOCAL DEV ONLY - testing UI without real streams)
cat > admin/docker-compose.dev.yml << 'EOF'
# DEV ONLY - for testing admin UI locally on Mac
# Production: add admin service to main stack on shingan
services:
  admin:
    build: ./web
    container_name: kanyo-admin-web
    ports:
      - "5000:5000"
    volumes:
      - ./web/app:/app/app:ro
      # Mount test data (create fake config.yaml files for testing)
      - ./test-data:/data:ro
    environment:
      - KANYO_ENV=development
EOF

# Create test data structure for local dev
mkdir -p admin/test-data/harvard admin/test-data/nsw

# Create minimal test configs (just for UI testing)
cat > admin/test-data/harvard/config.yaml << 'EOF'
stream_name: "Harvard Falcon Cam (TEST)"
video_source: "https://www.youtube.com/watch?v=glczTFRRAK4"
timezone: "-05:00"
detection_confidence: 0.35
exit_timeout: 90
roosting_threshold: 1800
telegram_enabled: false
EOF

cat > admin/test-data/nsw/config.yaml << 'EOF'
stream_name: "NSW Falcon Cam (TEST)"
video_source: "https://www.youtube.com/watch?v=yv2RtoIMNzA"
timezone: "+11:00"
detection_confidence: 0.35
exit_timeout: 90
roosting_threshold: 1800
telegram_enabled: false
EOF

# Verify structure
tree admin/
```

### Step 2: Agent Builds the App

Give the agent instructions to create:

1. `admin/web/app/main.py` - FastAPI app
2. `admin/web/app/config.py` - Settings
3. `admin/web/app/services/*.py` - Docker, Config, Clip services
4. `admin/web/app/routers/*.py` - API and page routes
5. `admin/web/app/templates/*.html` - Jinja2 templates
6. `admin/web/app/static/css/custom.css` - Minimal styles
7. `admin/web/app/static/js/app.js` - Minimal JS

### Step 3: Test Locally (on Mac)

```bash
cd admin
docker compose -f docker-compose.dev.yml build
docker compose -f docker-compose.dev.yml up
# Visit http://localhost:5000
```

Note: Docker/container management won't work locally (no real kanyo containers on Mac). But you can test:

- Templates render correctly
- Stream cards show test data
- Navigation works
- Config forms display

### Step 4: Deploy to Production (shingan)

**Add admin service to your existing stack on shingan:**

```yaml
# Add to /opt/services/kanyo-stack/docker-compose.yml (or wherever your main stack is)

services:
  # ... existing harvard-gpu, nsw-gpu services ...

  admin:
    build: 
      context: /opt/services/kanyo-code/admin/web
    container_name: kanyo-admin-web
    ports:
      - "5000:5000"
    volumes:
      # Docker socket for container management
      - /var/run/docker.sock:/var/run/docker.sock
      # Stream data - admin auto-discovers from /data/*/config.yaml
      - /opt/services/kanyo-harvard:/data/harvard:ro
      - /opt/services/kanyo-nsw:/data/nsw:ro
    restart: unless-stopped
```

**Then:**

```bash
# On Mac
git add admin/
git commit -m "feat: add admin GUI"
git push

# On shingan
cd /opt/services/kanyo-code
git pull

# Add admin service to your stack and restart
docker compose up -d --build admin

# Visit http://shingan.lan:5000
```

---

## Part 7: Service Layer

### StreamService (Auto-Discovery)

```python
# services/stream_service.py
import yaml
from pathlib import Path

class StreamService:
    def __init__(self, data_path: Path = Path("/data")):
        self.data_path = data_path
    
    def discover_streams(self) -> list[dict]:
        """Auto-discover streams from /data/*/config.yaml"""
        streams = []
        
        for stream_dir in sorted(self.data_path.iterdir()):
            if not stream_dir.is_dir():
                continue
            
            config_path = stream_dir / "config.yaml"
            if not config_path.exists():
                continue
            
            with open(config_path) as f:
                config = yaml.safe_load(f)
            
            stream_id = stream_dir.name  # "harvard", "nsw"
            
            streams.append({
                "id": stream_id,
                "name": config.get("stream_name", stream_id.title()),
                "container_name": f"kanyo-{stream_id}-gpu",
                "config_path": str(config_path),
                "clips_path": str(stream_dir / "clips"),
                "video_source": config.get("video_source"),
                "timezone": config.get("timezone", "+00:00"),
                "telegram_channel": config.get("telegram_channel"),
            })
        
        return streams
    
    def get_stream(self, stream_id: str) -> dict | None:
        """Get a single stream by ID."""
        for stream in self.discover_streams():
            if stream["id"] == stream_id:
                return stream
        return None
```

### DockerService

```python
# services/docker_service.py
import docker

class DockerService:
    def __init__(self):
        self.client = docker.from_env()
    
    def get_container_status(self, container_name: str) -> dict:
        try:
            container = self.client.containers.get(container_name)
            return {
                "status": container.status,
                "id": container.short_id,
            }
        except docker.errors.NotFound:
            return {"status": "not_found"}
    
    def restart_container(self, container_name: str) -> bool:
        container = self.client.containers.get(container_name)
        container.restart(timeout=30)
        return True
    
    def get_logs(self, container_name: str, lines: int = 100) -> str:
        container = self.client.containers.get(container_name)
        return container.logs(tail=lines).decode()
```

### ConfigService

```python
# services/config_service.py
import yaml
from pathlib import Path

class ConfigService:
    def __init__(self, base_path: Path):
        self.base_path = base_path
    
    def read_config(self, stream_id: str) -> dict:
        config_path = self.base_path / stream_id / "config.yaml"
        with open(config_path) as f:
            return yaml.safe_load(f)
    
    def write_config(self, stream_id: str, config: dict) -> None:
        config_path = self.base_path / stream_id / "config.yaml"
        with open(config_path, 'w') as f:
            yaml.dump(config, f, default_flow_style=False)
    
    def validate_config(self, config: dict) -> list[str]:
        errors = []
        if not config.get("video_source"):
            errors.append("video_source is required")
        
        exit_timeout = config.get("exit_timeout", 90)
        roosting_threshold = config.get("roosting_threshold", 1800)
        
        if roosting_threshold <= exit_timeout:
            errors.append(f"roosting_threshold must be > exit_timeout")
        
        return errors
```

### ClipService

```python
# services/clip_service.py
from pathlib import Path
import re

class ClipService:
    def __init__(self, base_path: Path):
        self.base_path = base_path
    
    def list_clips(self, stream_id: str, date: str) -> list[dict]:
        clips_dir = self.base_path / stream_id / "clips" / date
        if not clips_dir.exists():
            return []
        
        clips = []
        for f in sorted(clips_dir.iterdir()):
            if f.suffix in ('.mp4', '.jpg'):
                info = self._parse_filename(f)
                if info:
                    clips.append(info)
        return clips
    
    def _parse_filename(self, path: Path) -> dict | None:
        pattern = r"falcon_(\d{6})_(\w+)\.(mp4|jpg)"
        match = re.match(pattern, path.name)
        if not match:
            return None
        
        time_str, event_type, ext = match.groups()
        return {
            "filename": path.name,
            "time": f"{time_str[:2]}:{time_str[2:4]}:{time_str[4:6]}",
            "type": event_type,
            "is_video": ext == "mp4",
        }
```

---

## Part 8: Templates

### Base Template

```html
<!-- templates/base.html -->
<!DOCTYPE html>
<html lang="en" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{% block title %}Kanyō Admin{% endblock %}</title>
  
  <!-- Tailwind CSS via CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            zinc: { 850: '#1f1f23' }
          }
        }
      }
    }
  </script>
  
  <!-- HTMX -->
  <script src="https://unpkg.com/htmx.org@1.9.10"></script>
  
  <link rel="stylesheet" href="/static/css/custom.css">
</head>
<body class="bg-zinc-900 text-white min-h-screen">
  {% block content %}{% endblock %}
  <script src="/static/js/app.js"></script>
</body>
</html>
```

### Stream Card Component

```html
<!-- templates/components/stream_card.html -->
<div class="bg-zinc-800 rounded-lg overflow-hidden" id="stream-{{ stream.id }}">
  <div class="aspect-video bg-zinc-700 relative">
    {% if stream.thumbnail %}
      <img src="{{ stream.thumbnail }}" class="w-full h-full object-cover">
    {% else %}
      <div class="absolute inset-0 flex items-center justify-center text-zinc-500">
        No recent clips
      </div>
    {% endif %}
  </div>
  
  <div class="p-4">
    <h3 class="font-semibold text-lg">{{ stream.name }}</h3>
    
    <div class="flex items-center gap-2 mt-1 text-sm">
      <span class="w-2 h-2 rounded-full 
        {% if stream.status == 'running' %}bg-green-500
        {% else %}bg-red-500{% endif %}"></span>
      <span class="text-zinc-400 capitalize">{{ stream.status }}</span>
    </div>
    
    <div class="mt-3 text-sm text-zinc-400">
      <div>Today: {{ stream.today_visits }} visits</div>
      <div>Last: {{ stream.last_event_ago }}</div>
    </div>
    
    <div class="flex gap-2 mt-4">
      <a href="/streams/{{ stream.id }}" 
         class="flex-1 bg-zinc-700 hover:bg-zinc-600 text-center py-2 rounded text-sm">
        View
      </a>
      <a href="/streams/{{ stream.id }}/config" 
         class="flex-1 bg-zinc-700 hover:bg-zinc-600 text-center py-2 rounded text-sm">
        Config
      </a>
      <button hx-post="/api/streams/{{ stream.id }}/restart"
              hx-target="#stream-{{ stream.id }}"
              hx-swap="outerHTML"
              class="bg-zinc-700 hover:bg-zinc-600 px-3 py-2 rounded text-sm">
        ⟳
      </button>
    </div>
  </div>
</div>
```

---

## Part 9: Success Criteria

### Minimum Viable (This Ho)

- [ ] Admin container builds and runs
- [ ] Overview page shows all streams with status
- [ ] Can click into stream detail
- [ ] Can view recent clips
- [ ] Can restart a stream from UI
- [ ] Can view logs (last 100 lines)

### Complete (Stretch)

- [ ] Config editor with validation
- [ ] Live log streaming via WebSocket
- [ ] Timeline bar on detail page
- [ ] System status (GPU, disk)

---

## Part 10: Testing

### Manual Checklist

1. **Overview Page**
    
    - [ ] Shows Harvard and NSW streams
    - [ ] Status indicators match `docker ps`
    - [ ] Thumbnails load
2. **Stream Detail**
    
    - [ ] Lists recent clips
    - [ ] Video plays when clicked
    - [ ] Events list populates
3. **Config Editor**
    
    - [ ] Loads current config
    - [ ] Save writes file
    - [ ] Save & Restart works
4. **Container Operations**
    
    - [ ] Restart button works
    - [ ] Status updates after restart
5. **Logs**
    
    - [ ] Shows recent lines
    - [ ] Level filter works

---

## Next Steps

After Ho 6.1:

1. **Ho 6.2** - HKSV-style timeline component
2. **Ho 6.3** - React viewer for public deployment
3. **Ho 6.4** - Cloudflare R2 sync and Pages deployment

---

_Document created: 2025-12-27_ _Updated: 2025-12-28 (corrected development workflow)_