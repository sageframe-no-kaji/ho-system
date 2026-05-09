# Ho 0: Project Overview

## 観鷹（かんよう / kan'yō） - Peregrine Falcon Detection & Timeline Project

**Project Name:** 観鷹 (kan'yō) - "contemplating falcons"

### Origin Story

This project emerged from a conversation with Claudia Goldin (Nobel laureate in Economics) on a flight to New York. She mentioned her involvement with the peregrine falcon cam atop Memorial Hall at Harvard and expressed interest in having the live feed automatically mark timestamps when the peregrines are actually in frame.

**Memorial Hall Peregrine Cam:** https://www.youtube.com/watch?v=glczTFRRAK4

### Project Vision

**Immediate Goal:**
Build an automated system that processes the Memorial Hall falcon cam feed and creates a browsable timeline of falcon activity with timestamps and thumbnails.

**Potential Future:**

- Community tagging and annotation system
- Research tool for ornithology
- Collaboration opportunity with Claudia Goldin
- Multi-cam platform for peregrine population tracking

### Core Philosophy

This project prioritizes:

1. **Learning by doing** - Building ML/full-stack skills through practical application
2. **Shipping over perfection** - Getting something working and useful quickly
3. **Iterative improvement** - Start simple, add sophistication as needed
4. **Documentation as learning** - Each step documented for understanding and sharing

### Technical Architecture

**Detection Pipeline:**

```
YouTube Live Stream → yt-dlp (capture) → Frame Extraction →
YOLOv8 (ML detection) → Event Logic → Timestamp Database → Web Interface
```

**Infrastructure:**

- **Computation:** GitHub Actions (cloud) or homelab (Proxmox with GPU)
- **Storage:** GitHub repo for code, Cloudflare for hosting
- **Frontend:** Static site on Cloudflare Pages
- **Backend (future):** Cloudflare Workers + D1 for user tagging

**Event Detection Types:**

- `falcon_enters` - First detection after absence
- `falcon_exits` - Last detection before absence
- `movement_after_stasis` - Activity after 5+ minutes of stillness
- `falcon_count_change` - Multiple birds detected
- `significant_activity` - High motion/interaction

### Data Model

**Core Entities:**

```
Detection Event:
  - timestamp (ISO 8601)
  - youtube_time (embed parameter format)
  - event_type (enter/exit/movement/activity)
  - confidence (0.0-1.0)
  - thumbnail_url
  - falcon_count
  - notes

User Tag (future):
  - detection_id
  - user_id
  - tag_type (feeding/landing/prey/chicks/etc)
  - notes
  - created_at
```

### The Ho System

This project is organized into **hos** (歩/ほ - "step" in Japanese, as in walking). Each ho is a ~2-hour focused work session with:

- Clear objective and deliverable
- Step-by-step guidance
- Testable completion criteria
- Documentation requirements

**The beauty of limits:** Claude Pro has message limits (~45 per 5 hours), which actually helps maintain sustainable pacing and prevents burnout.

**Ho Structure:**

- **Ho 0.5: Tool Mastery** - Claude Code, Git workflow, development setup
- **Foundation Phase** (Ho 1-3): Setup, detection basics, event logic
- **Automation Phase** (Ho 4-6): Stream capture, pipeline, cloud deployment
- **Frontend Phase** (Ho 7-9): Web interface, deployment, user tagging
- **Polish Phase** (Ho 10-11): Tuning, documentation, launch

### Tool Ecosystem

**Three tools, three purposes:**

#### **Claude.ai (The Architect)**

**Use for:**

- Ho planning and briefings
- High-level decisions and strategy
- Understanding concepts and debugging
- Debrief after each ho

**Pattern:** ~2-3 messages per ho (briefing, optional help, debrief)
**Model:** Sonnet (default) - perfect balance for planning

#### **Claude Code (The Builder)**

**Use for:**

- Creating files and project structure
- Writing detection scripts and logic
- Complex multi-file operations
- Heavy implementation work

**Pattern:** 1-2 focused sessions per ho
**Models:**

- **Opus** for complex/learning hos (detection logic, events, backend)
- **Sonnet** for straightforward implementation (setup, config, deployment)
- **Haiku** for quick tweaks and simple edits

**Commands:**

```bash
# Default (Sonnet)
claude-code "create detection script with YOLOv8"

# For complex work (Opus)
claude-code --model claude-opus-4 "implement event detection logic"

# For quick fixes (Haiku)
claude-code --model claude-haiku-4 "fix typo in config"
```

#### **VSCode + GitHub Copilot (The Assistant)**

**Use for:**

- Small edits and tweaks
- Learning by typing
- Following clear instructions
- Git operations and testing

**Pattern:** Throughout each ho for manual work
**Cost:** Free with GitHub Education account

### Efficient Workflow Pattern

**Ho Start (Claude.ai - 1 message):**

- Request ho briefing
- Receive detailed mission with commands

**Ho Execution (Claude Code + Manual):**

- Use Claude Code for big builds (1-2 sessions)
- Use Copilot for small edits
- Test and iterate manually
- Document in devlog

**Ho Complete (Claude.ai - 1 message):**

- Report results
- Share what worked/broke
- Receive next ho brief

**Emergency Help (Claude.ai - as needed):**

- When truly stuck
- For clarification on concepts
- Strategic debugging

This pattern keeps usage sustainable at ~3-5 messages per ho.

### Time Commitment

**Target:** 22-28 hours over 6-8 weeks
**Pace:** 2-4 hos per week (4-8 hours/week)
**Structure:**

- Ho 0.5: 1-1.5 hours (tool mastery)
- Ho 1-11: ~2 hours each
- Buffer time for debugging and learning

**Sustainable pattern:** Claude Pro limits (~45 messages/5 hours) naturally pace the work, preventing burnout while maintaining momentum.

### Success Criteria

**Minimum Viable:**

- Working detection on Memorial Hall stream
- Static website showing falcon activity timeline
- Deployed and accessible via URL
- Something compelling to show Claudia Goldin

**Stretch Goals:**

- User tagging system functional
- Real-time or near-real-time detection
- Community adoption by falcon enthusiasts
- Collaboration with Claudia on research applications

### Technology Stack

**Core Tools:**

- **Python 3.10+** - Detection pipeline
- **yt-dlp** - YouTube stream capture
- **YOLOv8 (Ultralytics)** - Object detection ML model
- **OpenCV** - Frame processing and thumbnail generation
- **Git/GitHub** - Version control and CI/CD
- **GitHub Actions** - Automated processing
- **Cloudflare Pages** - Static site hosting
- **Cloudflare Workers + D1** - User tagging backend (future)

**Development Environment:**

- **VSCode** - Primary editor
- **Claude Code** - AI-assisted file creation and implementation
- **GitHub Copilot** - Code completion and assistance (via GitHub Education)
- **Python virtual environment** - Dependency isolation

**AI Model Selection Strategy:**

- **Claude Opus** - Complex logic (detection, events, backend)
- **Claude Sonnet** - Standard implementation (setup, deployment)
- **Claude Haiku** - Quick fixes and simple edits

### Learning Objectives

By completing this project, you will gain practical experience with:

- Machine learning deployment (YOLOv8 inference)
- Video processing and stream capture
- Event detection and temporal logic
- Static site generation
- CI/CD pipelines (GitHub Actions)
- Serverless architecture (Cloudflare Workers)
- Git workflow and version control
- Full-stack development patterns
- Product thinking and iteration
- AI-assisted development workflow

### Working Within Limits (A Feature, Not A Bug)

**Claude Pro usage limits (~45 messages/5 hours) create beneficial constraints:**

**Advantages:**

- **Prevents burnout** - Forces breaks and reflection
- **Encourages planning** - Think before asking
- **Builds independence** - Learn to solve problems manually
- **Sustainable pace** - 2-4 hos/week is healthy long-term
- **Better documentation** - Written guides reduce need for live help

**Strategy:**

- Use Claude.ai for architecture and planning (high-value conversations)
- Use Claude Code for implementation (efficient batched work)
- Use Copilot + manual work for iteration (build skills)
- Document thoroughly so future-you doesn't need AI help

The goal isn't maximum AI usage - it's building something real while developing genuine understanding.

### Repository Structure

```
kanyo/
├── README.md
├── docs/
│   ├── architecture.md
│   ├── data-model.md
│   └── deployment.md
├── devlog/
│   ├── ho-00-overview.md (this file)
│   ├── ho-0.5-tool-mastery.md
│   ├── ho-01-git-good.md
│   ├── ho-02-falcon-vision.md
│   └── ...
├── src/
│   ├── detection/
│   │   ├── capture.py
│   │   ├── detect.py
│   │   └── events.py
│   │   └── site_generator.py
│   └── utils/
├── tests/
├── .github/
│   └── workflows/
│       └── detect-and-deploy.yml
├── site/
│   ├── index.html
│   ├── styles.css
│   └── data/
│       └── detections.json
└── requirements.txt
```

### Next Steps

**Ho 0.5: "Tool Mastery"** - Learn Claude Code workflow, Git basics, development environment setup

Begin when ready to commit 1-1.5 hours to foundational tool learning.

---

**Project Start Date:** December 2025
**Target Demo Date:** February 2026
**Maintained by:** Tyro Sageframe

```
┌────────────────────────────────────────────────────────────────┐
│ YOUR LAPTOP (Development)                                      │
│  - Write code                                                  │
│  - Run tests                                                   │ 
│  - Git push to GitHub                                          │
└────────────────────────────────────────────────────────────────┘
                         ↓ push code
┌────────────────────────────────────────────────────────────────┐
│ GITHUB ACTIONS (Detection Pipeline) - FREE                     │
│  - Runs Python every hour                                      │
│  - Downloads stream with yt-dlp                                │
│  - Detects falcons with YOLOv8                                 │
│  - Generates site/ with Jinja2                                 │
│  - Pushes site/ to Cloudflare                                  │
└────────────────────────────────────────────────────────────────┘
                         ↓ deploy
┌────────────────────────────────────────────────────────────────┐
│ CLOUDFLARE PAGES (Static Hosting) - FREE                       │
│  - Hosts HTML/CSS/JSON                                         │
│  - Global CDN                                                  │
│  - https://kanyo.pages.dev                                     │
└────────────────────────────────────────────────────────────────┘
                         ↓ visit
┌────────────────────────────────────────────────────────────────┐
│ USERS (Falcon Enthusiasts)                                     │
│  - See timeline                                                │
│  - Click to watch on YouTube                                   │
│  - (Later: Add tags via Cloudflare Workers)                    │
└────────────────────────────────────────────────────────────────┘
```
