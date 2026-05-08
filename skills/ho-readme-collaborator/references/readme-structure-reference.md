# README Structure Reference

The map of what a Ho System project README contains, when each section appears, what each section does, and what the skill must refuse to produce.

This reference works alongside `project-type-guide.md`. **Structure** (this file) tells you which sections to include. **Project type** (the other file) tells you how to write each section depending on whether the project is a product, utility, infrastructure, or library. Both apply to every README.

---

## What "Done" Looks Like

A reader of the finished README should understand:

- What this project is and who it's for
- What it does, concretely (not abstractly)
- What it doesn't do (scope boundaries)
- Where it sits — in an ecosystem, a body of work, or alongside competitors
- What using it feels like or how to use it
- How to get it
- What state the project is in right now
- What's coming
- Under what license

The README should be publishable and link-able from the writer's public spaces on day one of the repository, even if no code has been shipped yet. It is the *continuously canonical public face* of the project — updated as the project develops, never stale, never coy about the current state.

## What "Done" Does NOT Look Like

- A tutorial (that's what hos are for)
- An architecture spec (that's the system design; `docs/architecture.md` is a public extract, not the full spec)
- Marketing copy (factual and experiential, not promotional)
- A frozen document (the README evolves as the project develops)
- A document with internal Kamae chain frontmatter (that belongs in private working copies, not the public face)

---

## Section Inventory

Sections fall into three groups: **always** (every README), **conditional** (produced only when the variant axis triggers), and **practical** (the deployment-oriented sections at the end).

### Always

| Section | Source | Purpose |
|---|---|---|
| Title (H1) | seed | Project name |
| Tagline (italic) | seed / hero text | One-line essence; the soul in 6–12 words |
| Subhead (italic) | seed / hero text | Optional; secondary frame |
| Hero/lede paragraph (blockquote) | seed / hero text | The metaphor, the framing, the way in |
| Status (bold paragraph) | framing convo | Where the project is right now in honest terms |
| What's Broken / The Problem | seed (Friction/Problem section) | Diagnostic clarity. Why this exists. |
| What X Does | seed + system design | The mechanism, the vocabulary, the primitives |
| What X Is Not | seed (Scope Boundaries) | Defends against misreads |
| Architecture | system design | Component breakdown |
| Tech Stack | system design | Surface-level: languages, frameworks, key libraries |
| Current State (table or prose) | framing convo | Now / Next / Later |
| License | framing convo | Per the forwardness fork |
| Footer | — | Attribution + last meaningful update date |

### Conditional (produced only when the variant axis is triggered)

| Section | Trigger | Source |
|---|---|---|
| Your First Session / Narrative walkthrough | Project type is Product, OR seed contains a use-case narrative or "test" framing | seed |
| How X Differs | Seed names a "closest existing tool" or "most direct comparable" | seed (Landscape) |
| Naming | Name carries meaning explained in seed (Identity / Naming section) | seed |
| Where X Sits | Project sits inside a body of work (parent ecosystem, sibling projects, broader research program) | framing convo + seed hints |
| How It Works | Architecture is substantial enough that a public reader would benefit from a one-paragraph accessible technical pitch separate from the formal Architecture section | system design (re-voiced) |
| What's Ahead | Seed has substantial deferred-but-imagined content (Architecturally Prepared For, Features in Development) | seed |

### Practical

| Section | Purpose |
|---|---|
| Download / Installation | How users actually get and install the thing. Honest placeholder if pre-build. |
| Usage / Quick Start | How users actually run the thing. Per project type — see project-type-guide. |
| Requirements | OS, hardware, runtime dependencies |
| Development | How to build from source (git clone, build commands) |
| Configuration | (For utilities and infrastructure) Config file format, environment variables — if applicable |
| Operations | (For infrastructure) Monitoring, logs, troubleshooting, backup |

---

## Suggested Order

The order below works as a default. Adjust if the project's specific shape calls for different flow. Project type also affects ordering — see `project-type-guide.md`.

1. Title
2. Tagline + subhead
3. Hero/lede paragraph
4. Status block
5. What's Broken (problem/diagnostic)
6. What X Does (vocabulary, primitives)
7. Your First Session (if narrative warranted — products especially)
8. What X Is Not
9. How X Differs (if competitor warranted)
10. Naming (if name carries meaning)
11. Where X Sits (if ecosystem positioning warranted)
12. How It Works (if accessible technical paragraph warranted)
13. Architecture
14. Tech Stack
15. Current State
16. What's Ahead (if substantial deferred content)
17. Download / Installation / Quick Start
18. Usage
19. Requirements
20. Development
21. Configuration / Operations (infrastructure-heavy projects)
22. License
23. Footer

---

## Variant Detection Logic

For each axis, the skill checks the source documents and the framing conversation. Each axis is independent.

### Competitor comparison?

Look in the seed's Landscape section for phrases like "the closest existing tool," "the most direct comparable," "the closest comparable." If a single competitor is named with substantive comparison, produce a "How X Differs" section that:

- Names the competitor and acknowledges shared territory
- Names what the competitor lacks that this project provides
- Closes with a clear differentiator

Skip if the seed has a landscape but no clear single competitor (multiple adjacent tools, none of them "the" comparable).

### What's Ahead section?

Look in the seed for sections titled "Architecturally Prepared For," "Bookmarked: Design Space," "Features in Development," "Out of Scope but Imagined," or equivalent. Count the items. If there are roughly four or more substantive items, produce a What's Ahead prose section with one paragraph per item. If there are three or fewer, fold into the status section's "Later" column.

### Ecosystem positioning section?

Determine whether the project sits inside a larger body of work. Sources of signal:
- Seed mentions a parent ecosystem, organization, or methodology
- Seed names sibling projects ("part of X")
- Conversation with the writer confirms the project belongs to a coherent corpus

If yes, produce a "Where X Sits" section that:
- Names the parent body of work in one sentence
- Connects this project to its role within (flagship, module, engine, sister)
- Names sibling projects briefly with one-line descriptions of each

Skip if the project is genuinely standalone.

### Naming section?

Check the seed for an Identity or Naming section that explains what the project's name means. If present and the meaning is non-obvious to outsiders, produce a brief Naming paragraph (3–5 sentences) that explains the meaning.

Skip if the name is conventional or its meaning is obvious from context.

---

## Anti-Patterns the Skill Refuses

These are non-negotiable. The skill does not produce them regardless of what the source documents contain.

### Frontmatter in public docs

Internal Ho System chain navigation frontmatter (`created: …`, `status: draft`, `kamae-chain: …`, `builds-on: …`, `next: …`) does not appear in the public README or architecture.md. It belongs in the writer's private working copy if anywhere. If the writer's existing README has frontmatter, strip it.

### Marketing-speak

Adjectives that assert quality or significance without evidence. Cut on sight:

- "state-of-the-art"
- "robust"
- "comprehensive"
- "innovative"
- "powerful"
- "transformative"
- "compelling"
- "seamlessly"
- "deeply" / "profoundly" / "fundamentally" (when depth has not been shown)

When the source document uses one of these, replace with concrete specifics or cut entirely. Example: "state-of-the-art retrieval quality" → either drop, or replace with specifics like "8K context window, multilingual."

### Three-adjective evaluative stacks

"Clear, concise, and compelling." "Robust, comprehensive, and innovative." Three or more evaluative adjectives in a row are an AI tell. If each adjective is doing distinct definitional work (naming a real property), fewer adjectives or rephrased prose lands cleaner.

### Throat-clear openings

"This document describes…" "In this section we will…" "Welcome to…" Cut. The H1 already says what the document is.

### Granular tech detail in the README

Do not put DB schemas, full env var tables, complete API references, or detailed deployment topology in the README. Push to `docs/architecture.md`, `docs/api.md`, or wherever the project's deeper documentation lives.

### Instance-specific implementation in architecture.md

ZFS dataset layouts, specific machine names, deployment paths from the writer's homelab — these are private deployment notes, not project architecture. Strip from public `architecture.md`.

### Fake completeness for install/usage when pre-build

If the project is not yet built, install and usage sections are honest placeholders, not invented commands. Use italics: *"Not yet. Installation instructions will appear here as the v1 components ship."* Tell the truth.

### Pre-emptive evaluation in section openers

"This section is critical because…" "What follows is the most important part of the project…" The section either is critical and shows it, or it isn't. Cut the framing.

### Mirror summary at section ends

"As we've seen…" "To recap…" "In summary, what this means is…" Cut. The section made its point; restating it dilutes.

---

## DI Discipline

Run the eight-class DI taxonomy on every produced section. The classes:

- **PE (Pre-emptive Evaluation)** — announcing significance before the content delivers
- **PU (Performed Urgency)** — stacked fragments as rhythm device disconnected from accumulated content
- **TC (Throat-Clear)** — language that prepares the reader instead of delivering to them
- **BH (Balanced Hedge)** — false balance; softening a known position
- **SS (Significance Stamp)** — adjectives asserting importance without evidence
- **MS (Mirror Summary)** — restating what was just said
- **EI (Emotional Instruction)** — telling the reader what to feel
- **MT (Manufactured Transition)** — sentences naming the relationship between paragraphs that the reader could have inferred

### Bootstrap Protection

Run the DI check on the seed FIRST in init mode, before drafting. Patterns in the seed propagate downstream — if the seed has a PE about the heat map, the README will inherit it unless caught early. Surface seed hits to the writer separately; do not silently fix the seed yourself, but produce clean output regardless.

### Localized DI checks in update mode

Update mode does not re-check sections that didn't change. It runs the DI check on each modified section as it's edited. This catches new DI introduced during the update without re-litigating sections that were already cleared.

---

## Voice Notes

- **Body and soul** come from the seed, re-voiced for outsider read. Preserve the writer's framing, vocabulary, and rhythm. Do not flatten metaphors the seed introduced.
- **Tech-shape** comes from the system design, re-voiced for accessibility. Strip internal voice ("This is the architectural commitment…") and Ho System chain references.
- **Hero text and taglines** — if the writer has produced these as separate voice-crafted material, use them directly. Do not reinterpret.
- **Build-drop rhythm** (long sentence accumulating specifics, followed by a short declarative that lands the argument) is a feature of the writer's voice, not PU. Watch for content-bearing short sentences vs. rhythm-only short sentences.
- **Corrective pairs** ("Not a demo. Not to show someone.") are the writer's voice. Keep.
- **Present tense throughout, except Status and Current State.** The README describes the project as if it exists. The Status block and Current State table are the only places written in the actual present tense (describing reality), not the aspirational present tense (describing the finished product).
- **The skill produces a structural scaffold, not finished voice.** The writer does the voice pass on top before publishing. Hand off honestly.

---

## File Naming and Output Hygiene

- Files go at canonical paths from the start. Not iteration-named.
  - `<project>/README.md`
  - `<project>/docs/architecture.md`
- Update mode rewrites in place. It does NOT produce parallel files with iterative names (`README-v2.md`, `synthesized-README.md`, `<project>-kamae-3-readme.md`).
- Process artifacts (e.g., a re-derivation produced during init mode for comparison) live under clearly process-labeled names (`<project>/pass1-rederivation.md`) and are NOT presented as the canonical README.
- When presenting files to the writer, present the canonical version. Process artifacts are reference material, not deliverables.
