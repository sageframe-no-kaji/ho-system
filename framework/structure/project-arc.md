# The Project Arc

## How Hos Sequence Into Complete Projects

---

## 1. What an Arc Is

A project arc is the complete sequence of hos that takes a project from idea to running system. It's the shape of the work over time — not a task list, not a curriculum, but a narrative of building that includes plans, surprises, pivots, and the gradual transfer of authority from author to learner.

An arc has three defining characteristics:

**It has a beginning and an end.** The beginning is [[kamae-project-framing|Kamae]] (framework/structure/kamae-project-framing.md) — the framing work that produces the plan. The end is a working system. Not a perfect system, not a feature-complete system, but something that runs in production and does what it was designed to do.

**It follows a characteristic shape.** Regardless of domain, arcs move through the same phases: orientation → foundation → construction → integration → operations. The phases may be different lengths, but the sequence is consistent. You can't integrate before you've constructed, and you can't construct before you've laid the foundation.

**It evolves.** The plan written during Kamae is a starting point, not a script. Real arcs branch, expand, and compress as the project demands and the learner develops. The gap between the planned arc and the actual arc is itself meaningful — it tells the story of what was harder than expected, what was easier, and where the learner's growth outpaced or lagged behind the plan.

---

## 2. The Five Phases

Every project arc passes through five phases. They're not rigid boundaries — work blends between them — but the progression is consistent.

### Orientation (Hos 0–0.5)

**Focus:** Tooling, environment, mental models.

The learner gets their bearings. What tools will they use? How does the development environment work? What's the shape of the project? Orientation hos are almost always shu-stage — the learner is encountering new tools and needs guided setup.

The deliverable isn't impressive. It's "the tools work, I can run a test, I know where things live." But without it, everything that follows is unstable.

**Kanyō evidence:** Ho 0.5 (Tool Mastery) covered Claude Code workflow, Git basics, and development environment setup. Ho 00 (Overview) was the project plan itself — the first output of Kamae.

**Typical duration:** 1–2 hos. Shorter for experienced practitioners starting a new project; longer for learners encountering an entirely new tool ecosystem.

### Foundation (Hos 1–2)

**Focus:** Project structure, core infrastructure, first working component.

The learner builds the skeleton. Directory structure, configuration system, logging, testing framework, the first piece of real functionality. Foundation work is unglamorous but load-bearing — everything that follows depends on these decisions.

Foundation hos are shu-stage. The author prescribes the structure because getting it wrong is expensive and the learner doesn't yet have the judgment to make good structural choices in this domain. The ho says "organize your files this way" and "set up your config system like this" — not because there's only one right answer, but because the learner needs a known-good starting point.

**Kanyō evidence:** Ho 01 (Git Good) established the project structure, virtual environment, configuration system, testing framework, and documentation. Ho 02 (Falcon Vision) built the first real capability — detection using YOLO. Both were author-prescribed, step-by-step, shu-stage work.

**Typical duration:** 2–3 hos. Enough to stand up the project and prove one capability works end-to-end.

### Construction (Hos 3–6)

**Focus:** Primary system capabilities, iterative build-out.

This is where the project takes shape. The learner builds the core features — detection pipeline, event logic, data persistence, notification system, deployment infrastructure. Construction is the longest phase and where the most branching occurs.

Construction hos typically begin shu and shift toward ha as the learner develops competence. Early construction hos are still prescriptive ("here's how to build the live detection pipeline"). Later ones open up ("here's the problem space for the GUI architecture; evaluate the options and decide").

This is also where the plan diverges most from reality. The Design Seed's phase table assigns Hos 3–6 to construction, but the Kanyō pilot produced 12 documents in this range (counting all the branches from 05 and 06). Construction is where complexity lives.

**Kanyō evidence:** Ho 03 (Live Detection & Notification) built the real-time pipeline. Ho 04 (Docker Deploy) containerized the system. Ho 05 (Deployment Verification) was supposed to be a single validation step but exploded into a branching arc: dev testing strategy (05.5), architecture redesign (05.6), state machine redesign (05.7), plus targeted fixes (05.71, 05.72). Ho 06 followed the same pattern: GUI planning (06) branched into admin implementation (06.1), code quality (06.12), arrival confirmation (06.13), public viewer (06.5), and Cloudflare tunnel (06.51).

**Typical duration:** 4–8 major hos, often with branches. The longest phase by far.

### Integration (Hos 7–9)

**Focus:** Assembly, system-level concerns, deployment.

The individual components built during construction are now connected into a working whole. Integration work is about boundaries — how the detection system talks to the notification system, how the Docker container exposes the GUI, how the deployment pipeline handles restarts.

Integration hos are typically ha-stage. The learner is making real architectural decisions: which components need to communicate, through what interfaces, with what failure modes. The decisions are genuine because the system is complex enough that there are real tradeoffs.

**Kanyō evidence:** Ho 07 (YOLO Training) was integration work — connecting a custom-trained model to the existing detection pipeline. The Kanyō pilot didn't reach planned Hos 08–11 because earlier phases consumed more sessions than expected. This is normal.

**Typical duration:** 2–4 hos. Often shorter than planned because construction addressed integration concerns along the way (especially if construction included deployment work like Ho 04).

### Operations (Hos 10+)

**Focus:** Maintenance, extension, optimization, polish.

The system is running. Work shifts from building to operating — bug fixes, performance improvements, new features, documentation for users. This is ri-stage territory: the practitioner owns the system and works on it independently.

Operations may not look like "hos" at all. The formal session structure dissolves into ri-stage documents and agent tasks. The practitioner's work log is a series of Problem → Solution → What Changed records, not guided learning sessions.

**Kanyō evidence:** Much of the Ho 05.7x and 06.1x work was already operational in character — the system was running, and the practitioner was maintaining and extending it. The formal "Operations Phase" that Ho 00 planned as Hos 10–11 never arrived as distinct sessions because operational work was already happening within construction branches.

**Typical duration:** Indefinite. The system is alive; it gets maintained for as long as it's useful.

---

## 3. The Planned Arc vs. the Actual Arc

### 3.1 What Gets Planned

During [[kamae-project-framing|Kamae]] (framework/structure/kamae-project-framing.md), the Ho Overview document lays out the planned arc. This includes:

- A numbered sequence of major hos (typically 8–15)
- Phase assignments (which hos belong to which phase)
- Stage assignments (shu/ha/ri for each ho, provisional)
- Dependencies (what must be true before each ho starts)
- Scope boundaries (what's in the arc vs. what's deferred)

The Kanyō plan looked like this:

```
Phase           Hos        Stage (implied)
─────────────   ─────────  ──────────────
Orientation     0, 0.5     Shu
Foundation      1–3        Shu
Automation      4–6        Shu → Ha
Frontend        7–9        Ha
Polish          10–11      Ha → Ri
```

Clean. Linear. 11 major steps, four phases, a smooth progression from shu to ri. This is a good plan. It's also wrong in the way all plans are wrong — not in its intent but in its assumptions about how the work would actually unfold.

### 3.2 What Actually Happens

Reality introduces three forces that reshape the arc:

**Branching.** A ho spawns work that wasn't in the plan. Ho 05 was supposed to be one session; it became four. Ho 06 was supposed to be one session; it became five. Branching is the most visible way arcs diverge from plans, and it's captured directly by the [[ho-structure|numbering system]] (framework/structure/ho-structure.md): minor and sub numbers extend the plan without disrupting it.

**Stage acceleration.** The learner develops faster (or slower) than expected. The Kanyō plan implied a gradual shu → ha → ri progression across the full arc. In practice, the learner hit ha-stage work by Ho 05 and ri-stage work by Ho 05.7 — much earlier than planned. Stage acceleration means later hos may need different templates than originally assigned.

**Scope discovery.** Building reveals requirements that weren't visible during planning. The Kanyō plan didn't include "arrival confirmation" or "stream outage handling" because those needs weren't apparent until the system was running and producing false positives. These became Hos 06.13 and 05.71 — work that emerged from operating the system, not from the plan.

### 3.3 The Kanyō Arc: Planned vs. Actual

```
PLANNED                          ACTUAL
───────                          ──────

00 ─────────────────────── 00 Overview
                            └─ 0.5 Tool Mastery
01 ─────────────────────── 01 Git Good
02 ─────────────────────── 02 Falcon Vision
03 ─────────────────────── 03 Live Detection & Notification
04 ─────────────────────── 04 Docker Deploy
05 ─────────────────────── 05 Deployment Verification
                            ├─ 05.5 Dev Testing Strategy
                            ├─ 05.6 Architecture Redesign
                            └─ 05.7 State Machine Redesign
                                ├─ 05.71 Stream Outage Fix
                                └─ 05.72 Startup Confirmation
06 ─────────────────────── 06 GUI Architecture Planning
                            ├─ 06.1 Admin GUI Implementation
                            │   ├─ 06.12 Admin GUI Code Check
                            │   └─ 06.13 Arrival Confirmation
                            ├─ 06.5 Public Viewer Web GUI
                            │   └─ 06.51 Cloudflare Tunnel
07 ─────────────────────── 07 YOLO Training
08                          (not reached)
09                          (not reached)
10                          (not reached)
11                          (not reached)
```

Seven planned major hos executed. Twelve additional documents emerged. The arc consumed 19 sessions instead of 12 — not because of failure, but because the construction phase was richer than the plan assumed. The planned "Frontend Phase" (07–09) was partially consumed by the construction-phase GUI work (06.x), and the "Polish Phase" (10–11) never arrived as distinct sessions because polishing was happening continuously in the operational branches.

### 3.4 Reading the Arc Diagnostically

The shape of the actual arc tells you things the plan couldn't:

**Where branching concentrates tells you where complexity lives.** Kanyō's branches cluster at 05 and 06 — deployment and GUI. These were the domains where the original plan underestimated the work. A facilitator seeing this pattern in a new project would know to allocate more sessions to these areas next time.

**Where the stage shifts tell you how the learner developed.** Hos 01–04 were clean shu execution. Ho 05.5 was the first self-directed work (ha). Ho 05.7 was the first ri-stage document. The stage transition happened in the deployment phase, not the frontend phase — the learner developed architectural independence earlier than the plan expected, triggered by the complexity of getting a real system running.

**Where the plan goes quiet tells you what was deferred.** Hos 08–11 were never reached. That's not failure — it's scope management. The system was running, the core features worked, and the remaining planned work (frontend polish, user tagging) was deferred in favor of the emergent work that had more immediate value.

---

## 4. The Shu-Ha-Ri Wave

The [[shu-ha-ri|shu-ha-ri progression]] (framework/structure/shu-ha-ri.md) doesn't just apply to individual hos — it creates a wave across the arc.

### 4.1 The Primary Wave

Every arc has a primary shu → ha → ri progression that follows the learner's overall development:

```
Ho:   00  0.5  01  02  03  04  05  05.5  05.6  05.7  06  06.1  07
       │        │       │       │         │           │         │
Stage: S   S    S   S   S   S   S    H    H     R    H    R    S→H
```

The wave isn't smooth — it advances and retreats. Ho 07 (YOLO Training) drops back to shu because it's a new domain (ML training) even though the learner is otherwise at ha/ri for the rest of the system.

### 4.2 Domain-Specific Resets

Shu resets when the domain changes. Within a single arc, the learner may be at different stages for different domains simultaneously:

```
              Ho 01  Ho 02  Ho 03  Ho 04  Ho 05  Ho 06  Ho 07
              ─────  ─────  ─────  ─────  ─────  ─────  ─────
Python         S      S      H      H      H      H      H
Detection      —      S      S      H      H      H      H→Ri
Docker         —      —      —      S      H      Ri     Ri
GUI            —      —      —      —      —      S→H    —
ML Training    —      —      —      —      —      —      S
```

The learner is a beginner and an expert at the same time, depending on the domain. The stage assignments in the Ho Overview should reflect this per-domain reality, not assume a single linear progression.

### 4.3 The Facilitation Implication

The shu-ha-ri wave tells the facilitator (or self-directed learner) when to tighten and loosen structure:

- **Tighten** when the learner enters a new domain (Docker in Ho 04, ML in Ho 07) — even if they're ri-stage elsewhere
- **Loosen** when the learner starts questioning prescriptions, pushing back on AI suggestions, or self-scoping work — they're ready for ha
- **Let go** when the learner is producing Problem → Solution → Changes → Results documents without being asked — they've internalized ri

---

## 5. The Ho Overview as Arc Document

The Ho Overview (Ho 00) is the arc's governing document. It's produced during Kamae and updated as the arc unfolds.

### 5.1 What It Contains

At minimum:

```markdown
# Ho 00: [Project Name] — Ho Overview

## Project Summary
[Brief: what are we building and why]

## Ho Sequence

| Ho | Title | Phase | Stage | Dependencies | Status |
|---|---|---|---|---|---|
| 0.5 | Tool Mastery | Orientation | Shu | — | ✅ |
| 01 | Project Setup | Foundation | Shu | 0.5 | ✅ |
| 02 | Core Detection | Foundation | Shu | 01 | ✅ |
| ... | ... | ... | ... | ... | ... |

## Phase Map
[Which hos belong to which phase, and why]

## Scope Boundaries
[What's in this arc and what's deferred to future work]

## Dependencies
[External dependencies, tool requirements, access needed]
```

### 5.2 How It Evolves

The Ho Overview is a living document. As the arc progresses:

- **Emergent hos get added.** When Ho 05 branches into 05.5, 05.6, and 05.7, those appear in the overview with their actual stage and status.
- **Stage assignments get corrected.** A ho planned as shu might turn out to be ha once the learner reaches it. Update the table.
- **Deferred work gets noted.** When planned Hos 08–11 aren't reached, mark them as deferred with a note on why and whether they're still relevant.
- **The phase map gets annotated.** "Construction was 2x longer than planned because deployment revealed architectural issues" is valuable information for the next arc.

The final state of the Ho Overview is a complete record of the project's development arc — planned, actual, and annotated.

---

## 6. Arc Design Principles

### 6.1 Front-Load Foundation

The first 2–3 hos should establish a working development environment, project structure, and at least one end-to-end capability. Resist the temptation to plan extensive orientation work — learners want to build, and early success builds momentum. But don't skip the foundation to get to "the interesting stuff" — a weak foundation makes everything harder.

### 6.2 Plan for Branching in Construction

Construction is where arcs branch. Budget for it. If the plan has 4 construction-phase hos, expect 6–10 total sessions by the time branches are included. This isn't pessimism — it's the pattern. Construction discovers complexity that the plan couldn't see.

### 6.3 Let Operations Emerge

Don't over-plan the operations phase. If the system works and the practitioner is maintaining it, the work will self-organize. Ri-stage practitioners don't need the plan to tell them what to do next — they see the problems and address them. The plan should get the system to operational; after that, the practitioner takes over.

### 6.4 Scope the Arc, Not the Project

An arc is not the whole project. It's one pass — orientation through operations — for a defined scope. A large project might require multiple arcs: the first builds the core system, the second adds a major feature set, the third refactors for scale. Each arc follows the same phase progression but starts at a different level of practitioner competence.

### 6.5 Use Time, Not Ho Count, as the Planning Unit

"12 hos" is meaningless without knowing session frequency. A learner working two sessions per week completes a 12-ho arc in 6 weeks. A learner working one session per week takes 3 months. The same arc feels completely different at different cadences — weekly sessions maintain momentum; biweekly sessions require more context recovery at the start of each ho.

Plan arcs in _weeks_ or _months_, then estimate ho count. Not the reverse.

---

## 7. When Arcs Go Wrong

### 7.1 The Stalled Arc

The learner stops making progress. Sessions produce partial deliverables. The devlog confidence level drops. Causes:

- **Foundation wasn't solid.** The learner is building on concepts they don't understand. Solution: insert a shu-stage ho that addresses the gap, even if it feels like going backward.
- **The ho is too ambitious.** The scope of a single ho exceeds what's achievable in one session. Solution: split it into smaller hos with clearer deliverables.
- **The learner is burned out.** Too many sessions too close together. Solution: reduce cadence and pick a high-energy, high-reward ho next.

### 7.2 The Runaway Arc

The arc keeps expanding. Every ho branches. The system works but the project never "finishes." Causes:

- **Scope wasn't bounded.** The arc is trying to build the whole project instead of a defined milestone. Solution: draw a line — "the arc ends when X is working" — and defer everything else.
- **Perfectionism.** The practitioner keeps finding improvements instead of shipping. Solution: ri-stage work should have exit criteria, not just open-ended maintenance.
- **The plan was too ambitious.** 15 planned major hos is probably too many. 8–12 is the sweet spot for a single arc.

### 7.3 The Linear Arc

The arc follows the plan exactly. No branches, no surprises. This sounds ideal but is actually a warning sign:

- The learner may be following instructions without engaging deeply enough to discover emergent problems.
- The plan may have been too conservative — the learner was ready for ha-stage work but kept getting shu-stage prescriptions.
- The system may not be complex enough to produce genuine engineering challenges.

Some branching is healthy. It means the learner is encountering and responding to real complexity.

---

## 8. The Kanyō Arc in Full

For reference, the complete Kanyō project arc annotated with phases, stages, and arc characteristics:

|Ho|Title|Phase|Stage|Character|
|---|---|---|---|---|
|00|Overview|Kamae|—|The plan|
|0.5|Tool Mastery|Orientation|Shu|Inserted pre-foundation|
|01|Git Good|Foundation|Shu|Clean execution|
|02|Falcon Vision|Foundation|Shu|First real capability|
|03|Live Detection & Notification|Construction|Shu|Core pipeline|
|04|Docker Deploy|Construction|Shu|New domain (containers)|
|05|Deployment Verification|Construction|Shu→Ha|Branching point|
|05.5|Dev Testing Strategy|Construction|Ha|Self-directed process work|
|05.6|Architecture Redesign|Construction|Ha|Major decision: 800 lines removed|
|05.7|State Machine Redesign|Construction|Ri|Practitioner-level simplification|
|05.71|Stream Outage Fix|Operations|Ri|Targeted fix from operating system|
|05.72|Startup Confirmation|Operations|Ri|Targeted fix from operating system|
|06|GUI Architecture Planning|Construction|Ha|Technology evaluation|
|06.1|Admin GUI Implementation|Construction|Shu→Ha|New domain (web UI)|
|06.12|Admin GUI Code Check|Operations|Ri|Maintenance: quality sweep|
|06.13|Arrival Confirmation|Operations|Ri|Feature from operational need|
|06.5|Public Viewer Web GUI|Construction|Ha|Design decisions|
|06.51|Cloudflare Tunnel|Operations|Ri|Infrastructure: deployment runbook|
|07|YOLO Training|Construction|Shu→Ha|New domain (ML training)|

19 sessions. 7 planned major hos executed. 12 emergent documents. The arc ran for approximately 6 weeks with 3–4 sessions per week. Construction and operations overlapped significantly after Ho 05 — the system was running and being built simultaneously.

---

## 9. Related Framework Documents

- [[kamae-project-framing|Kamae: Project Framing]] (framework/structure/kamae-project-framing.md) — How arcs begin
- [[ho-structure|Ho Structure]] (framework/structure/ho-structure.md) — The unit that arcs are built from, including the numbering system
- [[shu-ha-ri|Shu-Ha-Ri Progression]] (framework/structure/shu-ha-ri.md) — How stages flow within an arc
- [[tiered-understanding|Tiered Understanding]] (framework/structure/tiered-understanding.md) — How understanding develops across an arc
- [[template-selection-guide|Template Selection Guide]] (framework/templates/template-selection-guide.md) — Choosing the right template as the arc progresses

---

_This document is part of the Ho System framework. It describes the structural specification for project arcs. For guidance on designing effective arc sequences — including pacing, difficulty calibration, and learner-appropriate scoping decisions — see the facilitation layer (proprietary)._