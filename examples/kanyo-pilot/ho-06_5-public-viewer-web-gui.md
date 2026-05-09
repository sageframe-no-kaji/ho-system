# Ho 6.5: Public Viewer with HKSV-Style Timeline

**Date:** 2025-01-14 **Duration:** ~18 hours (5 days) **Status:** Ready to Build **Prerequisites:** Admin GUI running, clips being generated

---

## Overview

Build a public-facing viewer for Kanyō with an HKSV-inspired timeline interface. This is the "showcase" frontend where anyone can watch falcon activity, browse the timeline, and engage with the content.

**What We're Building:**

- Landing page with stream selector cards
- Stream viewer with HKSV-style thumbnail timeline
- Camera info panel (location, species, maintainer)
- Stats display (visits, arrivals, last event)
- Download and share buttons for clips
- About page with project info and collaboration CTA
- Simple anonymous comment system (prepared for future)

**Key Insight:** The HKSV thumbnail strip IS the video scrubber - thumbnails represent visit clips. Click a thumbnail → play that visit. Our adaptation uses event thumbnails (arrival/departure) positioned on a 24-hour timeline.

---

## Part 1: What You'll Learn

### Concepts Covered

- React + Vite project setup
- Tailwind CSS dark theme design
- Timeline component positioning (time → pixels)
- Video player integration (clips + YouTube embed)
- FastAPI static file serving
- Environment-based configuration (local vs production)

### Skills Practiced

- Component-based UI architecture
- Responsive design patterns
- API design for frontend consumption
- Docker multi-stage builds

---

## Part 2: Architecture

### System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                         User's Browser                          │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │                     React App (Vite)                      │  │
│  │  ┌─────────┐  ┌─────────────┐  ┌────────────────────────┐ │  │
│  │  │ Landing │  │ StreamView  │  │ About                  │ │  │
│  │  │  Page   │  │  + Timeline │  │  Page                  │ │  │
│  │  └─────────┘  └─────────────┘  └────────────────────────┘ │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      FastAPI Backend                            │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │ /api/streams │  │ /api/clips   │  │ /api/comments        │  │
│  │ (metadata)   │  │ (files)      │  │ (future)             │  │
│  └──────────────┘  └──────────────┘  └──────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                       File System                               │
│  /data/harvard/clips/2025-01-14/                                │
│  ├── falcon_072315_arrival.jpg    ← Thumbnail                   │
│  ├── falcon_072315_arrival.mp4    ← Arrival clip                │
│  ├── falcon_072315_visit.mp4      ← Full visit                  │
│  ├── falcon_074530_departure.jpg  ← Thumbnail                   │
│  └── events_2025-01-14.json       ← Event metadata              │
└─────────────────────────────────────────────────────────────────┘
```

### Deployment Options

| Method                | Setup                | Cost     | Best For                 |
| --------------------- | -------------------- | -------- | ------------------------ |
| **Local Dev**         | `npm run dev`        | Free     | Development              |
| **VPS**               | rsync clips + docker | $6-12/mo | Production (recommended) |
| **Cloudflare Tunnel** | cloudflared          | Free     | No DMZ yet               |
| **Direct (DMZ)**      | Port forward         | Free     | When DMZ ready           |

**Domain:** `kanyo.sageframe.net`

---

## Part 3: Data Model

### Stream Registry (streams.yaml)

```yaml
streams:
  harvard:
    # Technical (from existing config)
    name: 'Harvard Falcon Cam'
    youtube_id: 'glczTFRRAK4'
    container_name: 'kanyo-harvard-gpu'
    data_path: '/data/harvard'
    timezone: 'America/New_York'

    # Public-facing metadata (NEW)
    display:
      short_name: 'Harvard'
      location: 'Memorial Hall, Harvard University, Cambridge MA'
      coordinates: [42.3762, -71.1158]
      species: 'Peregrine Falcon (Falco peregrinus)'
      maintainer: 'Harvard Faculty of Arts & Sciences'
      maintainer_url: 'https://www.fas.harvard.edu/'
      description: >
        Active nest since 1999. Home to a breeding pair of peregrines
        nesting on the tower of Memorial Hall.
      telegram_channel: 'kanyo_harvard_falcon'

  nsw:
    name: 'NSW Falcon Cam'
    youtube_id: 'yv2RtoIMNzA'
    timezone: 'Australia/Sydney'

    display:
      short_name: 'NSW'
      location: 'Orange, New South Wales, Australia'
      coordinates: [-33.2833, 149.1000]
      species: 'Peregrine Falcon (Falco peregrinus)'
      maintainer: 'NSW Falcon Cams'
      maintainer_url: 'https://www.falconcams.org.au/'
      description: 'Monitoring peregrine falcons in rural NSW.'
      telegram_channel: 'kanyo_nsw_falcon'
```

### Events JSON (existing format)

```json
[
  {
    "id": "20250114_072315",
    "start_time": "2025-01-14T07:23:15-05:00",
    "end_time": "2025-01-14T07:45:30-05:00",
    "duration_seconds": 1335,
    "duration_str": "22m 15s",
    "peak_confidence": 0.847,
    "thumbnail_path": "falcon_072315_arrival.jpg",
    "arrival_clip_path": "falcon_072315_arrival.mp4",
    "departure_clip_path": "falcon_074530_departure.mp4"
  }
]
```

### API Endpoints

```
# Stream metadata
GET  /api/streams                     → List all streams with display info
GET  /api/streams/{id}                → Stream detail + today's stats

# Events for timeline
GET  /api/streams/{id}/events?date=   → Events for specific date
GET  /api/streams/{id}/stats?range=   → Stats (24h, 2d, 3d, etc.)

# Media serving
GET  /api/clips/{stream}/{date}/{file} → Serve thumbnail or video

# Comments (simple, future-ready)
GET  /api/streams/{id}/comments?date= → Get comments for date
POST /api/streams/{id}/comments       → Add comment
     Body: { event_id, text, display_name? }
```

---

## Part 4: UI Design

### Color Palette (HKSV-inspired dark theme)

```css
:root {
  --bg-primary: #000000; /* Pure black */
  --bg-secondary: #1c1c1e; /* Cards */
  --bg-tertiary: #2c2c2e; /* Elevated */
  --accent-orange: #ff9500; /* Timeline highlight, selected */
  --accent-blue: #0a84ff; /* Arrivals, links */
  --accent-red: #ff453a; /* Departures, live indicator */
  --accent-green: #30d158; /* Running status */
  --text-primary: #ffffff;
  --text-secondary: #8e8e93;
  --text-tertiary: #636366;
}
```

### Landing Page

```
┌────────────────────────────────────────────────────────────────────┐
│  🦅 KANYO                                        [About] [GitHub]  │
├────────────────────────────────────────────────────────────────────┤
│                                                                    │
│                    Automated Falcon Monitoring                     │
│                                                                    │
│      Computer vision meets ornithology. Watch peregrine falcons    │
│      on live nest cams with AI-powered activity detection.         │
│                                                                    │
│   ┌─────────────────────────┐   ┌─────────────────────────┐       │
│   │  ┌───────────────────┐  │   │  ┌───────────────────┐  │       │
│   │  │                   │  │   │  │                   │  │       │
│   │  │ [Harvard thumb]   │  │   │  │ [NSW thumb]       │  │       │
│   │  │                   │  │   │  │                   │  │       │
│   │  └───────────────────┘  │   │  └───────────────────┘  │       │
│   │                         │   │                         │       │
│   │  Harvard Falcon Cam     │   │  NSW Falcon Cam         │       │
│   │  📍 Cambridge, MA       │   │  📍 Orange, NSW         │       │
│   │  🟢 Live • 5 visits     │   │  🟢 Live • 12 visits    │       │
│   │                         │   │                         │       │
│   └─────────────────────────┘   └─────────────────────────┘       │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
```

### Stream View (Main Interface)

```
┌────────────────────────────────────────────────────────────────────────┐
│  🦅 KANYO    [Harvard Falcon Cam ▼]                   [About] [GitHub] │
├────────────────────────────────────────────────────────────────────────┤
│                                                                        │
│  ┌─────────────────────────────────────────────┐  ┌──────────────────┐│
│  │                                             │  │ Camera Info      ││
│  │                                             │  │ ────────────────-││
│  │                                             │  │ 📍 Memorial Hall ││
│  │           [Video Player]                    │  │    Cambridge, MA ││
│  │           16:9 aspect ratio                 │  │ 🦅 Peregrine     ││
│  │                                             │  │ 🎥 Harvard FAS   ││
│  │                                             │  │ 🕐 09:17 AM EST  ││
│  │                                             │  │                  ││
│  └─────────────────────────────────────────────┘  │ [🔔 Subscribe]   ││
│                                                   │                  ││
│  ┌─────────────────────────────────────────────┐  │──────────────────││
│  │ ◀ S  M  T  W  T  F  S ▶     7:23:15 AM     │  │ Stats (24h)      ││
│  │      13 [14] 15             Today           │  │ ────────────────-││
│  │                                             │  │ Visits: 10       ││
│  │ ══════[t]════════[t]═══[t]════════════ 🔴  │  │ Arrivals: 10     ││
│  │        ↑          ↑     ↑              LIVE│  │ Last: 08:43 dep  ││
│  │    (thumbnails positioned by time)         │  │                  ││
│  └─────────────────────────────────────────────┘  │ [24h][2d][3d]    ││
│                                                   └──────────────────┘│
│  ┌─────────────────────────────────────────────────────────────────┐  │
│  │ Selected Event: 07:23 AM - Arrival                              │  │
│  │ Duration: 22m 15s                                               │  │
│  │ [⬇ Download Clip] [🔗 Copy Link] [💬 Add Note]                  │  │
│  └─────────────────────────────────────────────────────────────────┘  │
│                                                                        │
└────────────────────────────────────────────────────────────────────────┘
```

### Timeline Component Detail

```
Week Calendar (top):
┌─────────────────────────────────────────┐
│  ◀   S   M   T   W   T   F   S   ▶     │
│      12  13 [14] 15  16  17  18        │  ← Today highlighted orange
└─────────────────────────────────────────┘

Timeline Bar (below calendar):
═══════════[t1]════════════════[t2]═══[t3]═══════════════════════ 🔴
            │                    │     │                          LIVE
         07:23                09:12  11:30                       NOW

[t] = Thumbnail marker
├── Positioned by: (time_minutes / 1440) * bar_width
├── Shows: arrival thumbnail image
├── On hover: shows time + "Arrival" or "Departure"
├── On click: loads clip in player, shows event details
└── Orange border when selected

LIVE button:
├── Red pulsing dot
├── Click: switches player to YouTube embed
└── Shows current stream time
```

### About Page

```
┌────────────────────────────────────────────────────────────────────┐
│  🦅 KANYO                                        [← Back] [GitHub] │
├────────────────────────────────────────────────────────────────────┤
│                                                                    │
│  About Kanyō                                                       │
│  ══════════════════════════════════════════════════════════════   │
│                                                                    │
│  Kanyō (観鷹, "contemplating falcons") is an open-source system   │
│  for automated monitoring of falcon nest cameras using computer   │
│  vision and machine learning.                                      │
│                                                                    │
│  Origin Story                                                      │
│  ─────────────────────────────────────────────────────────────    │
│  Born from a conversation with Claudia Goldin on a flight to      │
│  New York, where she mentioned wanting timestamps for when         │
│  falcons are actually visible on the Harvard nest cam.            │
│                                                                    │
│  How It Works                                                      │
│  ─────────────────────────────────────────────────────────────    │
│  • YOLOv8 detects birds in real-time from YouTube streams         │
│  • State machine tracks arrivals, visits, and departures          │
│  • Clips and thumbnails are automatically generated               │
│  • Notifications sent via Telegram                                │
│                                                                    │
│  Collaborate                                                       │
│  ─────────────────────────────────────────────────────────────    │
│  • 🔬 Ornithologists: Help improve detection accuracy             │
│  • 💻 Developers: Contribute on GitHub                            │
│  • 📹 Camera operators: Add your nest cam                         │
│                                                                    │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐       │
│  │  ⭐ GitHub     │  │  ☕ Ko-fi      │  │  📧 Contact    │       │
│  └────────────────┘  └────────────────┘  └────────────────┘       │
│                                                                    │
│  Built by [Your Name] • MIT License                               │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
```

---

## Part 5: Project Structure

```
viewer/
├── backend/
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py               # FastAPI app, serves React build
│   │   ├── config.py             # Settings, stream registry loader
│   │   └── routers/
│   │       ├── __init__.py
│   │       ├── streams.py        # /api/streams endpoints
│   │       ├── clips.py          # /api/clips file serving
│   │       └── comments.py       # /api/comments (simple JSON)
│   ├── requirements.txt
│   ├── Dockerfile
│   └── streams.yaml              # Stream registry with display info
│
├── frontend/
│   ├── src/
│   │   ├── App.jsx               # Router setup
│   │   ├── main.jsx              # Entry point
│   │   ├── index.css             # Tailwind imports
│   │   │
│   │   ├── pages/
│   │   │   ├── Landing.jsx       # Stream cards
│   │   │   ├── StreamView.jsx    # Main viewer
│   │   │   └── About.jsx         # Project info
│   │   │
│   │   ├── components/
│   │   │   ├── Header.jsx        # Logo + nav + stream dropdown
│   │   │   ├── StreamCard.jsx    # Landing page card
│   │   │   ├── Timeline.jsx      # HKSV-style scrubber
│   │   │   ├── WeekCalendar.jsx  # Date selector
│   │   │   ├── VideoPlayer.jsx   # Clip player + YouTube embed
│   │   │   ├── CameraInfo.jsx    # Info panel (location, etc.)
│   │   │   ├── StatsPanel.jsx    # 24h stats
│   │   │   ├── EventDetails.jsx  # Selected event info + actions
│   │   │   └── SubscribeButton.jsx # Telegram link
│   │   │
│   │   ├── hooks/
│   │   │   ├── useStreams.js     # Fetch stream list
│   │   │   ├── useEvents.js      # Fetch events for date
│   │   │   └── useStats.js       # Fetch stats
│   │   │
│   │   └── utils/
│   │       ├── api.js            # API client
│   │       ├── time.js           # Time formatting helpers
│   │       └── constants.js      # Colors, etc.
│   │
│   ├── public/
│   │   └── favicon.ico
│   ├── index.html
│   ├── package.json
│   ├── vite.config.js
│   ├── tailwind.config.js
│   └── postcss.config.js
│
├── docker-compose.yml            # Full stack
├── docker-compose.dev.yml        # Development overrides
└── README.md
```

---

## Part 6: Implementation Steps

### Day 1: Backend API (4 hours)

**Objective:** FastAPI backend serving stream metadata and clips

**Tasks:**

1. Create `viewer/backend/` structure
2. Implement stream registry loader from `streams.yaml`
3. Build `/api/streams` endpoints (list, detail, stats)
4. Build `/api/clips` file serving (thumbnails, videos)
5. Build `/api/streams/{id}/events` endpoint
6. Dockerfile for backend
7. Test with curl/httpie

**Success Criteria:**

- [ ] `GET /api/streams` returns list with display metadata
- [ ] `GET /api/streams/harvard` returns full detail
- [ ] `GET /api/streams/harvard/events?date=2025-01-14` returns events
- [ ] `GET /api/clips/harvard/2025-01-14/falcon_072315_arrival.jpg` serves file
- [ ] Docker container builds and runs

### Day 2: Frontend Foundation (4 hours)

**Objective:** React app with routing and basic pages

**Tasks:**

1. Create Vite + React project
2. Configure Tailwind CSS (dark theme)
3. Set up react-router (Landing, StreamView, About)
4. Build Header component with logo and nav
5. Build Landing page with StreamCard components
6. Build About page content
7. API client setup with fetch

**Success Criteria:**

- [ ] `npm run dev` starts dev server
- [ ] Landing page shows stream cards (hardcoded first)
- [ ] Click card navigates to `/stream/harvard`
- [ ] About page renders with content
- [ ] Dark theme applied correctly

### Day 3: Timeline Component (4 hours)

**Objective:** HKSV-style timeline with thumbnails

**Tasks:**

1. Build WeekCalendar component (date selector)
2. Build Timeline component (24-hour bar)
3. Position thumbnails by time calculation
4. Implement hover state (show time tooltip)
5. Implement click handler (select event)
6. Wire up to real events data from API
7. Style to match HKSV aesthetic

**Success Criteria:**

- [ ] Calendar shows week with today highlighted
- [ ] Timeline shows thumbnail markers at correct positions
- [ ] Hover shows event time
- [ ] Click selects event (orange border)
- [ ] Day navigation works

### Day 4: Video + Polish (4 hours)

**Objective:** Complete viewer functionality

**Tasks:**

1. Build VideoPlayer component (HTML5 video)
2. Add YouTube embed mode for LIVE button
3. Build CameraInfo panel
4. Build StatsPanel component
5. Build EventDetails with download/share buttons
6. Implement SubscribeButton (Telegram link)
7. Responsive design tweaks
8. Loading states and error handling

**Success Criteria:**

- [ ] Clicking timeline event plays clip
- [ ] LIVE button shows YouTube embed
- [ ] Camera info displays correctly
- [ ] Stats update with time range selector
- [ ] Download button works (direct file link)
- [ ] Share button copies URL to clipboard
- [ ] Subscribe opens Telegram

### Day 5: Deploy + Test (2 hours)

**Objective:** Production deployment

**Tasks:**

1. Build production frontend (`npm run build`)
2. Configure FastAPI to serve React build
3. Full docker-compose setup
4. Test locally with production build
5. Deploy to VPS (or tunnel for now)
6. Configure domain DNS
7. End-to-end testing

**Success Criteria:**

- [ ] `docker compose up` runs full stack
- [ ] Accessible at `kanyo.sageframe.net`
- [ ] All features work in production
- [ ] Cloudflare analytics enabled

---

## Part 7: Key Code Patterns

### Timeline Position Calculation

```javascript
// Convert time to X position (0-100%)
const timeToPosition = isoTime => {
  const date = new Date(isoTime)
  const minutes = date.getHours() * 60 + date.getMinutes()
  return (minutes / 1440) * 100 // 1440 = minutes in day
}

// Usage in Timeline.jsx
{
  events.map(event => (
    <div
      key={event.id}
      style={{ left: `${timeToPosition(event.start_time)}%` }}
      className="absolute transform -translate-x-1/2 ..."
    >
      <img src={`/api/clips/${streamId}/${date}/${event.thumbnail_path}`} />
    </div>
  ))
}
```

### Stream Dropdown with Info

```jsx
// Header.jsx
<select
  value={currentStream}
  onChange={e => navigate(`/stream/${e.target.value}`)}
  className="bg-zinc-800 text-white rounded px-3 py-2"
>
  {streams.map(s => (
    <option key={s.id} value={s.id}>
      {s.display.short_name} - {s.display.location.split(',')[0]}
    </option>
  ))}
</select>
```

### Telegram Subscribe Button

```jsx
// SubscribeButton.jsx
const SubscribeButton = ({ channel }) => {
  const telegramUrl = `https://t.me/${channel}`

  return (
    <a
      href={telegramUrl}
      target="_blank"
      rel="noopener noreferrer"
      className="flex items-center gap-2 bg-blue-600 hover:bg-blue-700 
                 text-white px-4 py-2 rounded-lg transition-colors"
    >
      <BellIcon className="w-5 h-5" />
      Subscribe
    </a>
  )
}
```

### Download and Share Buttons

```jsx
// EventDetails.jsx
const EventDetails = ({ event, streamId, date }) => {
  const clipUrl = `/api/clips/${streamId}/${date}/${event.arrival_clip_path}`
  const shareUrl = `${window.location.origin}/stream/${streamId}?date=${date}&event=${event.id}`

  const handleShare = async () => {
    await navigator.clipboard.writeText(shareUrl)
    toast.success('Link copied!')
  }

  return (
    <div className="flex gap-2">
      <a href={clipUrl} download className="btn">
        <DownloadIcon /> Download
      </a>
      <button onClick={handleShare} className="btn">
        <LinkIcon /> Share
      </button>
    </div>
  )
}
```

---

## Part 8: Success Criteria

### Minimum Viable (This Ho)

- [ ] Landing page shows all streams with cards
- [ ] Stream view with working timeline
- [ ] Click thumbnail to play clip
- [ ] LIVE button shows YouTube
- [ ] Camera info panel displays
- [ ] Stats panel shows 24h data
- [ ] Download button works
- [ ] Share button copies link
- [ ] Subscribe button opens Telegram
- [ ] About page complete
- [ ] Deployed and accessible

### Stretch Goals

- [ ] Comments system (JSON-based)
- [ ] Time range selector (24h, 2d, 3d)
- [ ] Keyboard navigation (arrows for events)
- [ ] Mobile responsive design
- [ ] PWA manifest (installable)

---

## Part 9: Future Enhancements (Not This Ho)

### Comments System

- JSON file per stream per day
- Anonymous with optional display name
- localStorage remembers name
- Future: OAuth for verified annotations

### Bird Tracking

- Structured annotations (species, behavior, count)
- Per-bird ID (banding numbers)
- Export to CSV for researchers

### Advanced Features

- Activity heatmap
- Multi-day timeline view
- Web push notifications
- Embeddable widget

---

## Part 10: Testing Checklist

### Manual Testing

**Landing Page:**

- [ ] Stream cards load with thumbnails
- [ ] Today's visit count shown
- [ ] Status indicator correct
- [ ] Click navigates to stream view

**Stream View:**

- [ ] Dropdown shows all streams
- [ ] Calendar shows current week
- [ ] Today highlighted in orange
- [ ] Timeline shows events for today
- [ ] Thumbnails positioned correctly

**Timeline Interaction:**

- [ ] Hover shows time tooltip
- [ ] Click selects event (orange border)
- [ ] Selected event details appear
- [ ] Day navigation updates timeline

**Video Player:**

- [ ] Clip plays when event selected
- [ ] LIVE button switches to YouTube
- [ ] YouTube embed loads correctly
- [ ] Stream local time displayed

**Actions:**

- [ ] Download button downloads file
- [ ] Share button copies URL
- [ ] Subscribe opens Telegram

**About Page:**

- [ ] Content renders correctly
- [ ] Links work (GitHub, Ko-fi, etc.)
- [ ] Back button works

---

## Part 11: Agent Implementation Prompt

Use this prompt with Claude Code or another coding agent to implement Ho 6.5:

---

# KANYO PUBLIC VIEWER IMPLEMENTATION

## Context

You are building a public-facing viewer for Kanyō, a falcon monitoring system. The backend detection pipeline is already running and generating clips/thumbnails. The admin UI exists at a separate endpoint.

This viewer lives in its own repository, separate from the detection tool. The detection tool (kanyo-contemplating-falcons) is meant to be reusable by others; this viewer is your specific frontend deployment.

## Repository Setup

**Two sibling repos in the workspace:**

~/vaults/sageframe-no-kaji-dev/
├── kanyo-contemplating-falcons/ ← Detection tool (existing, public)
│ ├── admin/
│ ├── src/
│ ├── docker/
│ └── ...
└── kanyo-viewer/ ← Viewer (NEW, create this, can be private)
├── backend/
├── frontend/
└── docker-compose.yml

**To create the new repo:**

```bash
cd ~/vaults/sageframe-no-kaji-dev
mkdir kanyo-viewer
cd kanyo-viewer
git init
# Create GitHub repo (private or public) and add remote
```

**On the production server (kanyo.lan):**

```
/opt/services/
├── kanyo-code/          → kanyo-contemplating-falcons repo (detection)
├── kanyo-viewer/        → kanyo-viewer repo (this project)
├── kanyo-harvard/       → stream data + clips
└── kanyo-nsw/           → stream data + clips
```

The viewer container mounts clip directories read-only from the detection containers' data paths.

## Reference Material

The detection tool repo contains useful reference:

- `kanyo-contemplating-falcons-dev/admin/web/` - Existing admin UI (FastAPI + Jinja2 + HTMX + Tailwind)
- `kanyo-contemplating-falcons-dev/docker/` - Docker patterns for the project
- `kanyo-contemplating-falcons-dev/src/kanyo/` - Detection pipeline code (for understanding data structures)

Project notes are in:

```
kanyo-contemplating-falcons-dev/devlog/ho-6.5-public-viewer-web-gui
```

You can reference these files while building the viewer.

## Tech Stack

- Backend: FastAPI (Python 3.11)
- Frontend: React 18 + Vite + Tailwind CSS
- Styling: Dark theme (HKSV-inspired, pure black background)
- Deployment: Docker (self-hosted)

## What to Build

### 1. Backend (`viewer/backend/`)

**FastAPI app with these endpoints:**

GET /api/streams → List streams with display metadata
GET /api/streams/{id} → Stream detail + today's stats  
GET /api/streams/{id}/events → Events for date (query: ?date=YYYY-MM-DD)
GET /api/streams/{id}/stats → Stats for range (query: ?range=24h|2d|3d)
GET /api/clips/{stream}/{date}/{filename} → Serve media files

**Stream registry:** Load from `streams.yaml` with this structure:

streams:
harvard:
name: "Harvard Falcon Cam"
youtube_id: "glczTFRRAK4"
data_path: "/data/harvard"
timezone: "America/New_York"
display:
short_name: "Harvard"
location: "Memorial Hall, Harvard University, Cambridge MA"
species: "Peregrine Falcon (Falco peregrinus)"
maintainer: "Harvard Faculty of Arts & Sciences"
maintainer_url: "https://www.fas.harvard.edu/"
description: "Active nest since 1999."
telegram_channel: "kanyo_harvard_falcon"

**Events source:** Read from existing `events_{date}.json` files in clips directory.

### 2. Frontend (`viewer/frontend/`)

**Pages:**

- `/` - Landing with stream cards
- `/stream/:id` - Main viewer with timeline
- `/about` - Project info

**Key Components:**

1. **Timeline.jsx** - HKSV-style horizontal bar
   - 24-hour range (midnight to midnight)
   - Thumbnail markers positioned by time: `(minutes / 1440) * 100%`
   - Orange border on selected
   - LIVE button at right edge

2. **WeekCalendar.jsx** - Date selector
   - Shows 7 days centered on today
   - Today highlighted in orange (#ff9500)
   - Click to change date

3. **VideoPlayer.jsx** - Dual mode
   - Default: HTML5 video playing clips
   - LIVE mode: YouTube iframe embed

4. **CameraInfo.jsx** - Right sidebar panel
   - Location, species, maintainer
   - Stream local time (live updating)
   - Subscribe button

5. **StatsPanel.jsx** - Below camera info
   - Visits, arrivals, departures count
   - Last event with timestamp
   - Time range selector buttons

6. **EventDetails.jsx** - Below timeline when event selected
   - Event time and duration
   - Download button (direct link to .mp4)
   - Share button (copy URL with event ID)

**Styling:**

- Background: #000000 (pure black)
- Cards/panels: #1c1c1e
- Accent: #ff9500 (orange)
- Arrivals: #0a84ff (blue)
- Departures: #ff453a (red)
- Status green: #30d158

### 3. Docker Setup

**docker-compose.yml:**

services:
viewer:
build: .
ports: - "3000:3000"
volumes: - /opt/services/kanyo-harvard:/data/harvard:ro - /opt/services/kanyo-nsw:/data/nsw:ro
environment: - NODE_ENV=production

**Multi-stage Dockerfile:**

1. Stage 1: Build React with Vite
2. Stage 2: Python with FastAPI serving built files

### 4. File Structure

viewer/
├── backend/
│ ├── app/
│ │ ├── main.py
│ │ ├── config.py
│ │ └── routers/
│ │ ├── streams.py
│ │ └── clips.py
│ ├── requirements.txt
│ └── streams.yaml
├── frontend/
│ ├── src/
│ │ ├── App.jsx
│ │ ├── pages/
│ │ │ ├── Landing.jsx
│ │ │ ├── StreamView.jsx
│ │ │ └── About.jsx
│ │ └── components/
│ │ ├── Timeline.jsx
│ │ ├── WeekCalendar.jsx
│ │ ├── VideoPlayer.jsx
│ │ └── ...
│ ├── package.json
│ ├── vite.config.js
│ └── tailwind.config.js
├── Dockerfile
└── docker-compose.yml

## Implementation Order

1. Backend API (streams, events, clips serving)
2. Frontend scaffold (Vite, Tailwind, routing)
3. Landing page with StreamCard
4. StreamView layout (player, sidebar, timeline area)
5. WeekCalendar component
6. Timeline component with thumbnail positioning
7. VideoPlayer (clips + YouTube)
8. CameraInfo and StatsPanel
9. EventDetails with download/share
10. About page
11. Docker setup
12. Test and polish

## Key Requirements

- Timeline thumbnails MUST be positioned by time calculation, not evenly spaced
- LIVE button opens YouTube embed, not navigation
- Download is direct file link (no API processing)
- Share copies URL with query params: `/stream/harvard?date=2025-01-14&event=20250114_072315`
- Subscribe button opens `https://t.me/{channel}` in new tab
- Dark theme throughout, no light mode
- Mobile responsive (timeline collapses gracefully)

## Do NOT Include

- Authentication (public viewer)
- Comments (prepared structure only, no UI yet)
- Admin features (separate app)
- Full visit clips in timeline (arrival/departure thumbnails only)

## Testing

After implementation, verify:

1. Landing shows streams with correct stats
2. Timeline positions match actual event times
3. Click thumbnail → plays correct clip
4. LIVE → shows YouTube embed
5. Download → browser downloads .mp4
6. Share → clipboard has correct URL
7. Subscribe → opens Telegram

---

## Appendix: Reference Images

### HKSV Timeline (inspiration)

- Week calendar at top with today highlighted
- Camera dropdown selector
- Thumbnail filmstrip at bottom
- Orange playhead indicator
- LIVE button at right

### Kanyō Admin (existing)

- Stream cards with thumbnails
- Status indicators (🟢 Running)
- Visit counts and last event time
- View/Config/Restart actions

---

_Document created: 2025-01-14_ _Ready for implementation_
