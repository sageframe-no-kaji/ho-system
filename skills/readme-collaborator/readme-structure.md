# README Structure — Reference

The README is the third document in the Kamae project framing chain (Seed → System Design → README → Ho Overview). It commits to scope by describing the finished project in user-facing language.

The README is a scope document, not a thinking document. The seed thinks. The System Design decides. The README describes. If you find yourself making decisions while writing the README, the upstream documents have gaps — go back and fill them.

---

## Territory (Always Covered)

Every README covers this territory regardless of project type. How it covers each item varies — see the project type guide for emphasis patterns.

### 1. Identity

Project name. One-sentence description or tagline. Hero paragraph if one was developed during the seed or System Design conversation.

The identity comes from upstream. If the seed and System Design produced a tagline and hero paragraph, use them. If not, develop them with the person — present options, let them choose, prefer their voice over yours.

### 2. What It Does

User-facing description of the project's capabilities. Written for the audience defined in the seed, in language that audience understands.

For products: describe experiences. "Mark a moment in your writing. See what changed between any two moments."
For utilities: describe functions. "Captures frames from a YouTube live stream, runs YOLO detection, and generates a browsable timeline."
For infrastructure: describe what it manages. "Wakes a remote backup server, runs ZFS sync, verifies the transfer, and shuts it down."
For libraries: describe what it enables. "Provides a typed Swift interface to git operations without exposing libgit2's C API."

If the project has its own vocabulary (as Sutra does), this is where the vocabulary is introduced — as features, not as translations. The reader should understand what each term means from the description, not from a mapping table to technical concepts.

### 3. What It Is Not

Scope boundaries from the seed, stated briefly. One to three sentences. "Sutra is not a notes app. It is not a collaboration tool. It is not a publishing platform."

This prevents misunderstanding and sets expectations. It also signals design confidence — the project knows what it chose not to be.

### 4. The Experience (varies by type)

The most important content section. Varies most by project type.

**Products:** "Your First Session" — a narrative walkthrough of the core loop. Not a tutorial. A story. The reader should feel what using the product is like. Written in second person present tense. Two to four paragraphs.

**Utilities:** "Quick Start" — the minimum commands to go from installed to working. Copy-pasteable. Three to five steps.

**Infrastructure:** "Quick Start" — setup, configuration, first run. Enough to get it working, with pointers to full documentation.

**Libraries:** "Usage" — a minimal code example that demonstrates the core capability. Import, configure, call.

### 5. How It Works

Brief explanation of how the project works internally. Depth varies by audience.

**Products:** One paragraph. "Each piece of writing is a git repository. You never interact with git." For the curious, not required reading.

**Utilities:** A paragraph or a pipeline diagram. Enough to understand the data flow.

**Infrastructure:** Architecture overview — what components, where they run, how they connect. The user is the operator; they need to understand the system.

**Libraries:** API overview or architecture. The user builds on this; they need to understand the structure.

### 6. Getting It

How the user obtains the project. The delivery commitment.

**Products:** Download link, install instructions (drag to Applications, run installer, etc.).
**Utilities:** Package manager command, or download + install steps.
**Infrastructure:** Docker command, deployment instructions, or setup script.
**Libraries:** Package manager command (pip, npm, cargo, SPM).

Placeholders are fine for pre-release projects: "Download: coming soon." The delivery *method* should be stated even if the deliverable doesn't exist yet.

### 7. Requirements

What the user needs. OS version, hardware, dependencies, accounts.

Keep it short. A bulleted list or a single sentence.

### 8. Development Setup

For contributors. How to build from source, what tools are needed, how to run tests.

This section exists in every README but may be brief for early-stage projects. At minimum: clone, build tool, build command.

### 9. Status

Honest statement of where the project is. What exists, what's in progress, what's planned.

For released projects: version number, stability level.
For pre-release projects: what's been built, what's next, what's not yet started.

This is the one section that's written in the actual present tense (describing reality), not the aspirational present tense (describing the finished product). The reader should know exactly what they're looking at.

### 10. License

Stated or noted as TBD. If TBD, a brief note on intent ("The instinct is open source core with a commercial layer. Resolution before public release.").

---

## What "Done" Looks Like

Someone reading the README should understand:

- What this project is and who it's for
- What it does (concretely, not abstractly)
- What it doesn't do (scope boundaries)
- What using it feels like or how to use it
- How to get it
- What they need to run it
- What state the project is in

The README should be publishable on day one of the repository. It is a commitment to scope, written as if the project exists. The Status section provides honest grounding.

## What "Done" Does NOT Look Like

- A tutorial (that's what hos are for)
- An architecture document (that's the System Design)
- Marketing copy (factual and experiential, not promotional)
- A frozen document (the README evolves as the project develops)

---

## Frontmatter Convention

```yaml
---
created: [date]
status: draft
type: readme
project: [project-name]
stage: kamae-3
kamae-chain: seed → system-design → **readme** → ho-overview
builds-on: [project]-kamae-2-system-design
next: [project]-kamae-4-ho-overview
---
```

## Filename Convention

`[project]-kamae-3-readme.md`

Note: This is the Kamae README — the scope document produced during project framing. When the project ships, a separate `README.md` may live in the repository root. The Kamae README is the source; the repo README is derived from it and updated as the project develops.

---

## The Kamae Chain

The README sits between the System Design and the Ho Overview:

- **Seed (Kamae 1)** provides vision — "here's what we're building and why"
- **System Design (Kamae 2)** commits to architecture — "here's how it works"
- **README (Kamae 3)** commits to scope — "here's what it does and how to get it"
- **Ho Overview (Kamae 4)** commits to build order — "here's how we build it"

The README's philosophical stem comes from the seed. Its technical content comes from the System Design. It is where intent meets specification.
