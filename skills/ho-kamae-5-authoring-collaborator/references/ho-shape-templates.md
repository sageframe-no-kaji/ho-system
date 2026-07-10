# Ho Shape Templates

Per-ho documents come in four shapes. The shape determines the document's structure and how it frames work for the agent. This file describes each shape, when to use it, and the structural template.

---

## The Four Shapes

| Shape | Used for | Output character |
|---|---|---|
| **Orientation** | ho-00, learning hos, replan checkpoints | Framing without execution; concept primer + handoff |
| **Ha** | Most building hos when architectural thinking precedes specifying | Decision record + execution spec + post-execution reflection |
| **Ri** | Maintenance hos, surgical fixes, well-defined work | Problem → solution → changes → results |
| **Shu** | Rare; for handing the framework to a learner | Author-prescribed parts walked through |

The first three are common in architectural-authority practice (practitioner directs, agent implements). Shu is rare in that mode — it's the original framework's shape for a learner doing the implementation themselves with prescribed steps.

---

## Orientation Shape

**When:** ho-00 (project-start orientation), learning hos that produce no shipping feature, replan checkpoints (decision-only sessions).

**What it does:** Frames where the practitioner stands at a particular moment in the project and what's known going forward. No code, no shipping feature.

**Structure:**

```markdown
---
created: YYYY-MM-DD
status: draft
type: ho-document
project: <project>
ho: <NN>
kamae: 5
shape: orientation
builds-on:
  - <upstream Kamae documents>
---

# ho-NN — <Title>

[Brief intro — what this session is for, what it's not, where it sits in the build]

---

## 1. Pre-conditions

[Verified state at session start. For ho-00: scaffold in place, upstream Kamae chain committed. For replan checkpoints: prior ho complete, work to evaluate exists.]

---

## 2. <Substantive content section — varies by orientation type>

For ho-00: **New concepts** — concept primers for the project's new tech. Generated via the learning interview (see `references/learning-interview.md`). Each concept gets paragraph-level depth + resources.

For learning hos: **Learning content** — what's being learned, why, how the practitioner will know they've learned it.

For replan checkpoints: **Evaluation** — what was built vs what was planned, what's the gap, what's the call (continue / pause / replan).

---

## 3. Project ho shape (ho-00 only) / Decisions and outcomes (replan checkpoints) / Learning checkpoints (learning hos)

[Section content varies by orientation type. For ho-00, this is where the project-level shape commitments land — what shape will most hos take, what naming conventions, what's deferred to the first building ho.]

---

## 4. Handoff

[What the next ho needs at session start. What's deferred to it. Practical actions to take between this session and the next.]

### Closing this ho

Even an orientation ho ends a session — so it closes the same way: flip `status: complete` and
write the state-summary block to the project's K6
(`ho-process/kamae-6-<project>-state-memory.md`, §3 of cross-session-continuity). If this ho is
tracked in K4's build record, append its entry too (§2.4 of kamae-project-framing). The block,
verbatim labels and order:

**STATE-SUMMARY**
- **COMPLETED** — <what this session finished>
- **NEXT** — <the single pointer to what comes next>
- **ACTION ITEMS / BLOCKS** — <open items; blocks loudly, or `none`>
- **PROJECT LIFECYCLE** — <kamae | dev | beta | production>

---

_Authored: YYYY-MM-DD._
_Execution: [N/A for orientation, or note]_
```

**Key points:**

- Orientation hos don't have Think/Execute/Reflect phases. The whole document IS the framing.
- The substantive section (section 2) is where the orientation-specific work lives. For ho-00 it's concept primers; for replan checkpoints it's evaluation.
- Section 3 in ho-00 is where project-level decisions land (e.g., "most hos will be ha-shaped," "agent tasks live in `ho-process/agent-tasks/`").
- Section 4 (handoff) is brief — what the next ho needs, what's deferred.

---

## Ha Shape

**When:** Most building hos. The practitioner has enough familiarity to specify the work but architectural decisions need to be made before specification can land. The Think phase produces the decisions; the Execute phase produces the work; the Reflect phase reviews what real implementation revealed.

**What it does:** Three-phase structure: Think → Execute → Reflect. Each phase has its own work, its own output.

**Structure:**

```markdown
---
created: YYYY-MM-DD
status: draft
type: ho-document
project: <project>
ho: <NN>
kamae: 5
shape: ha
builds-on:
  - <upstream Kamae documents>
agent-tasks:
  - <Ho-NN-AT-MM.md, if any>
---

# ho-NN — <Title>

[Brief intro — what this ho does, what's out of scope, what deferred decisions resolve here]

**Out of scope:** [explicit boundaries]

**Resolves deferred decisions** (from the ho-overview):
- <decision name>
- <decision name>

---

## Phase 1 — Think

[N architectural decisions made before execution. Plus discoveries deferred to execution time.]

### Decision 1 — <name>: <choice>

[Brief rationale. Concrete enough that the agent reads this and knows what was decided and why.]

### Decision 2 — <name>: <choice>

[...]

### Discovery (deferred to execution) — <topic>

[Some decisions can't be made without real data. Name them; describe the inspection that resolves them at execution time.]

---

## Phase 2 — Execute

[How the work gets done. Pointers to agent tasks if the work decomposes.]

### Ho-NN-AT-01 — <task name>

[Brief scope. → `ho-process/agent-tasks/Ho-NN-AT-01.md`]

### Ho-NN-AT-02 — <task name>

[...]

### Testing and iteration approach

[Per-task verification + real-data iteration loop. How feedback flows. What fixes propagate to which artifacts.]

### Done means

- [criteria]
- [criteria]

---

## Phase 3 — Reflect

*To be filled in after execution. Prompts:*

- **Did the design hold?** Where did real data surface things the Think phase didn't anticipate?
- **Decision review.** Were the Phase 1 decisions right?
- **Volume / scale / specifics.** What did the actual numbers turn out to be?
- **What broke that the tests didn't catch?**
- **Followups for the next ho or beyond.**

### Closing this ho

Closing = fill this Reflect + flip `status: complete` + write the state-summary block to the
project's K6 (`ho-process/kamae-6-<project>-state-memory.md`, §3 of cross-session-continuity) +
append a build-record entry to K4 (`ho-process/kamae-4-<project>-ho-overview.md`, §2.4 of
kamae-project-framing). The block, verbatim labels and order:

**STATE-SUMMARY**
- **COMPLETED** — <what this ho finished>
- **NEXT** — <the single pointer to what comes next>
- **ACTION ITEMS / BLOCKS** — <open items; blocks loudly, or `none`>
- **PROJECT LIFECYCLE** — <kamae | dev | beta | production>

---

_Authored: YYYY-MM-DD (Think phase)._
_Execution and Reflect: pending._
```

**Key points:**

- The Think phase is the architectural work. Decisions get logged with brief rationale, not full debate transcripts.
- Discoveries deferred to execution-time get explicit naming. "Real export shape" or "actual benchmark numbers" — name them and describe the inspection that resolves them.
- The Execute phase points at agent tasks if they exist. If the ho doesn't decompose into bounded tasks (simple enough to be one conversation), the Execute phase describes the work directly.
- The Reflect phase has prompts, not content, until execution finishes. Post-execution, it gets filled in with specific findings.
- "Done means" is the explicit acceptance criteria for the ho as a whole — not redundant with individual tasks' acceptance criteria.

---

## Ri Shape

**When:** Maintenance hos, surgical fixes, work where the practitioner already knows what to specify. No architectural thinking phase; the spec is clear from the start.

**What it does:** Records the problem, the solution, what changed, and what the result was. Often retrospective (written after the work) but can be authored before when the spec is bounded.

**Structure:**

```markdown
---
created: YYYY-MM-DD
status: draft
type: ho-document
project: <project>
ho: <NN>
kamae: 5
shape: ri
builds-on:
  - <upstream documents if any>
agent-tasks:
  - <Ho-NN-AT-MM.md, if any>
---

# ho-NN — <Title>

[One-line statement of what this ho does]

---

## Problem

[What was wrong, what was missing, what needed to change. Specific.]

---

## Solution

[What was done. The approach.]

---

## Changes

[File manifest, key edits. Reference agent tasks if they exist.]

- `<file path>` — <what changed>
- `<file path>` — <what changed>

If agent tasks: → `ho-process/agent-tasks/Ho-NN-AT-MM.md`

---

## Results

[What's true now that wasn't before. Verification that the change held. Any side effects.]

### Closing this ho

Closing = fill Results + flip `status: complete` + write the state-summary block to the
project's K6 (`ho-process/kamae-6-<project>-state-memory.md`, §3 of cross-session-continuity) +
append a build-record entry to K4 (`ho-process/kamae-4-<project>-ho-overview.md`, §2.4 of
kamae-project-framing). The block, verbatim labels and order:

**STATE-SUMMARY**
- **COMPLETED** — <what this ho finished>
- **NEXT** — <the single pointer to what comes next>
- **ACTION ITEMS / BLOCKS** — <open items; blocks loudly, or `none`>
- **PROJECT LIFECYCLE** — <kamae | dev | beta | production>

---

_Authored: YYYY-MM-DD._
_Commit: <hash or range>_
```

**Key points:**

- Ri hos are short. Lighter than ha, much lighter than orientation.
- Agent tasks under ri are common — the practitioner specifies bounded changes for the agent to execute. If the work is single-file and small, the ri document IS the spec.
- Results are concrete: "X works now," "the bug no longer reproduces," "the new field is populated on real data."

---

## Shu Shape

**When:** Rare in architectural-authority practice. Used when a learner is being walked through a domain — the practitioner is the author, the learner is the executor, the agent isn't doing the work.

**What it does:** Prescribed parts the learner walks through sequentially with verification at each step.

**Structure:** Use the framework's existing `framework/templates/shu-ho-template.md`. This shape exists in the framework's original learner-focused design and isn't reinvented here.

**Note:** If you find yourself authoring a shu-shaped ho for an architectural-authority practitioner who's working with an agent — stop. Either the practitioner needs an orientation-shaped learning ho (concept primer, then onward to ha when ready), or the work is actually ha and the practitioner just needs the Think phase scaffolded. Shu is a learner shape, not a practitioner shape.

---

## Choosing a Shape

Walk these questions in order. The first "yes" wins:

1. **Is this ho-00, a learning-only ho, or a replan checkpoint?** → Orientation.
2. **Does the work require architectural decisions before the spec can be written?** → Ha.
3. **Is the work well-defined, with the spec already clear?** → Ri.
4. **Is the practitioner being walked through a domain by a human author (not a peer agent)?** → Shu.

### Edge cases

- **A ha-shaped ho with a thin Think phase.** Acceptable. Sometimes the architectural decisions are fast (one or two clear choices) and the Execute phase carries the weight. Don't pad Think to make it look bigger.
- **A ri-shaped ho that develops a real Think phase mid-session.** Switch shape. Don't apologize for the rework — the discovery happens, and the document should reflect what actually happened.
- **No shape fits cleanly.** Surface to the practitioner. Force-fitting produces awkward documents. The orientation shape itself emerged from a moment when the existing four shapes didn't fit ho-00's actual function.

### Multiple shapes in one ho?

Don't. If the work is mixed (some architectural thinking, some surgical fixes), it's probably two hos that should be split. Use the numbering scheme — ho-N.1 (the ha-shaped part), ho-N.2 (the ri-shaped part) — rather than mashing shapes together.

Exception: hos that are clearly one shape but have a small additional move that doesn't warrant its own document (e.g., a ha-shaped ho ends with a small ri-flavored cleanup). Fold the small move into the larger shape's Execute phase, don't switch document structure.

---

## Frontmatter Conventions

All shapes share the frontmatter shape. Required fields:

- `created` — ISO date
- `status` — `draft` (most of life) → `complete` (after Reflect filled in for ha, after Results for ri, after authoring for orientation)
- `type` — always `ho-document`
- `project` — slug
- `ho` — number, zero-padded for ho-00 (`00`, not `0`)
- `kamae` — always `5` (per-ho documents are Kamae 5)
- `shape` — `orientation`, `ha`, `ri`, or `shu`
- `builds-on` — list of upstream documents this ho depends on

Optional fields:

- `agent-tasks` — list of agent task filenames, if the ho has them
- `commit` — hash or range, after execution
- `replaces` — if this ho was authored to replace a previous one
- `splits-from` — parent ho if this is a split child (e.g., ho-7.1 splits from ho-7)
