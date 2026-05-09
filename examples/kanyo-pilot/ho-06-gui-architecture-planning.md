# Ho 6: GUI Architecture & Planning

**Date:** 2025-12-27 **Status:** Planning **Objective:** Design and implement frontend interfaces for Kanyō falcon monitoring system

---

## Overview

Kanyō needs three distinct frontend components to serve different audiences and use cases:

|Frontend|Audience|Purpose|Deployment|
|---|---|---|---|
|**Admin**|You (operator)|System management, config editing, monitoring|Local (Docker)|
|**Local Viewer**|You + trusted users|Watch clips, browse timeline, view live streams|Local (Docker)|
|**Public Viewer**|Anyone|Same as Local + comments, subscriptions|Cloudflare Pages|

This document captures the architecture decisions, technology choices, and implementation plan.

---

## Part 1: Requirements Analysis

### Admin Frontend Requirements

**Must Have:**

- View all configured streams with status (running/stopped)
- Edit stream configuration (all config.yaml parameters)
- Restart/stop/start individual stream containers
- Browse clips and stills organized by date
- View recent logs with live tail capability
- Add new streams to the system

**Nice to Have:**

- GPU utilization monitoring
- Container resource stats (CPU, memory)
- Bulk operations (restart all, stop all)
- Config validation before save (timing constraints)
- Config diff view before applying changes

### Local Viewer Requirements

**Must Have:**

- Overview of all streams with thumbnails and activity summary
- Per-stream detail view with:
    - HKSV-style timeline (day bar with activity indicators)
    - Video player (clips or YouTube live embed)
    - Event list (arrivals, departures, activity)
- Click timeline to jump to clip
- "Watch Live" button to embed YouTube stream
- Navigate between days

**Nice to Have:**

- Thumbnail previews on timeline hover
- Keyboard navigation (arrow keys for timeline)
- Filter events by type
- Search within date range

### Public Viewer Requirements

**Must Have:**

- Everything from Local Viewer
- Works without exposing any local ports
- Subscribe to notification feed (Telegram) per feed

**Future (not in initial release):**

- Comment on individual clips (scientific annotations)
- User accounts (optional)

---

## Part 2: Architecture Decision

### The Core Question: Shared vs. Separate Codebases

**Option A: Three Separate Apps**

```
kanyo-admin/web-admin/     → FastAPI + Jinja2
kanyo-admin/web-viewer/    → React app
cloudflare/viewer/         → Different React app
```

- ✅ Simple to reason about individually
- ✅ Can optimize each for its use case
- ❌ Duplication of timeline, clip viewer, etc.
- ❌ Harder to keep consistent

**Option B: One Codebase, Multiple Deployments**

```
kanyo-admin/web/           → Single React app with feature flags
```

- ✅ Shared components (timeline, player)
- ✅ Consistent design
- ❌ Complex build configuration
- ❌ Admin features bundled even in public build

**Option C: Hybrid - Admin separate, Local+Remote share** ✅ CHOSEN

```
kanyo-admin/web-admin/     → FastAPI + Jinja2 + HTMX (ops-focused)
kanyo-admin/web-viewer/    → React app (viewer-focused)
cloudflare/                → Same React app, different API endpoint
```

- ✅ Admin has different needs (config editing, Docker control)
- ✅ Local and Remote are the same viewer with different data access
- ✅ Admin ships faster (no build step with HTMX)
- ✅ Viewer gets React interactivity for timeline

### Why This Split Makes Sense

**Admin is fundamentally different:**

- Needs Docker socket access (security-sensitive)
- Config editing is forms, not fancy UI
- Log viewing is text, not visual
- Operator audience (you) vs. viewer audience (anyone)

**Local and Remote viewers are identical:**

- Same timeline component
- Same clip player
- Same event list
- Only difference: where data comes from

---

## Part 3: Technology Choices

### Admin Frontend: FastAPI + Jinja2 + HTMX

**Why FastAPI?**

- Already Python-native (matches Kanyō codebase)
- Async support for log streaming (WebSockets)
- Built-in OpenAPI docs (useful for debugging)
- Easy Docker SDK integration
- You already understand Python

**Why Jinja2 templates (not React)?**

- No build step = faster iteration
- Server-side rendering = simpler mental model
- Config forms don't need SPA complexity
- Admin UI is ops tool, not user-facing product

**Why HTMX?**

- SPA-like interactivity without JavaScript framework
- Partial page updates (edit config without full reload)
- Works with Jinja2 templates naturally
- Small learning curve, big productivity boost

**HTMX Example:**

```html
<!-- Restart button that updates status without page reload -->
<button hx-post="/api/streams/harvard/restart" 
        hx-target="#harvard-status"
        hx-swap="innerHTML">
  Restart
</button>
<span id="harvard-status">🟢 Running</span>
```

### Viewer Frontend: React + Vite

**Why React?**

- Timeline needs real interactivity (scrubbing, hover previews)
- Large ecosystem of video players, timeline components
- Easy to deploy to Cloudflare Pages
- Industry standard = lots of resources

**Why Vite (not Create React App)?**

- Much faster dev server (instant HMR)
- Smaller production bundles
- Modern defaults (ES modules)
- CRA is deprecated/maintenance mode

**Why not Vue/Svelte/etc?**

- React has more timeline/video components available
- Larger community for troubleshooting
- You'll find more examples for HKSV-style UI

### Data Layer: Unified API

Both Admin and Viewer talk to the same FastAPI backend:

```
┌─────────────────────────────────────────────────────────────┐
│                     FastAPI Backend                         │
├─────────────────────────────────────────────────────────────┤
│  /api/streams/*        → Stream management, clips, events   │
│  /api/admin/*          → Docker control, config editing     │
│  /                     → Serves Admin UI (Jinja2)           │
│  /viewer/*             → Serves React viewer (static)       │
└─────────────────────────────────────────────────────────────┘
```

### Cloudflare Deployment: R2 + Pages + Workers

**The Data Problem:** Clips and images live on your local machine. To serve them publicly without exposing ports:

**Solution: Sync to Cloudflare R2**

```
Local Machine                           Cloudflare
┌──────────────┐                       ┌──────────────┐
│ clips/       │ ──── sync (cron) ───▶ │ R2 Bucket    │
│ metadata.json│                       │ clips/       │
└──────────────┘                       │ metadata.json│
                                       └──────────────┘
                                              │
                                              ▼
                                       ┌──────────────┐
                                       │ Pages        │
                                       │ (React app)  │
                                       └──────────────┘
```

**R2 Benefits:**

- S3-compatible (easy tooling)
- Generous free tier (10GB storage, 10M reads/month)
- Same Cloudflare ecosystem as Pages
- No egress fees (unlike AWS S3)

**Sync Strategy:**

- Cron job runs every 15 minutes
- Syncs new clips + metadata JSON
- Uses `rclone` or custom Python script
- Only uploads changed files (checksums)

---

## Part 4: Data Model for Frontend

### Stream Registry

Rather than parsing docker-compose.yml, maintain a simple registry:

```yaml
# web-admin/streams.yaml
streams:
  harvard:
    name: "Harvard Peregrine Cam"
    youtube_id: "glczTFRRAK4"
    data_path: "/data/harvard"      # Mounted path inside admin container
    container_name: "kanyo-harvard-gpu"
    timezone: "-05:00"
    enabled: true
    
  nsw:
    name: "NSW Falcon Cam"  
    youtube_id: "yv2RtoIMNzA"
    data_path: "/data/nsw"
    container_name: "kanyo-nsw-gpu"
    timezone: "+10:00"
    enabled: true
```

### API Response Shapes

**Stream List:**

```json
{
  "streams": [
    {
      "id": "harvard",
      "name": "Harvard Peregrine Cam",
      "status": "running",
      "youtube_id": "glczTFRRAK4",
      "today_visits": 5,
      "last_event": "2025-12-27T07:45:00-05:00",
      "last_event_type": "departure",
      "thumbnail": "/api/streams/harvard/thumbnail"
    }
  ]
}
```

**Stream Detail:**

```json
{
  "id": "harvard",
  "name": "Harvard Peregrine Cam",
  "status": "running",
  "container_id": "fdd822f4ac0a",
  "uptime": "9h 23m",
  "config": {
    "video_source": "https://youtube.com/watch?v=glczTFRRAK4",
    "detection_confidence": 0.5,
    "exit_timeout": 300,
    "roosting_threshold": 1800
    // ... full config
  },
  "stats": {
    "today_visits": 5,
    "total_clips": 47,
    "storage_used": "2.3 GB"
  }
}
```

**Timeline Events:**

```json
{
  "stream_id": "harvard",
  "date": "2025-12-27",
  "timezone": "-05:00",
  "events": [
    {
      "time": "07:23:15",
      "timestamp": "2025-12-27T07:23:15-05:00",
      "type": "arrival",
      "clip": "falcon_072315_arrival.mp4",
      "thumbnail": "falcon_072315_arrival.jpg",
      "duration": null
    },
    {
      "time": "07:45:30",
      "timestamp": "2025-12-27T07:45:30-05:00",
      "type": "departure",
      "clip": "falcon_074530_departure.mp4",
      "thumbnail": "falcon_074530_departure.jpg",
      "visit_duration": 1335
    }
  ],
  "summary": {
    "total_events": 12,
    "arrivals": 6,
    "departures": 6,
    "total_presence_seconds": 14400
  }
}
```

**Clips List:**

```json
{
  "stream_id": "harvard",
  "date": "2025-12-27",
  "clips": [
    {
      "filename": "falcon_072315_arrival.mp4",
      "type": "arrival",
      "timestamp": "2025-12-27T07:23:15-05:00",
      "size_bytes": 4500000,
      "duration_seconds": 45,
      "thumbnail": "falcon_072315_arrival.jpg",
      "url": "/api/clips/harvard/2025-12-27/falcon_072315_arrival.mp4"
    }
  ]
}
```

---

## Part 5: UI Design

### Design Principles

1. **Clean, not cluttered** - HKSV is the reference, not a generic dashboard
2. **Information hierarchy** - Most important info visible first
3. **Responsive actions** - Buttons give immediate feedback
4. **Consistent patterns** - Same interactions everywhere

### Color Palette

```
Background:     #1a1a1a (dark gray)
Surface:        #2d2d2d (card backgrounds)
Primary:        #3b82f6 (blue - arrivals, active elements)
Danger:         #ef4444 (red - departures, stop buttons)
Warning:        #f59e0b (amber - activity, warnings)
Success:        #22c55e (green - running status)
Text Primary:   #ffffff
Text Secondary: #a1a1aa
```

### Admin UI Wireframes

See [ho-06.1-admin-gui.md](https://claude.ai/chat/ho-06.1-admin-gui.md) for detailed wireframes.

### Viewer UI Wireframes

**Overview Page:**

```
┌─────────────────────────────────────────────────────────────────┐
│  KANYO                                    [Harvard ▼]  [Admin]  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │   Harvard       │  │   NSW           │  │   + Add Stream  │ │
│  │   [thumbnail]   │  │   [thumbnail]   │  │                 │ │
│  │   ● 3 visits    │  │   ○ No activity │  │                 │ │
│  │   Last: 2h ago  │  │   Last: 6h ago  │  │                 │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Stream Detail (HKSV-inspired):**

```
┌─────────────────────────────────────────────────────────────────┐
│  KANYO  ←  Harvard Falcon Cam                          [Admin]  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────   ┌──────────────────┐  │
│  │                                     │  │  TODAY           │  │
│  │         [Video Player]              │  │  ────────────────│  │
│  │         (clip or live)              │  │  07:23 🔵 Arrival │ │
│  │                                     │  │  07:45 🔴 Depart  │ │
│  │                                     │  │  09:12 🔵 Arrival │ │
│  └─────────────────────────────────────┘  │  ...             │ │
│                                           │                  │ │
│  ◀ Dec 26  ──────────────────── Dec 27 ▶  │  [Watch Live →]  │ │
│  ░░░░░▓▓░░░░░░░░░▓░░░░░░░░░░░░░░░░░░░░░   └──────────────────┘ │
│       ^                                                        │
│    [7:23 AM - Arrival]                                         │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

**Timeline Component Detail:**

```
Timeline bar represents 24 hours (midnight to midnight)
├── Gray background: no activity
├── Blue segments: falcon present (visiting/roosting)
├── Yellow dots: activity events (arrival/departure)
├── Playhead: current viewing position
└── Hover: shows thumbnail preview + timestamp

Interaction:
├── Click: jump to that time, play relevant clip
├── Drag: scrub through timeline
├── Scroll: zoom in/out (optional)
└── Arrow keys: prev/next event
```

---

## Part 6: Implementation Phases

### Phase 1: Admin Backend + Basic UI (Ho 6.1)

**Goal:** Working admin interface for daily operations **Deliverables:**

- FastAPI backend with all endpoints
- Stream list with status
- Config viewing and editing
- Container restart/stop/start
- Basic clip browsing
- Log viewer

### Phase 2: Timeline Component (Ho 6.2)

**Goal:** HKSV-style timeline for Admin detail view **Deliverables:**

- Timeline bar component (vanilla JS or small lib)
- Event markers on timeline
- Click to view clip
- Day navigation

### Phase 3: React Viewer (Ho 6.3)

**Goal:** Standalone viewer app **Deliverables:**

- React + Vite project setup
- Overview page with stream cards
- Detail page with timeline + player
- YouTube live embed

### Phase 4: Cloudflare Deployment (Ho 6.4)

**Goal:** Public viewer on Cloudflare **Deliverables:**

- R2 bucket setup
- Sync script for clips
- Metadata export for events
- Pages deployment
- Worker for dynamic bits (optional)

---

## Part 7: File Structure

### Final Structure

```
kanyo-admin/
├── docker-compose.yml              # Existing - add admin service
├── .env                            # Stream paths
│
├── web/                            # Admin service
│   ├── Dockerfile
│   ├── requirements.txt
│   ├── streams.yaml                # Stream registry
│   │
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py                 # FastAPI app entry
│   │   ├── config.py               # App settings
│   │   │
│   │   ├── routers/
│   │   │   ├── __init__.py
│   │   │   ├── streams.py          # /api/streams/*
│   │   │   ├── clips.py            # /api/clips/*
│   │   │   ├── admin.py            # /api/admin/* (docker ops)
│   │   │   └── pages.py            # HTML pages (Jinja2)
│   │   │
│   │   ├── services/
│   │   │   ├── __init__.py
│   │   │   ├── docker_service.py   # Container management
│   │   │   ├── config_service.py   # YAML read/write
│   │   │   ├── clip_service.py     # Clip listing/serving
│   │   │   └── log_service.py      # Log reading/streaming
│   │   │
│   │   ├── templates/              # Jinja2 templates
│   │   │   ├── base.html
│   │   │   ├── overview.html
│   │   │   ├── stream_detail.html
│   │   │   ├── stream_config.html
│   │   │   ├── stream_logs.html
│   │   │   └── components/
│   │   │       ├── stream_card.html
│   │   │       ├── timeline.html
│   │   │       ├── clip_grid.html
│   │   │       └── config_form.html
│   │   │
│   │   └── static/
│   │       ├── css/
│   │       │   └── style.css
│   │       └── js/
│   │           ├── timeline.js
│   │           └── htmx.min.js
│   │
│   └── viewer/                     # React viewer (Phase 3)
│       ├── package.json
│       ├── vite.config.js
│       ├── index.html
│       └── src/
│           ├── App.jsx
│           ├── pages/
│           │   ├── Overview.jsx
│           │   └── StreamDetail.jsx
│           └── components/
│               ├── Timeline.jsx
│               ├── VideoPlayer.jsx
│               └── EventList.jsx
│
├── scripts/
│   └── sync_to_r2.py               # Cloudflare sync (Phase 4)
│
└── cloudflare/                     # Cloudflare config (Phase 4)
    ├── wrangler.toml
    └── worker/
        └── index.js
```

---

## Part 8: Success Criteria

### Ho 6.1 (Admin) Complete When:

- [ ] Can view all streams with status on overview page
- [ ] Can click into stream detail and see recent clips
- [ ] Can edit config and save (validation works)
- [ ] Can restart a stream from the UI
- [ ] Can view logs (at least last 100 lines)
- [ ] Docker Compose updated and admin container running

### Ho 6.2 (Timeline) Complete When:

- [ ] Timeline bar shows events for selected day
- [ ] Can click event to view clip
- [ ] Can navigate between days
- [ ] Hover shows time indicator

### Ho 6.3 (Viewer) Complete When:

- [ ] React app builds and runs
- [ ] Overview shows all streams
- [ ] Detail page has working timeline
- [ ] YouTube embed works for live view

### Ho 6.4 (Cloudflare) Complete When:

- [ ] R2 bucket created and accessible
- [ ] Sync script successfully uploads clips
- [ ] Pages deployment works
- [ ] Public URL accessible and functional

---

## Part 9: Questions Resolved

### Q: Docker vs. Baremetal management?

**A:** Admin UI manages Docker containers. This is the right choice because:

- Your deployment IS Docker-based
- Docker SDK makes container ops trivial
- Baremetal would require SSH/systemd complexity
- Container approach is portable

### Q: Auth for Admin?

**A:** Not priority for Phase 1. Add HTTP Basic Auth later (5 lines of FastAPI code). For now, it's local-only and you're the only user.

### Q: How to handle config validation?

**A:** Reuse existing `_validate()` function from `kanyo.utils.config`. Call it before saving and show errors in UI.

### Q: Live log streaming?

**A:** WebSocket endpoint that tails the log file. FastAPI + `aiofiles` makes this straightforward.

### Q: Timeline library or custom?

**A:** Start custom (simple div positioning), enhance later. HKSV timeline is simpler than it looks - just positioned elements on a time axis.

---

## Appendix: Reference Material

### HKSV Timeline Reference

The HomeKit Secure Video timeline (see screenshot) has these key elements:

1. **Scrubber bar** at bottom spanning the day
2. **Thumbnail strip** showing activity moments
3. **Orange highlight** for currently selected moment
4. **"LIVE" button** on the right
5. **Date/time** display in center
6. **Camera selector** dropdown at top

We'll adapt this for Kanyō with:

- Colored segments instead of thumbnail strip (simpler)
- Event type indicators (arrival/departure icons)
- Same "LIVE" button concept
- Day navigation arrows

### Useful Libraries

**Admin (Python):**

- `docker` - Docker SDK for Python
- `pyyaml` - YAML parsing
- `aiofiles` - Async file operations
- `python-multipart` - Form handling

**Viewer (React):**

- `react-player` - Video playback
- `date-fns` - Date manipulation
- `tailwindcss` - Styling
- `lucide-react` - Icons

### Related Documentation

- [[sensing-logic]] - Detection pipeline
- [[ho-04-docker-deploy]] - Docker setup
- [[ho-05-deployment-verification]]- Current deployment