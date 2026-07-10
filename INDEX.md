---
id: "0.1"
title: "Ho System Index"
type: foundation
stage: n/a
status: stable
tags: [ho-system, navigation, index]
---

# Ho System Index

**Master navigation map for the complete Ho System framework.**

---

## How to Use This Index

Every document in the Ho System has an **ID** in the format `{layer}.{sequence}` — the layer number locates it in the hierarchy below, the sequence number reflects reading order within that layer. IDs appear in each document's frontmatter and throughout this index.

**The layers, each with a distinct role:**

| Layer | What it contains | Who needs it |
|-------|-----------------|--------------|
| **0. Repo Meta** | The map, the changelog, the contribution guide | Anyone orienting to the repository itself |
| **1. Foundation** | Core philosophy and evidence | Any new reader; the conceptual anchor |
| **2. Structure** | Specifications for how the system works | Practitioners and ho authors |
| **3. Templates** | Fill-in structures for hos, devlogs, seeds | Anyone starting a session or project |
| **4. Guides** | Practical orientation | New practitioners |
| **5. Examples** | Concrete instances of the methodology | Learners and facilitators |
| **6. Agent Tasks** | Delegated implementation specs | AI agents and their supervisors |
| **8. Practitioner** | The operating discipline and environment architecture | Practitioners configuring their practice |
| **9. Skills** | The skill catalog that operationalizes the methodology | Practitioners and their agents |

Layer **7 (Project Artifacts)** is retired (2026-07-03, ho-09): its occupants were
archived or live gitignored in `ho-process/`. The number stays dead — no new 7.x IDs are
assigned (forward-only).

**Entry points by intent:**

- *New to the system?* → Start at [1.1](framework/the-ho-system.md)
- *Starting a project?* → Read [2.1](framework/structure/kamae-project-framing.md) → [2.2](framework/structure/project-arc.md) → open [3.2](framework/templates/ho-seed-template.md)
- *About to run a session?* → Go to [3.1](framework/templates/template-selection-guide.md)
- *Unclear on a concept?* → Scan Layer 2 by topic

---

## 0. Repo Meta

The repository's own navigation and record-keeping.

| ID | Document | Description |
|----|----------|-------------|
| 0.1 | [Ho System Index](INDEX.md) | This map — master navigation for the framework |
| 0.2 | [Changelog](CHANGELOG.md) | The framework's version story — structural changes, newest first |
| 0.3 | [Contributing](CONTRIBUTING.md) | What contributions the framework wants and how to send them |

---

## 1. Foundation

Core philosophy and evidence base. Everything else is grounded here.

| ID | Document | Description |
|----|----------|-------------|
| 1.1 | [The Ho System](framework/the-ho-system.md) | Full methodology overview — the definitive introduction |
| 1.2 | [Foundations & Evidence](framework/ho-foundations-evidence.md) | Intellectual basis, research grounding, and pilot evidence |
| 1.3 | [Glossary](framework/glossary.md) | The framework's working vocabulary — every term, with its defining document |

---

## 2. Structure

Conceptual specifications. Ordered to follow the practitioner's actual experience — framing before building, unit before progression, practice before quality.

| ID | Document | Description |
|----|----------|-------------|
| 2.1 | [Kamae: Project Framing](framework/structure/kamae-project-framing.md) | Getting and staying in stance — the six-link Kamae chain, from idea to buildable plan to build-time memory |
| 2.2 | [Project Arc](framework/structure/project-arc.md) | How hos sequence into complete projects |
| 2.3 | [Ho Structure](framework/structure/ho-structure.md) | The fundamental unit of work — what a ho is and isn't |
| 2.4 | [Shu-Ha-Ri Progression](framework/structure/shu-ha-ri.md) | How the system adapts as the practitioner develops |
| 2.5 | [Tiered Understanding](framework/structure/tiered-understanding.md) | Framework for calibrated comprehension |
| 2.6 | [The Devlog](framework/structure/devlog.md) | Structured reflection as a learning practice |
| 2.7 | [Verification Practices](framework/structure/verification-practices.md) | Quality assurance in AI-augmented development |
| 2.8 | [Ho-Task Decomposition](framework/structure/ho-task-decomposition.md) | How hos and agent tasks compose one architectural thought |
| 2.9 | [Artifact Type Registry](framework/structure/artifact-type-registry.md) | The taxonomy of every artifact type the practice produces |
| 2.10 | [Kamae Addenda](framework/structure/kamae-addenda.md) | Mid-build architectural decisions without rewriting history |
| 2.11 | [Design Work](framework/structure/design-work.md) | Design tuning — the design method; design that happens partly outside the chain |
| 2.12 | [External-Project Contribution](framework/structure/external-contribution.md) | Running Ho against someone else's codebase |
| 2.13 | [The Idea Log](framework/structure/idea-log.md) | The forward-only idea/feature backlog, reviewed at every ho boundary |
| 2.14 | [Cross-Session Continuity](framework/structure/cross-session-continuity.md) | Carrying the build's thread across sessions — the universal state-summary block and the State Memory (Kamae 6) that holds it, with their freshness, hot/cold, and compaction disciplines |

---

## 3. Templates

Reusable document structures. Start with the selection guide if you're unsure which to use.

**Navigation**

| ID | Document | Description |
|----|----------|-------------|
| 3.1 | [Template Selection Guide](framework/templates/template-selection-guide.md) | Which template to use and when |

**Kamae (Project Framing)**

| ID | Document | Description |
|----|----------|-------------|
| 3.2 | [Project Seed Template](framework/templates/ho-seed-template.md) | First Kamae document — project philosophy and framing |

**Ho Sessions**

| ID | Document | Description |
|----|----------|-------------|
| 3.3 | [Shu Ho Template](framework/templates/shu-ho-template.md) | Prescriptive learning sessions |
| 3.4 | [Ha Ho Template](framework/templates/ha-ho-template.md) | Decision-driven development sessions |
| 3.5 | [Ri Ho Template](framework/templates/ri-ho-template.md) | Practitioner-level recording format |
| 3.9 | [Design Ho Template](framework/templates/design-ho-template.md) | In-chain wrapper for landings and design-session batches |
| 3.10 | [Autonomous Build Kickoff Template](framework/templates/autonomous-kickoff-template.md) | The prompt that starts a fully autonomous build — wires 2.14 from the first message (draft, pending first validating run) |

**Devlogs (Reflection)**

| ID | Document | Description |
|----|----------|-------------|
| 3.6 | [Shu Devlog Template](framework/templates/shu-devlog-template.md) | Structured reflection for shu stage |
| 3.7 | [Ha Devlog Template](framework/templates/ha-devlog-template.md) | Decision record for ha stage |

**Delegation**

| ID | Document | Description |
|----|----------|-------------|
| 3.8 | [Agent Task Specification](framework/templates/agent-task-spec.md) | Delegating implementation work to AI |

---

## 4. Guides

Practical orientation for new and continuing practitioners.

| ID | Document | Description |
|----|----------|-------------|
| 4.1 | [Getting Started](guides/getting-started.md) | Draft — the first arc, concretely: install, seed, walk the chain, open and close the first ho |
| 4.2 | [Choosing a Project](guides/choosing-a-project.md) | Draft — what makes a good first project, sizing, the black-box discipline, worked shapes |
| 4.3 | [AI Collaboration](guides/ai-collaboration.md) | Draft — the operating posture: planning mode, watching, bounded sessions, verification, ownership |

---

## 5. Examples

Concrete instances of the methodology in practice.

| ID | Document | Description |
|----|----------|-------------|
| 5.1 | [Learning Process Reflections](examples/learning-process.md) | Post-project learning reflections |
| 5.2 | [Kanyō Pilot](examples/kanyo-pilot/README.md) | Production falcon detection system — the founding project |
| 5.3 | [pālana Pilot — Continuity Discipline](examples/palana-autonomous/continuity-discipline.md) | The first autonomous build's four-layer continuity system, as a repeatable process + proposed framework additions |

---

## 6. Agent Tasks

Delegated implementation tasks handed to AI agents.

| ID | Document | Description |
|----|----------|-------------|
| 6.1 | [Integrate Verification Practices](agent-tasks/agent-task-integrate-verification-practices.md) | Task spec for verification integration into the framework |
| 6.2 | [Integrate Ho-Task Decomposition](agent-tasks/agent-task-2026-05-25-integrate-ho-task-decomposition.md) | Task spec for placing the ho-task-decomposition document into the framework |
| 6.3 | [Fable Audit Brief](agent-tasks/fable-audit-brief.md) | Historical — the 2026-07 framework-audit brief handed to Fable |
| 6.4 | [Fable Audit Prior Findings](agent-tasks/fable-audit-prior-findings.md) | Historical — prior-audit findings packaged as context for the Fable audit |
| 6.5 | [Opus Audit Merge Brief](agent-tasks/opus-audit-merge-brief.md) | Historical — the merge-session brief that turned audit findings into decisions D1–D20 |

---

## 7. Project Artifacts — retired

Retired 2026-07-03 (ho-09). The layer held active project seeds and working documents;
its occupants were archived to `practitioner/archive/` or live gitignored in
`ho-process/`. No new 7.x IDs are assigned.

---

## 8. Practitioner

The operating discipline and the environment it runs in — practitioner-scope, not
project-scope.

| ID | Document | Description |
|----|----------|-------------|
| 8.1 | [The Operating Discipline](practitioner/operating-discipline.md) | The operational rules a practitioner works under — verification, posture, workflow |
| 8.2 | [Operating Discipline — Rationale](practitioner/operating-discipline-rationale.md) | The philosophy and argument grounding the operational rules |
| 8.3 | [Environment Architecture](practitioner/SKILLS-ARCHITECTURE.md) | How the practitioner's setup fits together — the four locations and their roles |
| 8.4 | [Module Structure](practitioner/module-structure.md) | The `~/.claude/` layout — files, scopes, and what imports what |

---

## 9. Skills

The skills that operationalize the methodology. Individual skills are cataloged by 9.1,
not indexed row-by-row.

| ID | Document | Description |
|----|----------|-------------|
| 9.1 | [Ho System Skills — Overview](skills/ho-skill-overview.md) | The skill catalog — what exists, what each one does, and where it sits |
