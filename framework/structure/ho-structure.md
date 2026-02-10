# Ho Structure

## The Fundamental Unit of Work

---

## 1. What Makes a Ho a Ho

A ho (歩) is a bounded unit of work that produces something real and leaves a record of what happened. That definition holds across all three stages of the shu-ha-ri progression, even though the shape of the work changes dramatically.

Five properties are invariant. If a piece of work has all five, it's a ho. If it's missing one, it's something else — a task, a note, a session that wasn't captured.

### 1.1 Bounded

A ho has edges. It starts somewhere and it ends somewhere. The learner (or practitioner) can tell when they've entered a ho and when they've left it.

In shu, the boundary is strict: ~2 hours, prescribed by the author. In ha, it's a guideline: 2–4 hours, with conscious escalation past 4. In ri, there's no time boundary at all — but the work is still scoped. "Fix the departure clip anchoring" is bounded even if it takes 30 minutes or 6 hours. "Make the system better" is not a ho.

The boundary exists so the learner can commit, reflect, and reset before the next unit of work. Unbounded work produces unexamined work.

### 1.2 Deliverable

A ho produces something that exists outside the ho document itself. Working code. Passing tests. A deployed service. A configured system. A designed architecture with documented decisions. The deliverable is not a document _about_ work — it is the work.

The deliverable changes character across stages. In shu, it's typically working code that passes verification checks. In ha, it's a decision record AND working code. In ri, it's a system change documented for future maintainers. But in all cases: something concrete was produced.

A ho that produces only learning ("I now understand Docker") but no artifact is incomplete. The understanding should be _demonstrated_ through the artifact.

### 1.3 Traceable

Every ho connects to the actual changes it produced. At minimum: a commit hash or range. Ideally: a file manifest showing what was added, modified, and removed.

Traceability serves two audiences. The learner, who can revisit the work and see exactly what they built. And the future maintainer (often the learner's future self), who needs to understand what changed and why when something breaks six months later.

A ho without traceability is a story about work. A ho with traceability is evidence of work.

### 1.4 Reflective

Every ho includes honest self-assessment. What was built, what was learned, what was hard, what's still unclear. The form of reflection varies by stage — a full devlog in shu, a decision record in ha, a brief notes section in ri — but the practice of looking back before moving forward is non-negotiable.

Reflection is what separates "I used AI to build something" from "I developed as a practitioner by building something with AI." Without it, the Ho System is just a task tracker.

### 1.5 Sequenced

A ho exists within a larger arc. It has a number, it has predecessors and successors, and its position in the sequence means something. A ho can be planned or emergent, but it's never orphaned — it connects to the work that came before and the work that comes after.

The sequencing system is described in detail in §3 below.

---

## 2. What a Ho Is Not

**Not a task.** A task is a single action: "add a PENDING_STARTUP state." A ho is a session of work that may contain multiple tasks, produces a deliverable, and includes reflection. An [[agent-task-spec|agent task]] (framework/templates/agent-task-spec.md) is the right format for bounded single actions.

**Not a tutorial.** A tutorial teaches a concept using examples. A ho builds something real. The learner's project advances with every ho — it's not a sandbox exercise that gets thrown away.

**Not a sprint.** A ho is a single session, not a time-boxed collection of work items. A sprint might contain multiple hos. A ho never contains multiple sprints.

**Not a document.** The ho document records the work; it isn't the work. The deliverable is the work. The document is the trace.

---

## 3. The Numbering System

### 3.1 The Principle

Ho numbers encode _relationship_, not just sequence.

A number tells you three things: where the ho sits in the overall project arc (its position), what work it grew from (its parentage), and when it was conceived relative to the original plan (planned vs. emergent). The numbering system is simple enough to use without thinking and expressive enough to capture how real projects actually evolve.

### 3.2 The Grammar

A ho number has up to three levels:

```
[Major].[Minor].[Sub]
```

|Level|Name|What it means|Example|
|---|---|---|---|
|**Major**|The planned sequence|The original ho sequence from the project arc|`05`|
|**Minor**|The branch|Work that emerged from a major ho|`05.6`|
|**Sub**|The leaf|Work that emerged from a branch|`05.72`|

**Major numbers** are assigned during project planning (in the [[kamae-project-framing|Kamae]] (framework/structure/kamae-project-framing.md) phase). They represent the intended arc: Ho 01, Ho 02, Ho 03... These are the steps the author or practitioner _planned to take_.

**Minor numbers** appear when reality diverges from the plan. When Ho 05 (Deployment Verification) spawns additional work — dev testing, architecture redesign, state machine simplification — those become 05.5, 05.6, 05.7. The minor number says: "this work grew from Ho 05, but it wasn't in the original plan."

**Sub numbers** appear when a branch spawns its own children. When Ho 05.7 (State Machine Redesign) leads to a stream outage fix and a startup confirmation system, those become 05.71 and 05.72. The sub number says: "this work grew from Ho 05.7."

Three levels is the maximum. If a sub wants to branch further, that's a signal the project arc needs restructuring — either the original major number was too broad, or a new major number should be assigned.

### 3.3 Special Positions

**Zero (00):** The Ho Overview. Every project arc starts here. Ho 00 is the plan itself — the sequence map, the phase assignments, the dependency graph. It's the document the author produces during Kamae to define the arc.

**Point-five (0.5):** The conventional position for an inserted ho between two planned majors. Ho 0.5 (Tool Mastery) sits between the overview (00) and the first real work (01). It's reserved for "we need a step here that wasn't in the original plan, and it logically precedes the next major."

**The .0 and .00 positions** within a branch are never used. Ho 05.0 doesn't exist — it would be Ho 05. Ho 05.70 doesn't exist — it would be Ho 05.7. The number extends only when new work branches off.

### 3.4 The Rules

Five rules govern the numbering system. They're simple, but they matter.

**Rule 1: Numbers are permanent.** Once assigned, a ho number never changes and is never reused. If Ho 05.6 is abandoned, 05.6 stays dead. The next piece of work gets 05.7, not 05.6 recycled. This preserves the historical record — commit messages, cross-references, and devlogs all point to stable addresses.

**Rule 2: Numbers encode parentage.** Ho 05.71 is a child of 05.7, which is a child of 05. The number itself tells you the lineage. You can read the ancestry from the digits without checking any other document.

**Rule 3: Minor numbers don't need to be sequential.** Ho 05 might branch into 05.5, 05.6, and 05.7 without ever having 05.1 through 05.4. The gaps are fine — they mean "nothing needed to exist there." Similarly, 06 branched into 06.1 and 06.5, skipping 06.2 through 06.4. Don't fill gaps retroactively.

**Rule 4: Planned numbers are provisional.** The Ho Overview assigns major numbers as a plan: Ho 01 through Ho 11. But the plan is a guide, not a contract. The Kanyō pilot planned 11 major hos and produced 19 documents across 3 levels of branching. The numbering system absorbed this without breaking because minor and sub numbers extend the plan rather than disrupting it.

**Rule 5: Three levels maximum.** Major.Minor.Sub. No deeper. If sub-work wants to branch further, either promote it to a new major number or restructure the arc. Deep nesting makes numbers unreadable and signals that the work decomposition needs rethinking, not more digits.

### 3.5 Filename Convention

Ho documents are named with their number and a short descriptive slug:

```
ho-[number]-[slug].md
```

The number uses hyphens for dots and underscores for internal separation:

|Ho Number|Filename|
|---|---|
|00|`ho-00-overview.md`|
|0.5|`ho-0_5-tool-mastery.md`|
|01|`ho-01-git-good.md`|
|05.7|`ho-05_7-state-redesign.md`|
|05.72|`ho-05_72-startup-confirmation.md`|
|06.13|`ho-06_13-arrival-confirmation-system.md`|

The slug is kebab-case, brief, and descriptive. It should be recognizable at a glance in a directory listing or commit message.

### 3.6 What the Numbers Tell You

The most important property of the numbering system is that the _gap between planned and actual_ tells the project's real story.

The Kanyō plan:

```
00  01  02  03  04  05  06  07  08  09  10  11
```

The Kanyō reality:

```
00
 └─ 0.5
01
02
03
04
05
 ├─ 05.5
 ├─ 05.6
 └─ 05.7
     ├─ 05.71
     └─ 05.72
06
 ├─ 06.1
 │   ├─ 06.12
 │   └─ 06.13
 ├─ 06.5
 │   └─ 06.51
07
```

You can read this tree and immediately see:

- **Ho 05 was a turning point.** Deployment verification spawned three branches of architectural work. The system needed more than planned.
- **Ho 06 was a branching arc.** GUI work split into admin (06.1) and public (06.5), each with their own children. This was a domain complex enough to become a sub-project.
- **Ho 05.7 produced follow-on work.** State machine redesign led to two more targeted fixes — the kind of cascade that happens when a good architectural decision reveals previously hidden problems.
- **Hos 01–04 ran clean.** The foundation phase proceeded as planned. No branches, no emergent work. The early scaffolding held.
- **Hos 08–11 were never reached** (or not yet). The planned arc extended further than the pilot executed.

A facilitator reviewing this tree knows where the learner struggled (05's complexity), where they thrived (01–04's clean execution), and where they developed architectural independence (the 05.7 branch — self-directed ri-stage work).

An experienced practitioner reviewing their own tree sees where the plan was wrong (05 was underscoped), where emergent work added the most value (05.6 Architecture Redesign removed 800 lines), and where the project's actual shape diverged from its imagined shape.

### 3.7 Numbering and Shu-Ha-Ri

The character of branching correlates with stage:

**Shu-stage hos rarely branch.** The author planned the work; the learner follows the plan. If a shu ho does branch, it usually means the author underscoped it (too much for one session) and the minor number captures the overflow.

**Ha-stage hos often branch.** The learner is making decisions, and decisions produce emergent work. Ho 05.6 (Architecture Redesign) was a ha-stage decision that naturally led to Ho 05.7 (State Redesign) — one design decision revealing the next problem.

**Ri-stage hos branch into tasks more than sub-hos.** The practitioner's work produces follow-on items, but they're often agent tasks rather than full ho sessions. The sub-numbers that do appear (05.71, 05.72) tend to be targeted fixes, not exploratory sessions.

---

## 4. How Structure Varies Across Stages

The five invariant properties (bounded, deliverable, traceable, reflective, sequenced) hold at every stage. Everything else flexes.

### 4.1 The Structural Comparison

|Property|Shu|Ha|Ri|
|---|---|---|---|
|**Organizing unit**|Parts (4–9, author-defined)|Phases (3, fixed: Think → Execute → Reflect)|Sections (Problem → Solution → Changes → Results)|
|**Duration**|~2 hours (strict)|2–4 hours (guideline)|No limit|
|**Who scopes**|The ho author|The learner|The practitioner|
|**Primary artifact**|Working code|Decision documentation + working code|Change record|
|**AI relationship**|"Review and verify"|"Think together, then execute"|Implementation accelerator|
|**Reflection format**|Full devlog (learning journal)|Decision-focused devlog|Notes section (optional)|
|**Commit rhythm**|After every part|At natural breakpoints|Practitioner's discretion|
|**Tier declarations**|Author assigns|Learner declares|Internalized|
|**Confidence scale**|Measures understanding|Measures judgment|Not used|
|**Template**|Shu Ho Template|Ha Ho Template|Ri Ho Template|

See the [[template-selection-guide|Template Selection Guide]] (framework/templates/template-selection-guide.md) for help choosing between these.

### 4.2 What's Invariant

Regardless of stage, every ho:

1. **Has a number** — positioned in the project arc
2. **Produces a deliverable** — something testable exists afterward
3. **Is committed to version control** — with a meaningful message
4. **Includes self-assessment** — the practitioner reflects on what happened
5. **Can be reviewed** — someone else can read the document and understand the work

These five are the minimum bar. A piece of work that meets all five is a ho. A piece of work that misses any of them is incomplete.

### 4.3 What Dissolves

As the learner progresses through stages, structural elements dissolve — not because they're abandoned, but because they're internalized:

- **Verification questions** exist in shu, are implicit in ha, and are absent in ri (the practitioner self-verifies)
- **Understanding tiers** are declared in shu, self-assigned in ha, and invisible in ri (the practitioner knows what they know)
- **AI collaboration guidance** is explicit in shu, phase-specific in ha, and absent in ri (the practitioner knows when to think and when to delegate)
- **Time boundaries** are strict in shu, flexible in ha, and gone in ri (the practitioner manages their own energy)
- **Devlog structure** is templated in shu, decision-focused in ha, and merged into the ho document itself in ri

This dissolution is the point. Structure exists to build habits. Once the habits are built, the structure steps aside.

---

## 5. The Ho Document

Every ho produces a document. The document's shape varies by stage (see the templates), but its purpose is constant: to record what happened so that anyone — the learner, the facilitator, a future maintainer, or the practitioner's future self — can understand the work.

### 5.1 The Header

Every ho document begins with identification:

```markdown
# Ho [Number]: [Title]
```

The title is brief and descriptive. It names the _work_, not the lesson. "Git Good" not "Learning Git Basics." "Architecture Redesign" not "Thinking About System Architecture." The title should make sense in a directory listing, a commit message, and a conversation.

Additional header fields vary by stage:

|Field|Shu|Ha|Ri|
|---|---|---|---|
|Date|✓|✓|✓|
|Duration|✓ (actual vs. target)|✓ (actual)|—|
|Status|✓|✓|✓|
|Decision Required|—|✓|—|
|Commit|—|—|✓|

### 5.2 The Body

Shu: Prerequisites → Parts → Completion Checklist → Understanding Verification → Devlog Ha: Context → Phase 1 (Think) → Phase 2 (Execute) → Phase 3 (Reflect) → Devlog Ri: Problem → Solution → What Changed → Results → Notes

### 5.3 The Devlog

The devlog deserves special attention because it changes the most across stages and it's the primary evidence of learning.

**In shu,** the devlog is a learning journal. What was built, what understanding tiers were achieved, challenges encountered, key learnings, files changed, confidence level. It's structured by template because the learner is developing the _habit_ of reflection.

**In ha,** the devlog is a decision record. What was decided, what alternatives were considered, what the implementation revealed about the decision, what was removed (removal is a ha signature), how AI collaboration split between thinking and agent modes, confidence in judgment.

**In ri,** the devlog dissolves into the ho document. Problem, solution, what changed, results — that IS the record. A separate devlog would duplicate it. The reflection habit is internalized; it doesn't need its own section.

---

## 6. Relationship to Commits

Hos and commits are not the same thing, but they're deeply linked.

**A ho typically produces multiple commits.** In shu, the guidance is to commit after every part (4–9 commits per ho). In ha, the learner commits at natural breakpoints. In ri, the practitioner commits at their own rhythm.

**The first commit in a ho should be traceable.** Whether through a commit message that names the ho ("Ho 05.7: begin state machine redesign") or through the ho document's commit hash field, someone reading the git log should be able to connect commits to hos.

**The ho document itself is committed.** The document is a first-class project artifact, versioned alongside the code. It's not metadata stored somewhere else — it lives in the repository, in a `devlog/` or `hos/` directory.

**Commit messages are documentation.** They're not "fixed stuff" or "updated files." They describe what changed and why, at a level appropriate to the change. The ho document provides the narrative; the commit messages provide the granular record.

---

## 7. Related Framework Documents

- [[design-seed|Design Seed]] (framework/design-seed.md) — §3.1 introduces the ho as unit of work
- [[shu-ha-ri|Shu-Ha-Ri Progression]] (framework/structure/shu-ha-ri.md) — How ho structure adapts across stages
- [[kamae-project-framing|Kamae: Project Framing]] (framework/structure/kamae-project-framing.md) — Where major ho numbers are assigned
- [[tiered-understanding|Tiered Understanding]] (framework/structure/tiered-understanding.md) — How understanding depth is tracked within hos
- [[project-arc|The Project Arc]] (framework/structure/project-arc.md) — How hos sequence into complete arcs
- [[template-selection-guide|Template Selection Guide]] (framework/templates/template-selection-guide.md) — Choosing the right template

---

_This document is part of the Ho System framework. It defines the structural specification for the ho as a unit of work. For guidance on designing effective ho sequences — including pacing, difficulty curves, and learner-appropriate scoping — see the facilitation layer (proprietary)._