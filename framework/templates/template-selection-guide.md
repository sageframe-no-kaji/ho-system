# Template Selection Guide

## Choosing the Right Structure for the Work

This guide helps practitioners and ho authors select the appropriate template for a given piece of work. Choosing wrong isn't catastrophic — but it creates friction. Too much structure slows a practitioner down. Too little structure leaves a learner without support.

---

## The Decision in 30 Seconds

Ask one question: **Who is doing the thinking?**

| If...                                                                      | Use...                                                            |
| -------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| The **author** did the thinking. The learner follows.                      | Shu Ho Template (framework/templates/shu-ho-template.md)          |
| The **learner** does the thinking. The template provides the framework.    | Ha Ho Template (framework/templates/ha-ho-template.md)            |
| The **practitioner** already thought. Now they're recording what they did. | Ri Ho Template (framework/templates/ri-ho-template.md)            |
| The **practitioner** is handing specific work to an AI agent.              | Agent Task Specification (framework/templates/agent-task-spec.md) |

If you're unsure between two, pick the one with MORE structure. You can always skip sections. You can't add scaffolding that isn't there.

---

## The Decision in Detail

### Use the Shu Template When:

**The learner is new to what they're about to do.**

This doesn't mean new to everything — it means new to the specific domain, tool, or technique in this ho. A practitioner with six months of Python experience still gets a shu-stage ho for their first Docker deployment. Shu resets when the context changes.

Concrete signals:

- The learner can't yet explain the _why_ behind the steps they're taking
- They need verification checkpoints to know if they're on track
- The ho author can predict the steps because there's a known-good path
- Getting the approach wrong would waste significant time
- The learner would benefit from seeing the "right way" before developing their own style

**What shu provides:** Prescriptive parts (4–9), strict ~2hr duration, verification at each step, understanding tier declarations, AI collaboration guidance, full devlog template with confidence scale.

**The risk of using shu when you shouldn't:** The learner feels patronized. The structure slows them down. They skip sections because they already know the material. If you're seeing these signals, the learner has outgrown shu for this domain.

---

### Use the Ha Template When:

**The learner needs to make a real decision between competing approaches.**

Ha is for work where the interesting part is the _thinking_, not the _implementation_. The learner evaluates tradeoffs, chooses an approach, documents why, then builds. If there's no genuine decision to make, ha's Phase 1 (Think) becomes busywork.

Concrete signals:

- Multiple viable approaches exist and the learner must choose
- The learner can articulate the problem but not yet the solution
- The work involves designing or redesigning system architecture
- The learner would benefit from documenting their reasoning before building
- Getting the approach wrong is costly but recoverable (ha decisions are revisable)

**What ha provides:** Three-phase structure (Think → Execute → Reflect), `[LEARNER COMPLETES]` sections, decision documentation before implementation, agent task log, confidence scale calibrated for judgment, 2–4hr guideline.

**The risk of using ha when you shouldn't:** Two failure modes. If the learner isn't ready (should be shu), Phase 1 stalls because they can't evaluate approaches they don't understand yet. If the learner is past this (should be ri), the three-phase structure adds ceremony to work that doesn't need it.

---

### Use the Ri Template When:

**The practitioner knows what to do and needs a lightweight record.**

Ri is for competent practitioners doing real work — maintenance, debugging, feature implementation, system improvements. The thinking and the building happen fluidly, and the documentation serves future maintainers, not the learning process.

Concrete signals:

- The practitioner can scope the work without external guidance
- They could explain the approach to a colleague before starting
- The work is execution, not exploration
- The system is operational and the work is maintenance or extension
- A full decision record (ha) would be disproportionate to the work

**What ri provides:** Five sections (Problem, Solution, What Changed, Results, Notes). No phases, no verification, no devlog — the ho document IS the record.

**The risk of using ri when you shouldn't:** If the practitioner is actually facing a genuine architectural decision, ri's lightweight format lets them skip the thinking that ha would have structured. They build something, it works, but they can't explain why they chose this approach over alternatives — because they never explicitly considered alternatives. The work succeeds but the judgment doesn't develop.

---

### Use an Agent Task When:

**You're delegating a bounded piece of implementation to an AI agent.**

Agent tasks are specifications, not session structures. They cross the boundary between human judgment and machine execution. The human decides what and why; the agent executes how.

Concrete signals:

- You can describe the desired outcome precisely
- The work doesn't require the agent to make architectural decisions
- You'll review the output, not learn from the process
- The task is a single bounded unit (one feature, one fix, one refactor)

**What agent tasks provide:** Two formats (broad spec for low-risk features, surgical spec for critical-path fixes), naming convention, tracking system.

**The risk of using an agent task when you shouldn't:** If the specification is vague, the agent guesses, and the guess may be wrong in ways that are hard to detect. If the task actually requires architectural thinking, the agent's implementation may work but be wrong at a design level. In both cases, the human should do more thinking before delegating — which means using a ha template for the decision, then delegating the execution via agent tasks.

---

## Mixed Sessions

Real work often doesn't fit cleanly into one template. Common combinations:

**Ha ho containing agent tasks.** The practitioner thinks through an architectural decision (Phase 1), then delegates implementation pieces to an AI agent (Phase 2), then reflects on whether the decision held up (Phase 3). The ha template wraps the session; agent tasks document the delegated pieces.

**Ri ho referencing agent tasks.** A ri session might consist of writing and reviewing three agent tasks. The ri ho documents the overall session (what problems were addressed, what changed in total); the agent tasks document the individual delegations.

**Shu ho with minimal agent use.** In shu, the learner does most work manually to build understanding. Agent tasks are rare and limited to boilerplate ("have Claude Code scaffold the test directory structure").

**Standalone agent task, no ho.** Quick fixes and small features don't always need a session wrapper. If the task is bounded and the documentation is self-contained, the agent task spec is sufficient on its own.

---

## Stage Progression Over a Project Arc

Templates aren't chosen once — they shift as the learner develops. A typical project arc:

```
Project Start                                            Project Maturity
    │                                                         │
    ▼                                                         ▼

    Shu    Shu    Shu    Ha     Ha     Ri    Ri    Ri    Ri
    Ho 1   Ho 2   Ho 3   Ho 4   Ho 5   Ho 6  Ho 7  Ho 8  Ho 9
                                              ├─ AT 001
                                              ├─ AT 002
                                                    ├─ AT 003
                                                    ├─ AT 004
                                                          ├─ AT 005
                                                          ├─ AT 006
                                                          ├─ AT 007
```

Early hos are shu (building foundational skills). Middle hos shift to ha (making design decisions). Late hos are ri (operating and extending the system), with agent tasks becoming the primary work artifact.

But this isn't linear. **Shu resets when the domain changes.** A practitioner in ri for Python detection work drops back to shu for their first Docker deployment, then climbs back through ha as they gain container fluency. The [[shu-ha-ri|Shu-Ha-Ri Progression Model]] (framework/structure/shu-ha-ri.md) covers this in detail.

---

## Quick Reference

|                  | Shu                                | Ha                                                | Ri                                                                   | Agent Task                        |
| ---------------- | ---------------------------------- | ------------------------------------------------- | -------------------------------------------------------------------- | --------------------------------- |
| **One-liner**    | Follow the path                    | Make the decision                                 | Record the work                                                      | Specify the delegation            |
| **Duration**     | ~2 hours (strict)                  | 2–4 hours (guideline)                             | As long as needed                                                    | Minutes to hours                  |
| **Sections**     | Parts, verification, tiers, devlog | Phases (think/execute/reflect)                    | Problem, solution, changes, results                                  | Goal, spec, constraints, checks   |
| **Who thinks**   | The author                         | The learner                                       | Already done                                                         | The practitioner (before writing) |
| **Who builds**   | The learner (guided)               | The learner (self-directed)                       | The practitioner                                                     | The AI agent                      |
| **AI role**      | Verify and explain                 | Think together, then execute                      | Implementation accelerator                                           | Executor                          |
| **Key artifact** | Working code + understanding       | Decision record + working code                    | Change record                                                        | Reviewed implementation           |
| **Devlog**       | Learning journal                   | Decision record                                   | The ho IS the record                                                 | Not applicable                    |
| **When wrong**   | Feels patronizing                  | Phase 1 stalls or feels like busywork             | Skips important thinking                                             | Agent guesses wrong               |
| **Verification** | Lint + tests + template checks     | Lint + tests + self-review + emerging cross-agent | Full stack (lint + tests → self-review → cross-agent → human review) | Lint + tests + self-review        |

---

## For Ho Authors

When designing a ho sequence for a new project (during [[kamae-project-framing|Kamae: Project Framing]] (framework/structure/kamae-project-framing.md)), assign each planned ho a stage in the Ho Overview:

```markdown
| Ho  | Title              | Stage  | Rationale                                     |
| --- | ------------------ | ------ | --------------------------------------------- |
| 0.5 | Tool Mastery       | Shu    | New tools, new environment                    |
| 1   | Project Setup      | Shu    | Foundation, known-good path                   |
| 2   | Core Detection     | Shu    | New domain (ML), needs guidance               |
| 3   | Live Pipeline      | Shu→Ha | Starts prescriptive, ends with design choices |
| 4   | Docker Deploy      | Shu    | New domain (containers)                       |
| 5   | System Integration | Ha     | Architectural decisions, multiple approaches  |
| 6   | GUI Architecture   | Ha     | Technology evaluation, design work            |
| 7+  | Operations         | Ri     | System is running, maintenance mode           |
```

These assignments are provisional. A ho planned as ha may turn out to be shu once you realize the learner needs more foundation. A ho planned as shu may be ri if the learner already has the skills. Adjust as you go — the stage map is a guide, not a contract.

---

_This document is part of the Ho System framework._ _Templates: [[shu-ho-template|Shu]] (shu-ho-template.md) · [[ha-ho-template|Ha]] (ha-ho-template.md) · [[ri-ho-template|Ri]] (ri-ho-template.md) · [[agent-task-spec|Agent Task]] (agent-task-spec.md)_ _Structure: [[shu-ha-ri|Shu-Ha-Ri Progression]] (framework/structure/shu-ha-ri.md) · [[kamae-project-framing|Kamae: Project Framing]] (framework/structure/kamae-project-framing.md)_
