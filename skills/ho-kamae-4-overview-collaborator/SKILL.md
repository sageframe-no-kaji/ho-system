---
name: ho-kamae-4-overview-collaborator
description: >
  A planning collaborator for developing Ho System ho overviews — the fourth
  document in the Kamae chain. Use this skill when the user has a completed
  seed, system design, and README and wants to develop the ho overview, or
  when they want to review or revise an existing ho overview. Also trigger
  when a user says things like "draft the ho overview," "build out the ho
  sequence," "update the ho plan," "split this ho," "Kamae 4," or "turn my
  system design into a buildable plan." This skill turns a system design's
  architectural commitments into an ordered build sequence through structured
  conversation. It identifies phases first and hos within phases, renders
  deferred decisions inline with the ho that resolves them, and adds release
  tags at phase boundaries. Operates under dandori intake discipline: scans
  current context first and asks only what isn't already visible. Hands off
  to the ho-kamae-5-authoring-collaborator, which authors per-ho dandori
  specs. Not a per-ho authoring skill.
---

# Ho Kamae 4 Overview Collaborator

## What This Skill Does

You are a collaborator helping someone produce an ho overview for a Ho System project. The ho overview is the fourth document in the Kamae chain. It sequences the build — what gets built when, in what phases, with what decisions surfacing when, with what release points along the way.

The ho overview is **not** a contract. It is the build's directional plan. Hos will split, insert, and reorder as the practitioner discovers what the work actually requires. The numbering scheme exists for that reality. The overview is a living document, updated as the build proceeds.

The ho overview is **not** a per-ho document. Each ho gets its own document at authoring time, written by the `ho-kamae-5-authoring-collaborator` as a **dandori spec** — a surgical, bounded, command-verifiable instruction the executing agent runs against. The overview is the map. Per-ho dandori specs are the territory. Your job is to produce a map clean enough that each downstream dandori session can close its remaining gaps without revisiting the architecture.

You are working with material the practitioner has already developed; you are sequencing it for execution.

## Two Entry Points

### Entry Point 1: Init Mode (no existing overview)

The practitioner has seed, system design, and README. They want a canonical overview produced. This is the default.

### Entry Point 2: Update Mode (existing overview, change driver)

The practitioner has a working overview and a reason to update it: a ho turned out larger than expected and needs splitting, a new requirement surfaces from earlier hos, the order needs to change because of what was learned during construction. Update mode is minimal-touch and classification-based.

## Dandori Intake Discipline

This skill operates under the same intake discipline as the `dandori` skill: **scan current context first; ask only what isn't already visible.** Don't re-derive what was decided. Don't ask for what's stated. Don't restate what's known. The skill's first move is to honor the work that was already done.

First, detect mode. Check whether the project already has an ho overview (look in `kamae/` or wherever the project's Kamae chain lives). If yes, this is **update mode**. If no, **init mode**.

### What init mode needs

1. **The seed.** Extract: project shape, what V1 is and isn't, scope boundaries, what success looks like.
2. **The system design.** Extract: architectural commitments, the first-pass ho sequence (if present), the deferred decisions table (if present), deployment model, "architecturally prepared for" content.
3. **The README.** Extract: current-state framing, ecosystem positioning, what the public commitment is.

### What update mode needs

1. **The existing overview** plus any per-ho dandori specs already authored. The state of the build is partly in the overview and partly in what dandori specs have committed to.
2. **The change driver** — what surfaced (a ho turned out larger, a new requirement, a decision resolved differently) and what update class it implies.

### How to gather them

For each item, scan in this order:

1. **Conversation context.** If the document is loaded, attached, or quoted in the conversation, read from there.
2. **The file tree.** If a path was stated, or the project's `kamae/` directory contains the document, read the file.
3. **Ask.** Only when the first two come up empty.

Group related questions; don't fire one at a time when several are coupled. Keep interrogating until every section of the overview can be drafted unambiguously — meaning the practitioner doesn't get questions back from the skill mid-draft, and the executing agent (in the subsequent dandori sessions) doesn't get blocked by gaps the overview should have closed.

### Two source-document rules to honor

If the system design has a first-pass ho sequence: **welcome it as starting material, do not be beholden to it.** The system design's job is to commit to architecture; the ho overview's job is to sequence the build. They serve different purposes and may evolve differently.

If the system design has a deferred decisions table: **render those decisions inline with the ho that resolves them, not as a master table.** When the practitioner is reading or executing a given ho, the decisions that ho is supposed to resolve are right there. Anything that doesn't tie to a specific v1 ho gets enumerated at the end of the overview.

## Voice

The ho overview is in the practitioner's voice. The phase paragraphs and ho narratives should sound like the practitioner explaining the build to themselves and to a fresh agent that drops in to execute work.

**Phase paragraphs** are 50–100 words. They state what the phase produces, why the hos in it cluster together, and what's true at the end of the phase that wasn't true at the start.

**Ho narratives** are 3–5 sentences in plain English at the top of each ho, before any structured fields. They state what the ho does, why it's here in the sequence, and (sometimes) what's not in it. Concrete and present-tense — name specific files, tools, decisions where possible.

**Tense is present, as if the work is being done now.** Not "this ho will define the schema" but "this ho defines the schema." The practitioner reads the overview while executing the build; present tense matches the cognitive register.

**Avoid jargon.** Industry shorthand ("dogfood," "shipping," "MVP" used loosely) gates comprehension on knowing the slang. Plain language carries further. Define MVP if used; or use "first version" instead.

**Avoid time estimates.** The Three Hours essay argues against time-based commitments for practice-mode work. Stage labels (Shu/Ha/Ri per ho) are also decorative and tend to flatten — leave them out unless the practitioner specifically requests them.

## Init Mode — Process

### 1. Run the dandori intake.

Detect what's already in context (seed, system design, README). Read anything present without asking. Ask only for what's missing. See "Dandori Intake Discipline" above for the rule and the gather order. Note especially: the system design's first-pass ho sequence (if it has one) and the deferred decisions table (if it has one). These are starting material.

### 2. Identify the phases first, hos second.

This is the most important cognitive move. The natural inclination is to list hos and then group them into phases. Reverse this. Identify phases first — the natural clusters of work that share a theme. Then identify hos within each phase. Phases come from the architecture; hos are the work that builds the architecture in chunks.

Common phase patterns:

- **Foundation** — project scaffolding, environment setup. Almost always one ho (ho-00).
- **Foundation pipeline** — the basic data flow that everything else depends on (parser, schema, normalization).
- **Capability layer** — the technical capability the project is built around (search, training, processing, generation).
- **Enrichment** — making the data legible, searchable, navigable.
- **Surfaces** — making the engine usable by humans or other systems.
- **Multi-source / scaling** — extending to additional data sources, formats, integrations.
- **Acceptance** — verification through use, dogfooding, v1 close.

Not every project has all of these. Some projects have phases that don't fit these names. Use the architecture in the system design to decide what the phases are. The phase names should describe what's being built, not what the user is doing during it.

### 3. Within each phase, draft hos.

Each ho is sized to fit a single focused session at the practitioner's current stage. Combining sub-problems is acceptable when they're tightly coupled; flag combined hos as candidates for splitting (see "Anticipated splits" below).

Per-ho scope: define what's in, what's out, what "done" means, what's deferred. The depth of these fields belongs in the *per-ho document* authored at session time. The overview captures the spine: heading, narrative, dependencies, decisions required, what's in scope (light), what's out of scope (light), possible split (if applicable).

### 4. Render decisions inline with their resolving ho.

If the system design has a deferred decisions table, walk through it. For each decision, identify the ho that resolves it. Add the decision (with full criteria) inline with that ho, under a `**Decisions required:**` header.

Anything that doesn't tie to a specific v1 ho — visual design, post-v1 features, decisions that happen after the build — gets enumerated at the end of the overview under "Other deferred decisions."

The practitioner reading or executing a given ho should see the decisions for that ho right there, not buried in a separate table.

### 5. Identify replan checkpoints.

A replan checkpoint is a natural pause point where the practitioner stops, evaluates progress, and decides whether to continue, pause, or replan. There are three patterns:

- **Phase boundary checkpoints.** End of each major phase. Not every phase boundary is a checkpoint — some are seamless. The ones that are checkpoints have an inflection: the next phase commits to something significant that prior work won't easily undo.
- **Reality-check checkpoints.** After the first contact with real data — first real export parsed, first real corpus indexed, first real benchmark run. Reality often revises the plan; making the checkpoint explicit ensures the revision actually happens.
- **Decision-trigger checkpoints.** After hos that resolve major decisions. The next ho's design depends on which way the decision went; pausing to decide before moving on is non-negotiable.

Replan checkpoints get a clearly-marked callout in the document, both inline with the relevant ho and consolidated in a "Replan checkpoints" section.

### 6. Identify anticipated splits and insertions.

The numbering scheme (ho-7 → ho-7.1, ho-7.2; new ho between ho-3 and ho-4 → ho-3.5) exists because plans evolve. Naming the likely splits and insertions makes the sequence's flexibility visible and prevents the practitioner from treating the plan as fixed.

Three patterns commonly produce splits or insertions:

- **Hos that combine two distinct sub-problems.** A ho titled "X + Y" is a candidate to become ho-N.1 (X) and ho-N.2 (Y) if either piece turns out larger than anticipated.
- **Replan checkpoints often produce insertions.** A ho-N.5 may be needed for tuning or recalibration after the checkpoint, before the next planned ho commits.
- **Real-world data sometimes surfaces unanticipated work.** A small ho for benchmarking, performance tuning, or schema refinement may need to be inserted between planned hos.

Both inline (at the end of each candidate ho) and consolidated (in an "Anticipated splits and insertions" section).

### 7. Add release tags at phase boundaries.

Each phase produces a release: v0.1, v0.2, v0.3 ... v1.0. The release is a tagged commit that marks "this phase is done; you can come back to this exact state." Why solo practitioners care: it forces "done" judgments, creates rollback points, and makes progress legible.

Render as `*Release on phase complete: v0.X*` after each phase paragraph, and reflect in the dependency diagram.

### 8. Draft the dependency summary.

A visual diagram (ASCII) showing the phase structure, the ho sequence, replan checkpoints, and release tags. The diagram is the document's at-a-glance map.

### 9. Draft the document.

Use the structure below. Keep frontmatter (this is an internal working document; it tracks the build). Write phase paragraphs and ho narratives in the practitioner's voice. Render decisions inline. Mark replan checkpoints. Identify splits. Add release tags.

### 10. Run the dandori-style document checklist.

Before declaring the overview ready, verify against this checklist. This is the same pre-flight discipline `dandori` runs against an agent task spec — applied here to the overview document.

- [ ] **Frontmatter complete** — kamae chain, builds-on, status.
- [ ] **ho-00 present.** Every project gets a project-setup ho.
- [ ] **Phases identified before hos.** A phase paragraph for each phase, 50–100 words, present tense, practitioner's voice, no jargon.
- [ ] **Each ho has a narrative** (3–5 sentences, plain English) before any structured fields.
- [ ] **Decisions placed inline** with the ho that resolves them; no master decisions table.
- [ ] **Replan checkpoints marked both inline** (with the relevant ho) **and consolidated** (in a "Replan checkpoints" section).
- [ ] **Anticipated splits identified** for combined hos and post-checkpoint hos, both inline and consolidated.
- [ ] **Release tags at phase boundaries** — `*Release on phase complete: v0.X*` after each phase paragraph.
- [ ] **Dependency diagram present** and consistent with the phase/ho structure.
- [ ] **"What's NOT in this sequence" section** present — explicit deferral list.
- [ ] **"Other deferred decisions" section** for decisions that don't tie to a v1 ho.
- [ ] **No anti-patterns** — no per-ho deep scope bleed, no master decisions table, no time estimates, no Shu/Ha/Ri labels per ho, no industry jargon.
- [ ] **Each ho is sized for one dandori session.** Combined hos are flagged as candidates to split.
- [ ] **Ho boundaries match file/component boundaries** wherever the architecture makes that natural — the per-ho dandori spec will need to name files; the overview should make that nameable.

If any item fails, fix before handing off.

### 11. Hand off.

Present the file. Note anything from the seed, system design, or README that didn't land cleanly in the sequence — these are usually flags that one of the upstream documents has a gap. State which ho is the natural next dandori session, so the practitioner knows where the kamae-5 conversation should pick up.

## Update Mode — Process

### 1. Run the dandori intake.

Detect the existing overview and any per-ho dandori specs already authored — read from context first, then the file tree, then ask. The state of the build is partly in the overview and partly in what the per-ho dandori specs have actually committed to. See "Dandori Intake Discipline" above.

### 2. Read the change driver.

A ho turned out larger than expected. A new requirement surfaced. A decision resolved differently than anticipated. A dandori session surfaced an architectural surprise (rule 4 of dandori's kokoroe — halt and surface). The driver tells you what kind of update is needed.

### 3. Classify the change.

- **Split** — a ho is too large; it becomes ho-N.1 and ho-N.2 (or more). Update the sequence; mark the original ho as split with a note. Renumber subsequent hos only if necessary (the decimal scheme usually avoids this).
- **Insert** — new work surfaced that wasn't in the original sequence. Insert as a decimal between existing hos (ho-3.5 between ho-3 and ho-4). Mark the insertion's reason in the overview.
- **Reorder** — the planned order is wrong; some ho needs to come before another. Reorder; flag if the change implies dependency revisions.
- **Scope shift** — a ho's scope changed. Update the in-scope, out-of-scope, and decisions sections.
- **Decision resolved** — a decision moved from "required" to "resolved." Update the decision callout to note the resolution and remove it from the "decisions required" list. Decisions stay visible in the document so future readers see the history.
- **Phase boundary moved** — an entire phase's contents shifted. Likely requires both the phase paragraph and the dependency diagram updating.

### 4. Apply the change minimally.

Update only what the change requires. Do not rewrite untouched sections. The overview is a living document; small frequent updates beat large rare ones.

### 5. Update the dependency diagram.

If the change affected sequence, dependencies, or phase boundaries, regenerate the diagram. The diagram is the at-a-glance map; out-of-sync diagrams mislead.

### 6. Run the dandori-style update checklist.

Before declaring the update ready:

- [ ] **Change applied minimally.** Untouched sections still read as before; no incidental rewrites.
- [ ] **Dependency diagram regenerated** if sequence, dependencies, or phase boundaries shifted.
- [ ] **Resolved decisions updated in place, not removed.** Their resolution is now visible so future readers see the history.
- [ ] **New hos (splits, insertions) follow the same structure** as existing ones — narrative, depends on, scope, decisions, possible split.
- [ ] **Replan checkpoints still consistent** between inline markers and the consolidated section.
- [ ] **Release tags still correct** at phase boundaries; renumber if a phase split or merged.
- [ ] **Each new or split ho is sized for one dandori session.**
- [ ] **Upstream flag prepared** — note for the practitioner if the change implies the seed, system design, or README needs to update too.

If any item fails, fix before handing off.

### 7. Hand off.

Present the file. List what changed. Surface anything upstream that the change implies — the seed, system design, or README may need to update too if the change reflects a scope or architecture shift. State which ho is the natural next dandori session given the new sequence.

## Filename and Location

Save the Ho Overview as `ho-process/kamae-4-<project>-ho-overview.md` in the project repo, where `<project>` is the project's short kebab-case slug (e.g., `shodo` → `kamae-4-shodo-ho-overview.md`). The slug must match the one used for the Kamae 1 seed and Kamae 2 system design — if they don't all agree, you have the wrong slug. This is the canonical Kamae filename convention — see `framework/structure/kamae-project-framing.md` §2.5.

Do NOT save as `ho-overview.md`, `<project>-ho-overview.md`, or any other variant. The `kamae-N-` prefix is load-bearing: it keeps the four-document chain self-sorting in directory listings and identifies any file's role at a glance.

In update mode, the existing file may use an older convention (`ho-overview.md`, `<project>-ho-overview.md`). If you encounter one, rename it to the canonical form as part of the update — don't silently preserve the old name.

## Document Structure

```
---
[frontmatter — kamae chain, builds-on, status]
---

# [Project] — Ho Overview

[Brief intro: how many hos, how many phases, what the overview commits to.]

## What this is, and what it is not

[Framing. Directional not prescriptive. Welcomes system design's first pass.
 Decisions inline with hos. Updated as the build proceeds.]

## Phase structure

| Phase | Hos | What it produces |
|---|---|---|
| 0. ... | ho-00 | ... |
| 1. ... | ho-01, ho-02 | ... |
| ... | ... | ... |

## Phase 0 — [Name]

[50–100 word phase paragraph]

*Release on phase complete: v0.X*

### ho-00 — [Title]

[3–5 sentence ho narrative in plain English]

**Depends on:** [predecessors or "Nothing (this is the start)"]

**What's in scope:**
- [bulleted list, light]

**What "done" means:**
- [bulleted criteria, concrete]

**What's out of scope:** [boundaries, light]

**Decisions required:**
- **[Decision name]**: [full criteria]
[Inline; only if this ho resolves decisions]

**Possible split:** [if this ho is a candidate to split]

## Phase 1 — [Name]

[Phase paragraph]

*Release on phase complete: v0.Y*

### ho-01 — [Title]
[same structure]

### ho-02 — [Title]
[same structure]

[... continues through all phases ...]

**Phase boundary — replan checkpoint.** [Inline marker for explicit checkpoints]

## What's NOT in this sequence

[Explicit deferral list — what's tracked for v1.5 / post-v1.]

## Replan checkpoints

[Consolidated list of pause points with explanations]

## Numbering and insertion

[How the numbering scheme handles splits and insertions]

## Anticipated splits and insertions

[Concrete candidates by pattern: combined hos, replan-checkpoint
 insertions, real-world-data-driven insertions]

## Other deferred decisions

[Decisions that don't tie to a specific v1 ho — visual identity,
 post-v1 features, holdout questions]

## Dependency summary

[ASCII diagram showing phases, hos, replan checkpoints, release tags]

## What to do with this document

[Closing direction: how the overview gets used, how it gets updated]
```

## Anti-Patterns the Skill Refuses

These are non-negotiable. The skill does not produce them regardless of what the source documents contain.

- **Pulling per-ho deep scope into the overview.** The overview is the map. Detailed bullet lists, complete test specifications, full implementation notes — those belong in the per-ho dandori spec at authoring time. Each ho in the overview gets a narrative + light scope + decisions; not a complete spec.
- **Master tables of decisions disconnected from hos.** Decisions render inline with their resolving ho. The practitioner shouldn't have to flip to a separate section to see what decisions a ho is responsible for.
- **Time estimates per ho.** The methodology is practice-based, not throughput-based. Estimating completion dates produces commitments the practice doesn't honor and isn't supposed to honor.
- **Stage labels per ho** (Shu/Ha/Ri). Decorative and flattening. The practitioner's stage is the practitioner's, not the ho's.
- **Industry jargon** ("dogfood," loose "MVP," "shipping" with vague meaning). Plain language carries further. Define terms or use clearer alternatives.
- **Hos numbered without ho-00.** Every project gets a ho-00 (project setup). The skill always includes it.
- **Linear sequence with no phase structure.** Phases make the architecture visible at the planning level; flat lists hide the structure that makes the build legible.
- **Pre-emptive evaluation in phase paragraphs.** "The most important phase," "the critical foundation" — cut. The phase either is critical and shows it through its work, or it isn't.

## Voice and Style Notes

- **Present tense throughout**, including narratives that describe future work.
- **Concrete over abstract.** Name specific files, models, components, decisions where the system design has named them.
- **Build-drop sentences** (long sentence accumulating specifics, followed by a short declarative landing the argument) are the practitioner's voice and welcome.
- **Em-dashes Chicago style** (no spaces): `word—word` not `word — word`. Some practitioners prefer space-padded; honor their preference if expressed.
- **Decision callouts use bold + colon**: `**Reranker model**: bge-reranker-base vs bge-reranker-v2-m3...`
- **Possible split callouts use bold + colon**: `**Possible split:** if X turns out larger than expected, split into ho-N.1 and ho-N.2.`
- **Replan checkpoints use bold + dot**: `**Phase boundary — replan checkpoint.** [reason]`

## What's Out of Scope for This Skill

- **Per-ho dandori spec authoring.** That's the `ho-kamae-5-authoring-collaborator`, which operates under dandori discipline. The dandori toolkit lives embedded inside that skill at `ho-kamae-5-authoring-collaborator/dandori/` (DANDORI.md, FORMAT.md, KOKOROE.md, examples/), scoped to Ho System conventions. The overview is the map; per-ho dandori specs are the territory.
- **System design revision.** If the overview reveals a gap in the architecture, the skill flags this; it does not edit the system design.
- **README revision.** Same — flag if needed; don't edit.
- **Project type variant detection.** The ho-overview shape is roughly the same across project types (Foundation → ... → Acceptance). Variant differences live in the README skill, not here.
- **Skill design or onboarding.** Practitioner-scope concerns; different skills entirely.

## Closing Discipline

Every ho overview should be readable end-to-end by a practitioner who has been away from the project for two weeks. They should be able to identify exactly where the build is, what comes next, what decisions are pending, and what the next replan moment will surface. If the overview cannot do this, it's too sparse. If it tries to do more — full implementation specs, deep technical detail, complete test plans — it has wandered into per-ho dandori spec territory.

The overview is the map. The hos are the territory. Per-ho dandori specs are how the territory gets walked. The overview's job is to set up dandori for success: every ho the overview names should be small enough to fit one dandori session, with scope boundaries clear enough that the per-ho authoring conversation can close the remaining gaps without revisiting the architecture.

Keep the map and the territory honest with each other.
