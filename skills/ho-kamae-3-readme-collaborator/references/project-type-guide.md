# Project Type Guide — README Adaptation

The README covers the same territory for every project (see `readme-structure-reference.md` for the full inventory). How it covers that territory — what it leads with, what it emphasizes, what it minimizes — depends on what the project is. This guide describes four project types and the README patterns that serve each one.

**You should not need to ask the person what type their project is.** The seed and System Design make it clear. If you genuinely can't tell, the upstream documents have a clarity problem worth naming.

---

## Relationship to Structure and Variant Axes

Three things determine what a finished README looks like:

1. **Section inventory** (in `readme-structure-reference.md`) — which sections exist as possibilities.
2. **Variant axes** (in `readme-structure-reference.md`) — which conditional sections to include based on what the project is (has a competitor? is in an ecosystem? has imagined-not-built features? has a meaningful name?).
3. **Project type** (this file) — *how to write* each section depending on whether the project is a product, utility, infrastructure, or library.

Project type and variant axes are orthogonal. A Product project may or may not have a competitor section. An Infrastructure project may or may not sit in an ecosystem. Apply both: variant axes tell you which sections appear; project type tells you how to write them.

---

## How to Classify

Read the seed's audience section and the system design's deployment model. These two inputs are usually sufficient.

| Signal | Product | Utility | Infrastructure | Library |
|---|---|---|---|---|
| **Audience** | Non-technical users or domain experts | The builder themselves, or technical users with a specific task | The builder as operator, or a small team | Other developers |
| **Interface** | GUI (desktop, web, mobile app) | CLI, scripts, or automation | Services, containers, daemons | API, SDK, package |
| **Deployment** | Download/install, app store | Package manager, pip/npm/brew, direct download | Docker, systemd, cloud deploy | Package manager |
| **Core interaction** | "Open and use" | "Run a command" | "Deploy and configure" | "Import and call" |
| **Examples** | Sutra, iA Writer | yt-dlp, black, ffmpeg | Kanyō, Hōzō, Shodō, Grafana stack | SwiftGit2, FastAPI, React |

Hybrid projects exist. A tool with both a CLI and a GUI is a product if the GUI is the primary experience, a utility if the CLI is. A self-hosted service with an admin web UI is infrastructure. Use the primary interaction to classify.

---

## Product README Pattern

**The user is a person, not a developer. The README should feel like finding a tool you've been looking for.**

### Lead with identity and experience.

Open with the tagline and hero paragraph. The reader should understand what the project is and what it feels like before they read a single feature description.

### Describe capabilities as experiences, not functions.

"Mark a moment in your writing" — not "creates a git commit with a user-provided message." If the project has its own vocabulary, introduce it here as the natural way of talking about what the product does.

### Include "Your First Session."

A narrative walkthrough of the core loop. Second person, present tense. Not a tutorial — a story. The reader should feel what using the product is like. Two to four paragraphs. This is the most important content section.

### Minimize technical content in the README; offload to architecture.md.

"How It Works" is one paragraph for the curious. The full Architecture section is a brief component breakdown that points to `docs/architecture.md` for depth. The user doesn't need to understand the version engine; they need to know it's local, it's their format, and their files are theirs.

### Installation is simple.

"Download. Drag to Applications." Or "Download the installer. Run it." One or two steps. If it's more complicated than that, the deployment model has a problem.

### Suggested order

```
# [Name]
*[Tagline]*
*[Subhead — optional]*

> [Hero paragraph]

**Status:** [active development / pre-release / etc.]

## What's Broken
## What [Name] Does
## Your First Session
## What [Name] Is Not
## How [Name] Differs                  ← if competitor warranted
## Naming                              ← if name carries meaning
## Where [Name] Sits                   ← if ecosystem positioning warranted
## How It Works                        ← brief, accessible technical paragraph
## Architecture                        ← component breakdown; link to docs/architecture.md
## Tech Stack
## Current State                       ← Now / Next / Later table
## What's Ahead                        ← if substantial deferred features
## Download
## Usage
## Requirements
## Development
## License

[Footer: attribution + last meaningful update]
```

---

## Utility README Pattern

**The user is technical and task-oriented. The README should answer "what does it do and how do I use it" in under a minute.**

### Lead with what it does.

One paragraph. What the tool does, what input it takes, what output it produces. Concrete, not abstract.

### Include a quick start.

The minimum commands to go from installed to working. Copy-pasteable. Three to five steps. This replaces "Your First Session" — for a utility, the session IS the commands.

### Show usage examples.

Real commands with real flags and real output. Not exhaustive — the most common use cases. A full reference can live in `--help` output or a separate document.

### Installation is functional.

Package manager command, or download + PATH setup. Include dependencies if they're not automatic.

### Architecture is brief but welcome.

A pipeline diagram or data flow description helps the user understand what's happening. Especially useful for multi-stage tools (capture → process → output). For utilities with substantial pipelines, `docs/architecture.md` is warranted; for simple utilities, the README's Architecture section is enough.

### Suggested order

```
# [Name]
*[One-line description]*

> [What it does — one paragraph]

**Status:** [version, stability]

## What's Broken                       ← if the seed has a strong friction
## What [Name] Does
## Quick Start                         ← 3–5 copy-pasteable steps
## Usage                               ← Common commands and examples
## Installation
## How [Name] Differs                  ← if competitor warranted
## Where [Name] Sits                   ← if ecosystem positioning warranted
## Architecture                        ← brief; pipeline diagram if multi-stage
## Tech Stack
## Configuration                       ← config file format, env vars
## Current State                       ← Now / Next / Later
## Requirements
## Development
## License

[Footer: attribution + last meaningful update]
```

---

## Infrastructure README Pattern

**The user is the operator. The README should answer "how do I deploy this and keep it running."**

### Lead with what it does and where it runs.

One paragraph. What the system does, what infrastructure it needs, what it produces. Include the deployment context (homelab, cloud, hybrid).

### Architecture is important.

The operator needs to understand the system. Component diagram, data flow, port mappings, storage locations. Operational depth, not full system design depth. For substantial infrastructure projects, `docs/architecture.md` is almost always warranted.

### Include deployment instructions.

Docker Compose, systemd units, Ansible playbooks — whatever the deployment mechanism is. This is the equivalent of "installation" but more detailed because infrastructure has more moving parts.

### Show configuration.

Full config file examples with comments. Environment variables. What's required vs. optional. Sensible defaults.

### Include operational notes.

How to monitor. How to check if it's working. How to troubleshoot common problems. Where logs live. Backup and restore.

### Suggested order

```
# [Name]
*[One-line description]*
*[Subhead — optional]*

> [What it does and where it runs]

**Status:** [version, stability, deployment status]

## What's Broken                       ← if the seed has a strong friction
## What [Name] Does
## How [Name] Differs                  ← if competitor warranted
## Where [Name] Sits                   ← if ecosystem positioning warranted
## How It Works                        ← optional: accessible technical paragraph
## Architecture                        ← component diagram, data flow; link to docs/architecture.md
## Tech Stack
## Current State                       ← Now / Next / Later
## What's Ahead                        ← if substantial deferred features
## Deployment                          ← Docker/systemd/cloud setup
## Configuration                       ← Config examples with comments
## Usage                               ← How to interact with the running system
## Requirements                        ← Hardware, OS, network, dependencies
## Operations                          ← Monitoring, logs, troubleshooting, backup
## Development                         ← Build from source, local dev setup
## License

[Footer: attribution + last meaningful update]
```

---

## Library / Framework README Pattern

**The user is a developer building something with your tool. The README should answer "how do I integrate this."**

### Lead with the problem it solves.

One paragraph. What gap it fills, what it enables, why it exists instead of the alternatives.

### Show a minimal code example immediately.

Import, configure, call. The reader should see working code within the first screen. This is the equivalent of "Your First Session" for a library — the session is writing code.

### Document the API surface.

Not exhaustive (that's the API docs), but enough to understand the shape. Key types, key functions, key patterns.

### Installation is a package manager command.

One line. pip, npm, cargo, SPM. Include version pinning guidance if stability matters.

### Architecture helps.

How the library is structured. What the key abstractions are. How the user is expected to extend or customize it. This helps the developer build a mental model of what they're working with.

### Suggested order

```
# [Name]
*[One-line description]*

> [What it does and why — one paragraph]

**Status:** [version, stability]

## What's Broken                       ← problem the library solves
## Quick Example                       ← Minimal working code
## Installation
## Usage                               ← Key types, functions, patterns with examples
## API Overview                        ← Shape of the API; not exhaustive
## How [Name] Differs                  ← if competitor warranted
## Where [Name] Sits                   ← if ecosystem positioning warranted
## Architecture                        ← How it's structured
## Tech Stack
## Current State                       ← Version, stability, roadmap
## What's Ahead                        ← if substantial deferred features
## Requirements                        ← Language version, platform, dependencies
## Development                         ← Build from source, run tests, contribute
## License

[Footer: attribution + last meaningful update]
```

---

## Edge Cases

**The project doesn't fit neatly into one type.** Use the primary interaction to classify. A tool with a CLI and a web dashboard is infrastructure if the dashboard monitors a running system, a product if the dashboard IS the experience.

**The project is pre-release.** Write as if it exists, with an honest Status section and a Current State table that makes the gap explicit. Placeholders are fine for download links and version numbers. The description of what it does should be present-tense and concrete.

**The project is a personal tool with no external audience.** The README still exists — it serves the future version of the builder who has forgotten the details, AND any future collaborator. Simplify where appropriate, but don't skip it. The Personal Infrastructure variant is closest to Infrastructure pattern but with less emphasis on Configuration / Operations and more emphasis on "what I built and why."

**The seed and system design are thin.** Name the gaps. The README cannot commit to scope that the upstream documents didn't define. Send the person back to fill the gaps rather than inventing content.

**The project is methodology or framework, not software.** Treat as Library/Framework pattern but adjust: the "code example" becomes a usage example (how to engage with the methodology), and the audience is practitioners rather than developers in the strict sense.
