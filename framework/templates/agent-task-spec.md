# Agent Task Specification

## Template for Delegated Implementation Tasks

**Use when:** You know what needs to be built or fixed and you're handing the work to an AI agent for execution. **See:** [[ri-ho-template|Ri Ho Template]] (framework/templates/ri-ho-template.md) for the session wrapper that may contain multiple agent tasks.

---

## What an Agent Task Is

An agent task is a specification written by a human for an AI agent to execute. It's the artifact that crosses the boundary between human judgment and machine implementation. The human decides _what_ and _why_; the agent executes _how_.

Agent tasks are distinct from ho documents:

||Ho Document|Agent Task|
|---|---|---|
|**Written by**|Author (shu), learner (ha/ri)|The practitioner|
|**Written for**|The human doing the work|The AI executing the work|
|**Purpose**|Structure a session|Specify a delegation|
|**When written**|Before the session begins|During a session, when work is ready to hand off|
|**Reviewed by**|The learner (self-check)|The practitioner (code review)|

Agent tasks exist at all stages of the shu-ha-ri progression, but they become the primary work artifact in ri stage. In shu, the ho tells you exactly what to build. In ha, you decide what to build and may delegate pieces. In ri, agent tasks ARE the work — the practitioner's job is writing good specs and reviewing the output.

---

## The Spectrum

Agent tasks range from broad feature specs to surgical engineering instructions. Both are valid. The precision should match the stakes:

**Broad (feature spec):** "Make the Files link work — show a file browser for the stream's clips and logs directories. Here are the features, here's the route, here's the general approach."

**Surgical (engineering spec):** "Add `arrival_anchor_frame: Optional[int] = None` to FalconState. Set it at first SUSPECTED arrival. Use it for clip offset math. ~15 lines across 3–4 files. Do NOT change the state machine timing logic."

The difference isn't quality — it's risk. A new page that's easy to rework gets a broad spec. A timing fix in a state machine where the wrong change produces silent data corruption gets a surgical spec with invariants and acceptance checks.

**Calibration principle:** The spec's precision should be proportional to the cost of getting it wrong.

---

## Template

> The sections below are the agent task structure. Not all sections are needed for every task — use judgment. The only required sections are Goal and the task content itself.

---

### File Naming Convention

```
[NNN] - Agent Task [STATUS] - [Short Title].md
```

- **NNN:** Sequential number, zero-padded to 3 digits (001, 002, ... 013)
- **STATUS:** Blank when created, `DONE` when complete
- **Short Title:** What the task does, in a few words

Examples:

- `002 - Agent Task DONE - Implement Files Browser Page.md`
- `013 - Agent Task - Fix Arrival Clip Anchoring.md`
- `027 - Agent Task DONE - Add Timezone to Log Formatter.md`

Tasks live in the project's `tasks/` directory (or equivalent).

---

### Broad Spec (Feature Implementation)

Use for: new features, new pages, new components, straightforward additions where the agent has latitude in implementation details.

```markdown
# Agent Task: [Title]

## Goal

[One or two sentences. What should exist or work when this is done.]

## Context

[Optional. What the agent needs to know about the system to do this well.
Existing routes, data structures, UI patterns to match.]

## Features

[What the implementation should include. Numbered list.]

1. [Feature/behavior]
2. [Feature/behavior]
3. [Feature/behavior]

## Implementation Guidance

[Optional. Suggested approach — routes, file locations, patterns to follow.
The agent can deviate if it has a better idea, but this is the starting point.]

## Commit

[Commit message template or guidance.]
```

> 📐 **AUTHOR — Broad Spec Notes**
> 
> The broad spec trusts the agent's judgment on implementation details. This is appropriate when:
> 
> - The feature is additive (not modifying critical existing behavior)
> - The output is easy to review and rework
> - The codebase has established patterns the agent can follow
> - Getting it 80% right is fine — you'll iterate
> 
> Task 002 (Files Browser) from the Kanyō pilot is a good example. It includes suggested code, but the agent could have implemented it differently and the result would still be acceptable. The review surface is a new page — isolated, testable, low-risk.

---

### Surgical Spec (Precision Fix)

Use for: bug fixes in critical paths, changes to state machines or timing logic, modifications where the wrong implementation produces silent failures, anything where you need exact control over what changes.

```markdown
# TASK: [TITLE IN CAPS — signals this is a directive, not a discussion]

## GOAL

[What must be true after this task. Precise. Include what must NOT change.]

## DO NOT CHANGE

[Explicit list of things the agent must leave alone. This is as important
as what they should change.]

- [Invariant 1]
- [Invariant 2]

## REQUIRED CHANGES (EXACT)

### 1. [filename.py]

[What to add/modify. Include exact attribute names, exact insertion points,
exact logic. Pseudocode or actual code.]

Constraints:
- [Constraint on this change]
- [What NOT to do here]

### 2. [filename.py]

[Same level of precision.]

### 3. [filename.py]

[Same level of precision.]

## INVARIANTS TO PRESERVE

[Things that must remain true about the system after the change.
These are the acceptance criteria for the system's behavior, not
just the code diff.]

1. [Invariant]
2. [Invariant]

## ACCEPTANCE CHECKS (MANDATORY)

[How to verify the change is correct. Not "run the tests" — specific
scenarios that must produce specific results.]

- [Scenario] produces [expected result], not [wrong result]
- [Edge case] handles correctly
- [Negative case] produces no change

## LINE COUNT EXPECTATION

[Optional but powerful. Forces the agent to make minimal changes.
"~15 lines across 3-4 files" prevents the agent from rewriting
the module.]

## QUALITY

[Testing and linting requirements.]
```

> 📐 **AUTHOR — Surgical Spec Notes**
> 
> The surgical spec constrains the agent precisely because the cost of getting it wrong is high. This is appropriate when:
> 
> - The change touches critical path logic (state machines, timing, data integrity)
> - A wrong implementation could produce silent failures (data loss, incorrect clips, false notifications)
> - The agent needs to modify existing behavior, not add new behavior
> - You know EXACTLY what the fix should look like
> 
> Task 013 (Arrival Clip Anchoring) from the Kanyō pilot is the canonical example. Every change is specified to the attribute name. Invariants are listed twice — what to preserve AND what must hold after. The line count expectation (~15 lines) prevents scope creep. The acceptance checks describe specific frame-math scenarios.
> 
> Writing a surgical spec takes more time than writing the code yourself would — for a 15-line change. The value is in VERIFIABILITY. You can review the agent's output against a precise spec. You can't review it against a vague one.
> 
> **The ALL CAPS convention** is deliberate. It signals to the agent (and to the human reviewing later) that this is a directive, not a conversation. The agent should execute, not discuss alternatives.

---

## Writing Good Agent Tasks

### The Core Skill

Writing agent tasks is specification writing. It's the same skill as writing a good ticket, a good bug report, or a good technical requirement. The practitioner who can write a clear agent task can write a clear spec for a human junior engineer too.

What makes a good agent task:

1. **Goal is a state change, not an activity.** "Implement file browser" is an activity. "The Files link works — users can navigate directories and view/play media" is a state change. The agent knows what "done" looks like.
    
2. **Scope is bounded.** The agent should be able to complete the task without making architectural decisions. If the task requires choosing between approaches, the human should make that choice in the spec — or use a ha-stage ho instead of delegating.
    
3. **Constraints are explicit.** What NOT to change is as important as what to change. Especially for surgical specs, the "do not change" list prevents the agent from "improving" things that shouldn't be touched.
    
4. **Review surface is clear.** After the agent completes the task, the reviewer should know exactly what to check. For a broad spec: does the feature work? For a surgical spec: does each invariant hold?
    
5. **Precision matches risk.** A new UI page gets a broad spec. A state machine fix gets a surgical spec. Don't over-specify low-risk work (wastes time) or under-specify high-risk work (invites bugs).
    

### Common Failure Modes

**Too vague:** "Fix the clip timing." Fix what about it? What's wrong? What should the correct behavior be? The agent will guess, and the guess may be wrong in a way that's hard to detect.

**Too broad:** "Refactor the detection system to be cleaner." This is a ha-stage ho, not an agent task. The agent doesn't know what "cleaner" means in your architectural context.

**Missing constraints:** "Add a PENDING_STARTUP state." Without specifying what it should and shouldn't affect, the agent may wire it into systems you didn't intend.

**No acceptance criteria:** "Make arrival clips correct." Correct according to what definition? The practitioner knows — but the agent doesn't unless you tell it.

---

## Tracking

Agent tasks should be trackable. Minimum viable tracking:

```
tasks/
├── 001 - Agent Task DONE - Initial Project Setup.md
├── 002 - Agent Task DONE - Implement Files Browser Page.md
├── 003 - Agent Task DONE - Add Stream Status Endpoint.md
...
├── 013 - Agent Task - Fix Arrival Clip Anchoring.md
├── 014 - Agent Task - Add Departure Clip Symmetry.md
```

The filesystem IS the tracker. Sequential numbering gives order. Filenames give status and description at a glance. `DONE` in the filename means it's complete. No `DONE` means it's open or in progress.

For projects that outgrow filesystem tracking, a simple table in the Ho Overview or a project-level `TASKS.md` works:

```markdown
| # | Task | Status | Ho | Commit |
|---|---|---|---|---|
| 001 | Initial Project Setup | ✅ Done | — | a1b2c3d |
| 002 | Implement Files Browser | ✅ Done | 06.1 | d4e5f6g |
| 013 | Fix Arrival Clip Anchoring | ✅ Done | 06.13 | h7i8j9k |
| 014 | Add Departure Clip Symmetry | 🔲 Open | — | — |
```

The "Ho" column links tasks to the ho session they were part of (if any). Many ri-stage tasks exist outside of any ho — they're standalone work items.

---

## Relationship to Ho Documents

Agent tasks and ho documents serve different purposes and can coexist:

**Shu stage:** The ho IS the instruction. Agent tasks are rare — the learner is building manually to develop understanding. An author might include an agent task for boilerplate setup ("have Claude Code scaffold the project structure") but the learning work is manual.

**Ha stage:** The practitioner may write agent tasks during Phase 2 (Execute) for well-defined implementation pieces, while keeping architectural decisions in Phase 1 (Think). The agent task log in the ha template tracks these delegations.

**Ri stage:** Agent tasks are the primary work artifact. A ri ho session might consist entirely of writing and reviewing agent tasks. The ri ho document is the wrapper; the agent tasks are the content.

**Standalone:** Many agent tasks exist outside any ho. Quick fixes, small features, maintenance work — not everything needs a session wrapper. If the work is a single bounded task, the agent task spec is sufficient documentation on its own.

---

## The Kanyō Evidence

The Kanyō pilot produced agent tasks across a wide range of precision:

**Task 002 (Files Browser):** Broad spec. New feature, additive, low risk. Includes suggested route code, HTML template, commit message. Conversational tone. The agent has latitude. ~195 lines of spec for a feature implementation.

**Task 013 (Arrival Clip Anchoring):** Surgical spec. Critical path fix, timing-sensitive, silent failure risk. ALL CAPS directives. Exact attribute names, exact insertion points, invariants listed twice, acceptance checks with specific frame-math scenarios, line count expectation (~15 lines). ~130 lines of spec for a 15-line change.

The ratio is revealing: 130 lines of spec for 15 lines of code. That's not waste — that's the cost of precision. The spec IS the engineering. The code is just the output.

---

_This document is part of the Ho System framework._ 
_For session wrappers, see: [[shu-ho-template|Shu Ho Template]] (shu-ho-template.md) · [[ha-ho-template|Ha Ho Template]] (ha-ho-template.md) · [[ri-ho-template|Ri Ho Template]] (ri-ho-template.md)_
*For template selection guidance, see the [[template-selection-guide|Template Selection Guide]] (template-selection-guide.md).*