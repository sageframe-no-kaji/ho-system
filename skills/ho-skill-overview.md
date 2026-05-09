# Ho System Skills — Overview

A catalog of the skills that operationalize the Ho System methodology, organized by scope and status. Each skill is a `.skill` package that produces or maintains a specific artifact in the methodology. Together they cover the work of getting a practitioner set up and producing a project end-to-end.

This document is for practitioners who want to understand what skills exist, what each one does, when to use it, and where it sits in the larger architecture. It is also the working catalog the framework uses to track which skills are built, which are planned, and which are deferred.

---

## The Two Scopes

The Ho System operates at two distinct scopes. Skills are organized accordingly.

**Practitioner scope.** How the practitioner works, regardless of project. The operating discipline, the IDE configuration, the agent instructions, the practitioner's persistent preferences. Practitioner-scope artifacts travel with the practitioner across every project. Practitioner-scope skills are typically run once per practitioner-tool combination, not once per project.

**Project scope.** What gets built. The Kamae chain — five documents that frame, sequence, and execute the work of a specific project. Project-scope skills are run for each new project, with each project getting its own complete chain.

The two scopes meet at ho-00 (the project setup ho), where the practitioner's operating discipline gets instantiated in a specific repo's `pyproject.toml`, pre-commit hooks, folder structure, and so on. Beyond that meeting point, the scopes operate independently.

---

## Execution Surface — Chat vs IDE

A second, orthogonal axis: where each skill runs. Some skills are conversational by nature; some need direct file system access to do their work.

**Chat-primary skills** are conversational — Think-phase dialog, drafting, structured interviews, synthesis. They produce documents as outputs. The Kamae authoring skills (seed through design) live here. So does DI, which audits prose.

**IDE-primary skills** modify the file system as their primary output: scaffolding repos, configuring environments, executing agent tasks. The setup skills (environment setup, project setup) live here. Agent-task execution itself is IDE-only.

The two axes interact loosely. Most authoring work is chat-primary regardless of scope. Most configuration work is IDE-primary regardless of scope. The boundary cases — skills that have both conversational and file-system work — pick a primary surface and accept some friction on the other. `ho-design-collaborator` is chat-primary with an IDE escape hatch for mid-build invocation.

---

## Practitioner-Scope Skills

Skills that configure how the practitioner works. These produce artifacts that live in the practitioner's tools, not in any specific project.

### ho-environment-setup-collaborator

**Status:** Planned, not built
**Surface:** IDE-primary
**Produces:** IDE-specific agent configuration (CLAUDE.md, codex configuration, VS Code settings, pre-commit hook templates, opinionated lint stack defaults)
**When to run:** Once per practitioner-tool combination. After running, the practitioner's environment knows the operating discipline and the agent will conform to it across projects.

This skill interrogates the practitioner about their stack (language, IDE, model access), reads the operating discipline document, and generates the configuration files that encode the discipline in the practitioner's working environment. The skill is opinionated — about lint stacks, about folder organization, about commit hygiene — and explains its opinions rather than asserting them.

The skill is not meant for true beginners. A practitioner running it should already know what an IDE is, how to install packages, how to configure tools. For practitioners earlier than that, see the deferred Palana skill below.

### ho-onboarding-collaborator (Palana)

**Status:** Deferred — separate project
**Surface:** —
**Produces:** A guided onboarding experience for true beginners (no programming background)
**When to run:** Once for practitioners new to coding entirely.

This is a separate project, not a skill in the same sense as the others. Palana would be an AI-guided course that takes a true beginner from no environment, no language, no GitHub account through to running their first ho. It is referenced here for completeness but is not part of the immediate skill ecosystem.

### ho-stack-selection-collaborator

**Status:** Deferred — only if needed
**Surface:** Chat-primary (if built)
**Produces:** A recommended language and framework stack for a given project
**When to run:** Only when the practitioner is genuinely choosing between languages or frameworks for a new project, and the seed and system design don't already commit to a choice.

This skill exists as a hypothetical because language selection sometimes matters and isn't always obvious. In practice, most practitioners have a default language and the skill is unnecessary. Build only if a real practitioner need surfaces.

---

## Project-Scope Skills (The Kamae Chain)

Skills that produce or maintain the five Kamae documents, plus the project-setup skill that scaffolds the repo. Each project gets its own complete chain.

### ho-seed-collaborator (Kamae 1)

**Status:** Built
**Surface:** Chat-primary
**Produces:** A project seed — the raw idea, problem framing, scope boundaries, audience, vision
**When to run:** At the start of a new project, before any other Kamae work. Also useful for reviewing or revising an existing seed.

The seed is the thinking document. It captures why the project should exist, what it must do, what's in and out of scope, who it's for. Written for the practitioner first; refined for outsider read later. The seed collaborator is a probing partner — pushes back on underdeveloped framings, surfaces assumptions, names what's missing.

### ho-system-design-collaborator (Kamae 2)

**Status:** Built
**Surface:** Chat-primary
**Produces:** A system design document — architecture, components, data flow, technology stack, deployment model, deferred decisions, provisional ho sequence
**When to run:** After the seed is complete. Before the README.

The system design turns the seed's intentions into committed architecture. Components, data structures, technology choices, deployment shape, what's deferred for later resolution. The system design collaborator translates between the practitioner's systems vocabulary and the technical decisions the project requires.

The system design typically includes a provisional ho sequence and a deferred decisions table. These are starting material for Kamae 4 (Ho Overview) — the Ho Overview welcomes them but is not beholden to them.

### ho-readme-collaborator (Kamae 3)

**Status:** Built
**Surface:** Chat-primary
**Produces:** A canonical public README (and optionally a paired `docs/architecture.md`)
**When to run:** After the seed and system design are complete. Also runs in update mode when features ship or the project's framing shifts.

The README is the project's continuously canonical public face. Written in the present tense as if the project exists. Adapts to project type (product, utility, infrastructure, library) — see the skill's project-type guide for detail. Strips frontmatter, marketing-speak, and over-formatted decoration; keeps the practitioner's voice and the project's body and soul.

### ho-overview-collaborator (Kamae 4)

**Status:** Built
**Surface:** Chat-primary
**Produces:** An ho overview document — phases, hos, dependencies, decisions inline with their resolving ho, replan checkpoints, anticipated splits, release tags per phase
**When to run:** After the README is complete. Also runs in update mode when hos split, insert, reorder, or surface scope shifts during construction.

The ho overview is the build's directional plan. It identifies phases first (clusters of work that share a theme), then identifies hos within phases. Decisions deferred from the system design appear inline with the ho that resolves them — not in a master table. Phase boundaries are also natural release boundaries (v0.1, v0.2 ... v1.0).

### ho-project-setup-collaborator

**Status:** Planned, not built
**Surface:** IDE-primary
**Produces:** A repo scaffold ready for ho-00 — `pyproject.toml`, smoke tests, project-level `CLAUDE.md`, `ho-process/hos/` and `ho-process/agent-tasks/` directory structure, lint stack configured, pre-commit hooks installed, initial commit.
**When to run:** At project start, before authoring any per-ho document. Update mode handles scaffold adjustments.

The companion to ho-design-collaborator. ho-design-collaborator does chat-side authoring of ho-00 (concept primers, shape decisions); ho-project-setup-collaborator does the IDE-side scaffolding work. They run in sequence — setup first, design against the scaffold it produces.

ho-design-collaborator's pre-condition check refers to this skill: if the scaffold isn't in place, the practitioner runs ho-project-setup-collaborator first.

### ho-design-collaborator (Kamae 5)

**Status:** Built
**Surface:** Chat-primary, with IDE escape hatch
**Produces:** Per-ho documents (orientation, ha, ri, or rarely shu shape) — the bounded scope for an individual working session. Optionally produces sibling agent task documents (`Ho-NN-AT-MM.md`) for hos whose work decomposes into bounded executable units.
**When to run:** At the start of each new ho's session, against that ho's slot in the ho-overview. Update mode: when scope creeps mid-execution and the ho should split, when the Reflect phase needs filling in post-execution, or when new agent tasks emerge.

The per-ho document is what the practitioner and agent work against in a single session. It takes a position from the ho-overview and turns it into something concrete — narrative, dependencies, in-scope, out-of-scope, what done means, decisions to resolve, verification gates.

The skill selects among four document shapes:

- **Orientation** — for ho-00, learning hos, and replan checkpoints. Concept primers, project shape, handoff. No execution phase.
- **Ha** — for building hos with architectural decisions. Three-phase structure: Think (decisions) → Execute (work, possibly via agent tasks) → Reflect (post-execution).
- **Ri** — for surgical fixes and well-defined work. Problem → Solution → Changes → Results.
- **Shu** — rare, for handing the framework to a learner. Author-prescribed parts walked through.

The skill loads conditional references based on shape: `learning-interview.md` for orientation work, `agent-task-format.md` when the Think phase decomposes into bounded tasks, `ho-shape-templates.md` for every authoring. The agent-task-format reference is intentionally portable — it contains behavioral guidelines (Karpathy's four principles), format conventions, and translation moves that work outside the Kamae chain. It may extract into its own skill (`agentic-prompt`) if standalone-task pressure shows up.

Per-ho documents are read by both the practitioner returning to the project and the agent arriving fresh into a session.

---

## Supporting Skills

Skills that aren't part of the Kamae chain but support the practice.

### di (Destructive Interference audit)

**Status:** Built
**Surface:** Chat-primary
**Produces:** A diagnostic editing pass against the eight-class DI taxonomy — flags structural failures in AI-influenced prose
**When to run:** After drafting any substantial writing artifact (essay, framework document, README, ho overview), before publication or commit.

DI catches discourse-level failures that survive vocabulary policing — pre-emptive evaluation, performed urgency, throat-clears, false-balance hedges, significance stamps, mirror summaries, emotional instructions, manufactured transitions. The skill returns severity-rated findings with delete/replace/rewrite edits in the practitioner's voice. Used as a quality gate on writing produced by skills earlier in the chain.

---

## The Order of Operations

For a practitioner new to the methodology, the natural order is:

1. **Read the operating discipline.** Internalize what discipline the methodology assumes. (Currently at `practitioner/the-operating-discipline.md`.)
2. **Run ho-environment-setup-collaborator** (when built; in IDE). Configure your IDE and agent to know the discipline.
3. **For each new project:**
   - Run **ho-seed-collaborator** in chat — Kamae 1.
   - Run **ho-system-design-collaborator** in chat — Kamae 2.
   - Run **ho-readme-collaborator** in chat — Kamae 3.
   - Run **ho-overview-collaborator** in chat — Kamae 4.
   - Run **ho-project-setup-collaborator** in IDE (when built) to scaffold the repo.
   - For each ho in the overview: run **ho-design-collaborator** in chat to author the per-ho document and any agent tasks (Kamae 5), then execute in IDE against the agent task specs.
4. **Run di on substantial writing artifacts** before publication.

For an experienced practitioner who already has an environment configured and is starting a new project, only steps 3 and 4 apply.

For a practitioner who has internalized the methodology to the point of Ri-stage compression (see "Three Hours" essay), individual Kamae steps may collapse into single working sessions. The skills remain useful as scaffolding but are not strictly required for every project.

---

## How Skills Compose

Skills are deliberately separated by scope, by document, and by surface. They do not depend on each other at runtime — each skill is self-contained and can be used independently. They depend on each other through their _outputs_. The system design collaborator reads the seed; the README collaborator reads the seed and the system design; the ho overview collaborator reads all three; the design collaborator reads all four plus the project's `CLAUDE.md`.

This composition through outputs (rather than through tool chaining) matches the operating discipline's principle of encoded environment. The artifacts are the persistent layer. The skills produce and maintain artifacts. The agent (and the practitioner) read artifacts and act accordingly. The skills don't need to know about each other; they only need to know about the artifacts they consume and produce.

A consequence of this design: skills can be built and shipped independently. The methodology degrades gracefully when a skill doesn't exist yet — the practitioner does the work by hand, with the artifact serving the same role in the chain. The skills are leverage, not gates.

The same is true across surfaces. A chat-primary skill running in IDE (or vice versa) loses some ergonomics but produces the same artifact. The artifact is the contract; the surface is implementation detail.

---

## Status Summary

| Skill                               | Scope         | Status                      | Surface           | Kamae # |
| ----------------------------------- | ------------- | --------------------------- | ----------------- | ------- |
| ho-environment-setup-collaborator   | Practitioner  | Planned                     | IDE               | —       |
| ho-onboarding-collaborator (Palana) | Practitioner  | Deferred (separate project) | —                 | —       |
| ho-stack-selection-collaborator     | Practitioner  | Deferred (build if needed)  | Chat              | —       |
| ho-seed-collaborator                | Project       | Built                       | Chat              | 1       |
| ho-system-design-collaborator       | Project       | Built                       | Chat              | 2       |
| ho-readme-collaborator              | Project       | Built                       | Chat              | 3       |
| ho-overview-collaborator            | Project       | Built                       | Chat              | 4       |
| ho-project-setup-collaborator       | Project       | Planned                     | IDE               | —       |
| ho-design-collaborator              | Project       | Built                       | Chat (IDE escape) | 5       |
| di                                  | Cross-cutting | Built                       | Chat              | —       |

Built: 6 skills (5 Kamae chain + DI). Planned: 2 skills (env-setup at practitioner scope, project-setup at project scope, both IDE-primary). Deferred: 2 skills (separate-project Palana and conditional stack-selection).

---

## What This Document Does NOT Cover

- **Skill authoring methodology.** How to create new skills, debrief from real work to extract them, package them as `.skill` files. That belongs in a separate document on skill development practice.
- **Skill update and versioning.** As skills evolve through use, their authoritative version updates. The protocol for that — when to revise, when to deprecate, how to communicate changes — is its own concern.
- **The relationship to other Sageframe skills.** The practitioner may have other skills (`atm-brand-system`, `sageframe-cartographer`, `atm-correspondence`) that are part of their broader workflow but not part of the Ho System framework. Those skills are personal/Sageframe, not Ho System.

---

## Related Framework Documents

- [[design-seed|Design Seed]](framework/design-seed.md) — The governing document for the Ho System
- [[kamae-project-framing|Kamae: Project Framing]](framework/structure/kamae-project-framing.md) — The Kamae chain explained
- [[the-operating-discipline|The Operating Discipline]](practitioner/the-operating-discipline.md) — The practitioner-scope canonical document
- [[shu-ha-ri|Shu-Ha-Ri Progression]](framework/structure/shu-ha-ri.md) — How practitioner stage shapes engagement with the methodology

---

_This document is part of the Ho System framework. It catalogs the skills that operationalize the methodology. As skills get built, deferred, or retired, this document updates to reflect current state._
