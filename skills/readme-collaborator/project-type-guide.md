# Project Type Guide — README Adaptation

The README covers the same territory for every project. How it covers that territory — what it leads with, what it emphasizes, what it minimizes — depends on what the project is. This guide describes four project types and the README patterns that serve each one.

**You should not need to ask the person what type their project is.** The seed and System Design make it clear. If you genuinely can't tell, the upstream documents have a clarity problem worth naming.

---

## How to Classify

Read the seed's audience section and the System Design's deployment model. These two inputs are usually sufficient.

| Signal | Product | Utility | Infrastructure | Library |
|---|---|---|---|---|
| **Audience** | Non-technical users or domain experts | The builder themselves, or technical users with a specific task | The builder as operator, or a small team | Other developers |
| **Interface** | GUI (desktop app, web app, mobile app) | CLI, scripts, or automation | Services, containers, daemons | API, SDK, package |
| **Deployment** | Download/install, app store | Package manager, pip/npm/brew, direct download | Docker, systemd, cloud deploy | Package manager |
| **Core interaction** | "Open and use" | "Run a command" | "Deploy and configure" | "Import and call" |
| **Examples** | Sutra, iA Writer, m4Bookmaker | yt-dlp, black, ffmpeg | Kanyō, Hōzō, Grafana stack | SwiftGit2, FastAPI, React |

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

### Minimize technical content.

"How It Works" is one paragraph for the curious. Architecture is absent. The user doesn't need to know about the version engine or the piece manager. They need to know it's local, it's markdown, and their files are theirs.

### Installation is simple.

"Download. Drag to Applications." Or "Download the installer. Run it." One or two steps. If it's more complicated than that, the deployment model has a problem.

### Structure

```
# [Name]
*[Tagline]*

[Hero paragraph]

## What [Name] Does
[Vocabulary / capabilities as experiences]

## What [Name] Is Not
[Scope boundaries — 1-3 sentences]

## Your First Session
[Narrative walkthrough — 2-4 paragraphs]

## How It Works
[One paragraph for the curious]

## Download
[Link and install instructions]

## Requirements
[OS, hardware — brief]

## Development
[Build from source — for contributors]

## Status
[Honest state of the project]

## License
[Stated or TBD]
```

---

## Utility README Pattern

**The user is technical and task-oriented. The README should answer "what does it do and how do I use it" in under a minute.**

### Lead with what it does.

One paragraph. What the tool does, what input it takes, what output it produces. Concrete, not abstract.

### Include a quick start.

The minimum commands to go from installed to working. Copy-pasteable. Three to five steps. This replaces "Your First Session" — for a utility, the session IS the commands.

### Show usage examples.

Real commands with real flags and real output. Not exhaustive — the most common use cases. A full reference can live in a separate document or `--help` output.

### Installation is functional.

Package manager command, or download + PATH setup. Include dependencies if they're not automatic.

### Architecture is optional but welcome.

A brief pipeline diagram or data flow description helps the user understand what's happening. Especially useful for multi-stage tools (capture → process → output).

### Structure

```
# [Name]

[One paragraph: what it does]

## Quick Start
[3-5 copy-pasteable steps]

## Usage
[Common commands and examples]

## Installation
[Package manager or download + install]

## Requirements
[OS, dependencies]

## How It Works
[Brief pipeline or data flow — optional]

## Configuration
[Config file format, environment variables — if applicable]

## Development
[Build from source, run tests]

## Status
[Version, stability]

## License
```

---

## Infrastructure README Pattern

**The user is the operator. The README should answer "how do I deploy this and keep it running."**

### Lead with what it does and where it runs.

One paragraph. What the system does, what infrastructure it needs, what it produces. Include the deployment context (homelab, cloud, hybrid).

### Include deployment instructions.

Docker Compose, systemd units, Ansible playbooks — whatever the deployment mechanism is. This is the equivalent of "installation" but more detailed because infrastructure has more moving parts.

### Show configuration.

Full config file examples with comments. Environment variables. What's required vs. optional. Sensible defaults.

### Architecture is important.

The operator needs to understand the system. Component diagram, data flow, port mappings, storage locations. Not System Design depth — operational depth.

### Include operational notes.

How to monitor. How to check if it's working. How to troubleshoot common problems. Where logs live. Backup and restore.

### Structure

```
# [Name]

[What it does and where it runs]

## Architecture
[Component diagram, data flow]

## Deployment
[Docker/systemd/cloud setup — detailed]

## Configuration
[Full config examples with comments]

## Usage
[How to interact with the running system — web UI, CLI, API]

## Requirements
[Hardware, OS, network, dependencies]

## Operations
[Monitoring, logs, troubleshooting, backup]

## Development
[Build from source, local dev setup]

## Status
[Version, stability]

## License
```

---

## Library/Framework README Pattern

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

### Structure

```
# [Name]

[What it does and why — one paragraph]

## Quick Example
[Minimal working code — import to result]

## Installation
[Package manager command]

## Usage
[Key types, functions, patterns with examples]

## API Overview
[Shape of the API — not exhaustive]

## Architecture
[How it's structured — for understanding, not contribution]

## Requirements
[Language version, platform, dependencies]

## Development
[Build from source, run tests, contribute]

## Status
[Version, stability, roadmap]

## License
```

---

## Edge Cases

**The project doesn't fit neatly into one type.** Use the primary interaction to classify. A tool with a CLI and a web dashboard is infrastructure if the dashboard monitors a running system, a product if the dashboard IS the experience.

**The project is pre-release.** Write as if it exists, with a Status section that's honest. Placeholders are fine for download links and version numbers. The description of what it does should be present-tense and concrete.

**The project is a personal tool with no external audience.** The README still exists — it serves the future version of the builder who has forgotten the details. Simplify where appropriate, but don't skip it.

**The seed and System Design are thin.** Name the gaps. The README can't commit to scope that the upstream documents didn't define. Send the person back to fill the gaps rather than inventing content.
