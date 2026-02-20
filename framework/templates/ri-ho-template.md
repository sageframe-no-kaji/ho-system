# Ri-Stage Ho Template

## Template for Practitioner-Level Work Sessions

**Stage:** Ri (離) — Transcend the Form **Use when:** The practitioner owns the system, scopes their own work, and needs a lightweight record — not scaffolding. **See:** [[shu-ha-ri|Shu-Ha-Ri Progression Model]] (framework/structure/shu-ha-ri.md) for when to use this template vs. others.

---

## How to Use This Template

This template is different from shu and ha. It's short on purpose.

In shu, the template IS the experience — it structures every minute. In ha, the template provides a thinking framework — three phases the learner moves through. In ri, the template is a **recording format**. The practitioner already knows how to think, plan, and execute. What they need is a consistent shape for documenting what they did and why, so their future self (and anyone else maintaining the system) can follow the trail.

If this template feels too light, you might be in ha territory. If it feels like overhead, you might want an agent task instead — see the [[agent-task-spec|Agent Task Specification]] (framework/templates/agent-task-spec.md).

### When to Use Ri vs. Agent Task

Both are ri-stage tools. The difference:

|              | Ri Ho                                            | Agent Task                             |
| ------------ | ------------------------------------------------ | -------------------------------------- |
| **Scope**    | A session of work (may include multiple changes) | A single well-defined task             |
| **Author**   | The practitioner, after the work                 | The practitioner, before handing to AI |
| **Audience** | Future maintainers (including yourself)          | The AI agent executing the task        |
| **Duration** | 30 minutes to a full day                         | Minutes to an hour                     |
| **Contains** | Problem, solution, what changed, results         | Spec, constraints, acceptance criteria |

A ri ho might _contain_ several agent tasks. The ho documents the session; the agent tasks document individual delegated pieces within it.

---

## Template Begins Here

> _Everything below this line is the ho document structure. Authoring notes are inline._

---

```markdown
# Ho [N]: [Title]

**Date:** [date]
**Status:** ✅ Complete / 🔄 In Progress / ⚠️ Partial
**Commit:** [hash or range]
```

> 📐 **AUTHOR — Header**
>
> Minimal. Date, status, commit hash. No duration field — ri work takes as long as it takes. No "Decision Required" field — if there's a major decision to make, use the ha template.
>
> The commit hash links the document to the actual code change. For work spanning multiple commits, use a range: `a1b2c3d..e5f6g7h`.

---

```markdown
## Problem

[What's wrong, what's missing, or what needs to change. Brief and precise.
Not "the system has issues with clips" but "departure clips extract from
end-of-file instead of last detection time, producing 30s of empty nest."]
```

> 📐 **AUTHOR — Problem**
>
> One paragraph, maybe two. The practitioner knows their system — they don't need context-setting. The problem statement serves the reader who comes later and needs to understand _why_ this change was made.
>
> If the problem takes more than a few sentences to explain, either:
>
> - It's actually multiple problems (split into separate hos or tasks), or
> - It needs architectural thinking (use the ha template)

---

```markdown
## Solution

[What was done and why this approach. Include diagrams, state transitions,
or architecture sketches if they clarify the change. Keep it proportional
to the complexity of the work.]
```

> 📐 **AUTHOR — Solution**
>
> This is the core of the document. Not a tutorial — a technical record. Proportionality matters: a one-line config change gets a one-line explanation. A state machine redesign gets a diagram and a rationale.
>
> "Why this approach" is brief at ri stage — the practitioner isn't evaluating five options. But a sentence on _why_ still has value: "Used a ring buffer instead of segment files because RAM is cheap and segment boundary math was the source of the timing bugs."

---

```markdown
## What Changed

| File               | Change                            |
| ------------------ | --------------------------------- |
| `path/to/file.py`  | [What was added/modified/removed] |
| `path/to/other.py` | [What was added/modified/removed] |

**Added:** [count] files, ~[count] lines
**Modified:** [count] files
**Removed:** [count] files, ~[count] lines
```

> 📐 **AUTHOR — What Changed**
>
> A manifest. What files were touched and what happened to them. This is the section that makes ri-stage documentation operationally useful — when something breaks in six months, the change manifest tells you where to look.
>
> The line count summary (added/removed) is optional but revealing. Net negative line counts are a sign of healthy ri-stage work — the practitioner is simplifying, not just adding.

---

```markdown
## Verification

[One line for routine work. A few lines for anything significant.
What was run, what it caught.

Examples:

- "Tests pass (14/14). Lint clean. Self-reviewed agent output — no issues."
- "Tests pass. Layer 2: caught a missing null check in the clip offset. Layer 3
  applied — cross-agent review confirmed the state transition logic is correct."
- "All quality tools clean. Small config change; tests cover the behavior."]
```

> 📐 **AUTHOR — Verification**
>
> Brief by design. The practitioner records what verification was applied — which layers, and what was caught. The value is in the record, not the prose. A one-liner is the norm for routine work; a few lines for anything non-trivial.
>
> If nothing was caught, say so: "All clean." If a layer was skipped deliberately, note it: "No cross-agent review — isolated utility function with full test coverage." Deliberate choices are documented decisions; unrecorded omissions are gaps.
>
> See [[verification-practices|Verification Practices]] (framework/structure/verification-practices.md) for the full four-layer stack.

---

```markdown
## Results

[What's working now that wasn't before. Test output if relevant.
Observed behavior. Performance notes if applicable.]
```

> 📐 **AUTHOR — Results**
>
> Evidence that the solution works. Can be as brief as "all tests pass, deployed, monitoring" or as detailed as test output, performance measurements, or before/after comparisons. Proportional to the stakes.

---

```markdown
## Notes

[Optional. Anything worth recording that doesn't fit above:

- Edge cases discovered
- Related issues found but not addressed
- Future work this enables or requires
- Gotchas for anyone touching this code later]
```

> 📐 **AUTHOR — Notes**
>
> Optional section. This is where the practitioner leaves breadcrumbs for their future self. "The freeze-frame approach works for <5s outages but will produce visible artifacts for longer gaps — consider black frames or a 'stream interrupted' overlay if this becomes a problem."
>
> Not every ho needs this. Don't pad it.

---

## That's It

Five sections: Problem, Solution, What Changed, Results, Notes. No phases, no verification questions, no understanding tiers, no devlog template, no confidence scale. The practitioner has internalized all of that. The template is a recording format, not a learning structure.

### What's Deliberately Absent (and Why)

**No AI Collaboration Guide.** The practitioner knows when to think and when to delegate. They don't need a template telling them to use thinking mode for design and agent mode for implementation.

**No Understanding Tiers.** By ri stage, the practitioner self-assesses automatically. They know what they know, what they don't, and what they're treating as a black box. Declaring tiers would be busywork.

**No Devlog.** The ri ho document IS the devlog. In shu, the devlog is a learning journal. In ha, it's a decision record. In ri, the ho document itself — problem, solution, changes, results — is the complete record. A separate devlog would duplicate it.

**No Duration or Phase Timing.** Ri work isn't structured in phases. The practitioner may think for five minutes, build for three hours, and reflect while committing. Or they may spend two hours debugging and twenty minutes implementing. The rhythm is their own.

**No Commit Cadence Guidance.** The practitioner's commit habits are established. They commit at natural breakpoints without being reminded.

**No "What's Next."** If the practitioner needs to track follow-up work, they create another ho or agent task. The document records what was done, not what might be done later — except in Notes, where future concerns are fair game.

---

## Structural Constraints (Cross-Project, Non-Negotiable)

Even at ri stage, some constraints hold:

1. **The document must exist.** Ri-stage work that isn't recorded didn't happen — not for learning purposes, but for maintenance purposes. If you changed the system, write it down.
2. **Problem and Solution are required.** What was wrong, what you did about it. Everything else is optional.
3. **What Changed must be traceable.** Either a file manifest, a commit hash, or both. Someone must be able to find the actual code changes from this document.
4. **Quality tools are still run.** No template section enforces this — but tests pass, linting is clean, types check. If they don't, that's a problem regardless of what the template says.

---

## Signals That This Should Be a Ha-Stage Ho Instead

Use the ha template when:

- The work requires choosing between **meaningfully different approaches.** If you're weighing a redesign — not just a fix — that's ha-stage thinking even if you're an experienced practitioner.
- The change is **architecturally significant.** Adding a new state to the state machine is ri. Redesigning the state machine from 4 states to 3 is ha.
- You find yourself **writing more than a paragraph in the Solution section** to justify your approach. That's a signal that the decision deserves the full think → execute → reflect treatment.
- The work **affects multiple system boundaries.** A change that touches detection, recording, AND notification is architectural even if each individual piece is straightforward.

## Signals That This Should Be an Agent Task Instead

Use an agent task when:

- The work is a **single, well-defined implementation task** that you're handing directly to AI.
- You can write the **acceptance criteria before starting.**
- The work doesn't need a problem/solution narrative — it needs a **specification.**
- You'll review the output but you're not doing the implementation yourself.

---

## The Kanyō Evidence

Every ri-stage Kanyō ho follows the same natural pattern, even though no template prescribed it:

**[[ho-05_7-state-redesign|Ho 05.7 — State Machine Redesign]] (examples/kanyo-pilot/ho-05_7-state-redesign.md):** Problem (4-state complexity, moov atom errors) → Solution (simplify to 3 states) → What Changed (three phases of cleanup) → Results (938 lines removed, departure clips fixed). 124 lines total.

**[[ho-05_72-startup-confirmation|Ho 05.72 — Startup Confirmation]] (examples/kanyo-pilot/ho-05_72-startup-confirmation.md):** Problem (false positives on container restart) → Solution (PENDING_STARTUP state, freeze-frame handling, outage recovery) → Files Changed (table) → state diagram → testing results. 104 lines.

**[[ho-06_12-admin-gui-code-check|Ho 06.12 — Code Quality Check]] (examples/kanyo-pilot/ho-06_12-admin-gui-code-check.md):** Ran quality tools, found bugs, fixed them. Maintenance work documented for the record.

**[[ho-06_51-cloudflare-tunnnel|Ho 06.51 — Cloudflare Tunnel]] (examples/kanyo-pilot/ho-06_51-cloudflare-tunnnel.md):** Infrastructure deployment documented as a runbook. Not a learning document — an operational reference.

The pattern was already there. The template just names it.

---

_This template is part of the Ho System framework._
_For shu-stage work, see [[shu-ho-template|Shu Ho Template]] (shu-ho-template.md)._
_For ha-stage work, see [[ha-ho-template|Ha Ho Template]] (ha-ho-template.md)._ _For agent task specification, see [[agent-task-spec|Agent Task Specification]] (agent-task-spec.md)._
_For template selection guidance, see the [[template-selection-guide|Template Selection Guide]] (template-selection-guide.md)._
