# Ha-Stage Ho Template

## Template for Decision-Driven Development Sessions

**Stage:** Ha (破) — Break from the Form
**Use when:** The learner has established system understanding, can identify tradeoffs, and is making architectural decisions rather than following prescribed ones.
**See:** [[shu-ha-ri|Shu-Ha-Ri Progression Model]](framework/structure/shu-ha-ri.md) for when to use this template vs. others.

---

## How to Use This Template

This template has two layers:

1. **The ho structure** — sections marked with `#` headers that appear in the finished ho document.
2. **Authoring notes** — blocks marked with `> 📐 AUTHOR` that explain *why* a section exists, what's cross-project (fixed) vs. project-specific (adapt), and how AI should handle it.

When creating a ho from this template:
- Keep all `#` header sections (they are structurally required)
- Read the authoring notes, then delete them from the finished document
- Sections marked **[LEARNER COMPLETES]** are filled in by the learner during the session, not the author
- The ha template is *lighter* than shu. Resist the urge to over-specify.

### How Ha Differs from Shu

The shu template is organized around **parts** — sequenced steps the learner follows. The ha template is organized around **phases** — thinking, execution, and reflection. The difference is structural, not cosmetic:

| | Shu | Ha |
|---|---|---|
| **Organizing unit** | Parts (4–9, author-defined) | Phases (3, fixed structure) |
| **Who scopes the work** | The ho author | The learner |
| **Duration** | ~2 hours (strict) | 2–4 hours (guideline) |
| **Primary artifact** | Working code/config | Decision documentation + working code |
| **AI relationship** | "Review and verify" | "Think together, then execute" |
| **Devlog character** | Learning journal | Design decision record |
| **Commit rhythm** | After every part | At natural breakpoints |

The most important difference: in shu, the ho author has done the architectural thinking. In ha, *the learner does the architectural thinking*, and the template provides the structure for doing it well.

### The Three-Phase Model

Every ha-stage ho follows three phases:

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│  PHASE 1: THINK                                     │
│  Define the problem. Evaluate approaches.           │
│  Make the decision. Document it BEFORE building.    │
│                                                     │
│  AI mode: Thinking partner (Claude / chat)          │
│                                                     │
│         ↓ decision documented                       │
│                                                     │
│  PHASE 2: EXECUTE                                   │
│  Build what you decided to build.                   │
│  Use agent tooling for implementation speed.        │
│                                                     │
│  AI mode: Implementation agent (Claude Code / etc)  │
│                                                     │
│         ↓ implementation complete                   │
│                                                     │
│  PHASE 3: REFLECT                                   │
│  Did the implementation reveal something the        │
│  design missed? What would you change? What         │
│  did you learn about your own judgment?             │
│                                                     │
│  AI mode: Thinking partner (back to chat)           │
│                                                     │
└─────────────────────────────────────────────────────┘
```

This is what Ho 05.6 (Architecture Redesign) in the Kanyō pilot did naturally — the "Thinking Process" and "Alternatives" sections happened before any code was written. The template makes that standard practice rather than something that happens only when the learner is thoughtful enough to do it unprompted.

---

## Template Begins Here

> *Everything below this line is the ho document structure. Authoring notes are inline.*

---

```markdown
# Ho [N]: [Title]

## [Subtitle — one sentence describing what this ho accomplishes]
```

> 📐 **AUTHOR — Title & Subtitle**
> *Cross-project structure. Project-specific content.*
>
> Same conventions as shu: memorable title, clear subtitle. The subtitle for a
> ha-stage ho often describes a *decision* or *transformation* rather than a
> *feature*: "Redesign clip extraction from segment-based to buffer-based"
> rather than "Set up clip extraction."
>
> Numbering: ha-stage hos often emerge mid-project as decimal numbers (5.5, 5.6)
> because they weren't in the original Ho Overview. This is expected and correct —
> the learner is scoping work the author didn't anticipate.

---

```markdown
**Date:** [date]
**Duration:** 2–4 hours (flexible)
**Goal:** [What is TRUE at the end that wasn't true at the start]
**Deliverable:** [What EXISTS at the end that didn't exist before]
**Decision Required:** [The core architectural or design choice this ho must resolve]
```

> 📐 **AUTHOR — Header Fields**
> *Cross-project structure. Project-specific content.*
>
> **Duration** is 2–4 hours, stated as a guideline. The learner has earned
> enough self-awareness to manage their own time. The guideline serves as a
> calibration check, not a wall — if the session is pushing past 4 hours,
> the learner should consciously decide whether to continue or split the work.
>
> **Decision Required** is new relative to shu. This field forces the ho to
> name the choice that must be made. If you can't articulate what decision
> the learner needs to make, the ho might be shu-stage work (follow instructions)
> or ri-stage work (just execute) rather than genuine ha-stage work.
>
> Examples of good "Decision Required" statements:
> - "Choose between tee-based and buffer-based clip extraction architecture"
> - "Select frontend technology stack for admin, viewer, and public interfaces"
> - "Design the state machine for visit lifecycle management"
> - "Determine development workflow — image rebuilds vs. volume-mount iteration"

---

```markdown
---

## Context

**Where this fits:** [What's been built so far. What works. What doesn't.
What prompted this ho — a bug, a missing capability, a scaling problem,
a design smell.]

**What's at stake:** [Why this decision matters. What does a good outcome
enable? What does a bad outcome cost? Is this reversible or a one-way door?]

**Constraints:** [What's fixed that the learner can't change — existing
interfaces, deployment model, hardware limitations, time pressure,
dependencies from other components.]
```

> 📐 **AUTHOR — Context**
> *Cross-project structure. Project-specific content.*
>
> This replaces shu's "Why This Ho Matters" and "Prerequisites." A ha-stage
> learner doesn't need motivational framing or a prerequisites checklist — they
> know why they're here and they can verify their own readiness.
>
> What they DO need is *decision context*: what's the current state, what
> constraints exist, and what's at stake. This is the information required
> to make a good architectural choice.
>
> **"What's at stake"** is particularly important. Not every decision has
> equal weight. "Which CSS framework to use" is reversible with moderate effort.
> "How to structure the state machine" shapes everything downstream. The learner
> should calibrate their thinking investment to the decision's weight.
>
> **"Constraints"** prevents the learner from designing in a vacuum. The best
> architecture in the abstract may be wrong given the actual constraints.
> "RAM is cheap" was a constraint-aware insight in Ho 05.6 — buffer-based
> architecture was viable *because* the deployment target had 16GB RAM.

---

```markdown
---

## Understanding Tiers for This Ho

**Tier 2 — Functional Understanding** (the work of this ho):
- [Component/concept]: [What understanding looks like after this session]
- [Component/concept]: [What understanding looks like after this session]

**Tier 1 — Black Box** (use but don't investigate):
- [Component]: [What you need to know to use it]

**Tier 3 — Deep Understanding** (if this ho develops it):
- [Component]: [What architectural understanding this ho should produce]
```

> 📐 **AUTHOR — Understanding Tiers**
> *Cross-project structure. Project-specific content.*
>
> Ha-stage hos are more likely to produce Tier 3 understanding than shu-stage
> hos. When the learner is making architectural decisions — evaluating tradeoffs,
> choosing between approaches, designing system interactions — they're developing
> deep understanding of the components involved.
>
> The tier declarations also help the learner know when they're over-investigating.
> If a component is Tier 1 for this ho, the learner shouldn't spend 45 minutes
> understanding its internals when they should be making a decision about
> something else.

---

```markdown
---

## Phase 1: Think

> **Mode: Thinking Partner** — Use conversational AI (Claude / chat).
> Do NOT start building until this phase produces a documented decision.

### The Problem

**[LEARNER COMPLETES]**

[Define the problem in your own words. Not what the ho says — what YOU
understand the problem to be. Include:
- What's currently happening that shouldn't be
- What's not happening that should be
- Why the current approach is insufficient]
```

> 📐 **AUTHOR — The Problem (Learner-Defined)**
> *Cross-project structure. The learner fills this in.*
>
> This is the first major structural difference from shu. In shu, the ho
> *tells* the learner what the problem is. In ha, the learner *articulates*
> the problem themselves. The ho provides context (above), but the problem
> definition is the learner's work.
>
> This matters because problem definition IS architectural thinking. How you
> frame a problem constrains which solutions you can see. The learner who
> frames the tee issue as "segment timing is broken" will debug the timing.
> The learner who frames it as "we're solving a memory problem with a storage
> approach" will redesign the architecture.
>
> If a facilitator is involved, this is a key coaching moment. Is the learner
> framing the problem at the right level of abstraction?

---

```markdown
### Approaches

**[LEARNER COMPLETES]**

[Identify at least 2 approaches. For each one:
- Brief description (what is it?)
- Key advantage (what's the best thing about it?)
- Key risk (what could go wrong or what does it cost?)
- Fit with constraints (does it work given what's fixed?)]

| Approach | Advantage | Risk | Fits Constraints? |
|---|---|---|---|
| [Approach A] | [Best thing] | [Worst thing] | [Yes/No/Partially — why] |
| [Approach B] | [Best thing] | [Worst thing] | [Yes/No/Partially — why] |
| [Approach C] | [Best thing] | [Worst thing] | [Yes/No/Partially — why] |
```

> 📐 **AUTHOR — Approaches**
> *Cross-project structure. The learner fills this in.*
>
> The table format is a scaffold, not a requirement — the learner may prefer
> prose or a different structure. What IS required is that multiple approaches
> are considered and compared. "I'm going to do X" without considering Y and Z
> is not ha-stage thinking — it's either shu (following implicit instructions)
> or ri (already knowing the answer).
>
> The "Fits Constraints?" column forces the learner to check their ideas against
> reality. The most elegant solution may be unshippable given the constraints.
>
> **How to use AI here:** This is the highest-value moment for thinking-mode AI.
> The learner should describe the problem and ask the AI to help them identify
> approaches they haven't considered. But the evaluation — which approach is
> best for THIS situation — must be the learner's judgment, not the AI's
> recommendation. The AI can inform; the learner decides.
>
> Prompt patterns for this phase:
> - "I'm trying to solve [problem]. I've thought of [A] and [B]. What other
>   approaches should I consider?"
> - "What are the tradeoffs between [A] and [B] for a system with [constraints]?"
> - "What am I not thinking about? What failure modes does [approach] have?"

---

```markdown
### Decision

**[LEARNER COMPLETES]**

**Chosen approach:** [which one]

**Why:** [The reasoning. Not just "it's simpler" but WHY simpler matters
here. What does this choice optimize for? What does it sacrifice?
What would make you reconsider?]

**What this means for implementation:**
- [Key implementation implication 1]
- [Key implementation implication 2]
- [What changes in the existing system]
- [What new components are needed]
```

> 📐 **AUTHOR — Decision**
> *Cross-project structure. The learner fills this in.*
>
> The decision section is the most important artifact in a ha-stage ho. It's
> the evidence that architectural thinking happened. Without it, the learner
> may have made a good decision but has no record of *why* — which means
> they can't learn from it, defend it, or know when to revisit it.
>
> **"What would make you reconsider?"** is a powerful question. It forces
> the learner to identify the assumptions behind their decision. If those
> assumptions change, the decision should be re-evaluated. This is how
> practitioners develop judgment — not by being right every time, but by
> knowing the conditions under which their decisions hold.
>
> This section must be written BEFORE Phase 2 begins. Not after. Not during.
> Before. The discipline of documenting the decision before implementing it
> prevents the common failure mode of rationalizing whatever you happened to
> build. "I chose buffer-based because it's simpler" written before
> implementation is a decision. Written after implementation, it's a story
> you're telling yourself.

---

```markdown
---

## Phase 2: Execute

> **Mode: Implementation Agent** — Use agent tooling (Claude Code / Copilot).
> Build what you decided to build in Phase 1. If the implementation reveals
> that your decision was wrong, STOP and go back to Phase 1.

### Implementation Plan

**[LEARNER COMPLETES]**

[Break the implementation into working steps. These are NOT shu-style parts
with 10-minute time estimates and verification questions. They're YOUR
decomposition of the work — what to build first, what depends on what,
where the risks are.]

1. [First thing to build — why this first?]
2. [Second thing — what does it depend on?]
3. [Third thing — where's the risk?]
...
```

> 📐 **AUTHOR — Implementation Plan**
> *Cross-project structure. The learner fills this in.*
>
> The learner defines their own work decomposition. This is a deliberate
> contrast with shu, where the ho author sequences the parts. The ability
> to break a large task into ordered steps is itself a skill that ha-stage
> practice develops.
>
> The prompt questions ("why this first?", "what does it depend on?",
> "where's the risk?") are scaffolding — they help the learner think about
> sequencing rather than just listing. They can be dropped as the learner
> internalizes the thinking.
>
> **The escape clause is critical:** "If the implementation reveals that your
> decision was wrong, STOP and go back to Phase 1." This is not failure —
> it's feedback. The willingness to abandon an implementation that isn't
> working and rethink the approach is the signature of ha-stage competence.
> In shu, the learner follows through because the author designed correctly.
> In ha, the learner must recognize when their own design is wrong.

---

```markdown
### Work Log

**[LEARNER COMPLETES DURING EXECUTION]**

[Track what you actually did, not what you planned to do. Brief notes —
this isn't a tutorial, it's a record.]

- [timestamp/step] [What was done. What happened. Any surprises.]
- [timestamp/step] [What was done. Deviation from plan? Why?]
- [timestamp/step] [What was done. Problems encountered?]

### Agent Task Log

[If you delegated work to agent-mode AI during this phase, track it here.
What did you hand off? What did you review? Did you catch anything?]

- [Task handed to agent]: [Brief description]
  - Review notes: [What you checked. What you accepted/rejected/modified.]
- [Task handed to agent]: [Brief description]
  - Review notes: [What you checked.]
```

> 📐 **AUTHOR — Work Log and Agent Task Log**
> *Cross-project structure. The learner fills this in during execution.*
>
> The Work Log replaces shu's per-part verification steps. The learner is
> tracking their own progress rather than checking off someone else's steps.
>
> The **Agent Task Log** is new and important. It creates a record of what
> was delegated to AI agents and what the human reviewed. This serves two
> purposes:
>
> 1. **Learning:** The learner develops judgment about what to delegate
>    and what to do manually. Over time, the log reveals patterns in their
>    delegation decisions.
>
> 2. **Quality:** If a bug surfaces later, the agent task log helps trace
>    whether it came from a delegated task that wasn't reviewed carefully
>    enough. This is diagnostic, not punitive.
>
> The log should be brief. "Handed agent the test scaffolding, reviewed,
> added two edge cases it missed" is sufficient. This is working documentation,
> not a report.

---

```markdown
### Commit Points

[Commit at natural breakpoints — when a coherent piece of work is complete.
Not after every small change, not in one giant commit at the end.]

\```bash
git add [files]
git commit -m "[conventional commit message]"
\```
```

> 📐 **AUTHOR — Commit Points**
> *Cross-project structure.*
>
> In shu, commits happen after every part — externally mandated rhythm.
> In ha, the learner determines commit granularity. The template reminds
> them to commit at natural breakpoints but doesn't prescribe when.
>
> If the learner isn't committing at all (one giant commit at the end),
> that's worth noting in reflection — it suggests either the work wasn't
> decomposable or the habit hasn't fully internalized.

---

```markdown
---

## Phase 3: Reflect

> **Mode: Thinking Partner** — Back to conversational AI.
> The implementation is done. Now evaluate it honestly.

### Outcome

**[LEARNER COMPLETES]**

**What was built:**
[Concrete artifacts — files created, files modified, files removed.
What exists now that didn't before.]

**Does it meet the goal?** [Yes / Partially / No — and why.]

**Decision quality:** [Was the Phase 1 decision right? Did implementation
confirm it or challenge it? What would you decide differently with
what you know now?]

### What the Implementation Revealed

**[LEARNER COMPLETES]**

[This is the section that makes ha-stage reflection different from shu.
The question isn't "what did I learn about [technology]?" It's "what
did I learn about my own judgment?"

- Did the implementation go as planned? Where did it deviate?
- Were the risks you identified the actual risks, or did different ones emerge?
- Did any of your assumptions turn out to be wrong?
- Was the problem actually the problem you defined in Phase 1?]
```

> 📐 **AUTHOR — Outcome and Implementation Revealed**
> *Cross-project structure. The learner fills this in.*
>
> **"Decision quality"** is the ha-stage equivalent of shu's understanding
> verification. Instead of "can you explain X?" it asks "was your decision
> good, and how do you know?"
>
> **"What the Implementation Revealed"** is where the deepest learning
> happens. The gap between what the learner designed and what they
> actually had to build is where judgment develops. A learner who finds
> no gap is either exceptionally skilled or not looking hard enough.
>
> From Ho 05.6 in Kanyō: the implementation revealed that the confidence
> threshold was too low (false positive at 0.41). This wasn't an
> architecture problem — it was a parameter that only became visible
> through live testing. The reflection captures this, making it
> available for future decisions.

---

```markdown
### Performance and Quality

**[LEARNER COMPLETES]**

**Tests:** [What's passing. What was added. What was removed.]

**Quality pipeline:** [Did linting/typing/formatting pass? Any new issues?]

**Performance observations:** [Anything notable — speed, resource usage,
behavior under load. Only if relevant to this ho.]
```

> 📐 **AUTHOR — Performance and Quality**
> *Cross-project structure. The learner fills this in.*
>
> This section maintains the quality discipline established in shu without
> the step-by-step "run this command" scaffolding. The learner reports on
> quality rather than being walked through it.
>
> If the learner consistently skips this section or reports no information,
> that's a signal. Either quality tools aren't being run (a problem) or
> the learner doesn't consider quality part of the work (a bigger problem).

---

```markdown
---

## Devlog Entry

Create or update: `devlog/ho-[N]-[slug].md`

\```markdown
# Ho [N]: [Title] — Devlog

**Date:** [date]
**Duration:** [actual time — be honest about whether Phase 1 and Phase 2 were separate sessions or continuous]
**Status:** Complete / Partial / Revisiting

## The Decision

**Problem as I defined it:** [Brief — what you wrote in Phase 1]

**Approach chosen:** [What and why — the short version]

**Decision held up?** [Yes / Partially / No — what changed during implementation]

## What Was Built

[Concrete artifacts. Files, components, configurations.]

## What Was Removed

[Equally important. What was deleted, simplified, replaced?
Removal is a sign of architectural maturity — it means
the learner has enough confidence to reduce complexity.]

## Understanding Tiers Achieved

**Tier 2 (Functional):**
- [What I now understand well enough to modify and explain] ✓/✗

**Tier 3 (Deep):**
- [What I now understand at an architectural level — what I designed and could redesign]

**Tier 1 (Black Box):**
- [What I consciously left as a black box]

## AI Collaboration Reflection

**Thinking mode (Phase 1):**
[How did you use conversational AI to think through the problem?
What questions helped? Where did the AI push you toward a better
approach? Where did you override the AI's suggestion and why?]

**Agent mode (Phase 2):**
[What did you delegate to agent tooling? What did you keep manual?
Where did you catch agent mistakes? Where did the agent do
something better than you would have?]

**The balance:**
[Looking at the full session — did you think enough before building?
Did you build too long without reflecting? Was the Phase 1 → 2 → 3
rhythm useful, or did you find yourself looping back?]

## Lessons Learned

[Not "what I learned about [technology]" — that's shu.
Instead: "what I learned about making decisions in this kind of system."

Examples from Kanyō:
- "Step back before debugging" (Ho 05.6)
- "RAM is cheap, complexity is expensive" (Ho 05.6)
- "The boring solution is usually right" (Ho 05.6)
- "45-minute iteration cycles are unacceptable" (Ho 05.5)]

## Confidence Level: [1–5]

1 = I made a decision but I'm not sure it was right
2 = I think the decision was reasonable but I can't fully defend it
3 = I can explain my decision, its tradeoffs, and when to revisit it
4 = I'm confident in the decision and could apply the same reasoning to a new problem
5 = I developed a principle or heuristic I'll use again

## What I'd Do Differently

[Hindsight. Not self-criticism — honest calibration. What would a
second attempt look like?]
\```
```

> 📐 **AUTHOR — Devlog**
> *Cross-project structure. Cross-project template.*
>
> The ha-stage devlog is fundamentally different from shu. It's organized
> around *the decision* rather than *what was learned*. The key sections:
>
> **"The Decision"** — a compressed record of the Phase 1 thinking. Someone
> reading the devlog months later should understand what was decided, why,
> and whether it held up.
>
> **"What Was Removed"** — gets its own section because removal is a ha-stage
> signature. In shu, the learner adds code. In ha, the learner starts
> removing code — simplifying, consolidating, deleting dead paths. This
> should be celebrated, not hidden.
>
> **"AI Collaboration Reflection"** — now split into thinking mode and agent
> mode, with a "balance" question. This is where the learner develops the
> meta-skill of knowing when to think and when to execute. Over multiple
> ha-stage hos, the reflection should show increasing sophistication in
> how the learner uses AI across modes.
>
> **Confidence scale is recalibrated for ha.** In shu, confidence measures
> understanding. In ha, it measures *judgment* — can you defend the decision
> and apply the reasoning elsewhere? A ha-stage learner at level 5 has
> extracted a reusable principle, not just solved a problem.
>
> **"What I'd Do Differently"** is especially important in ha because the
> learner made real choices that could have gone differently. This isn't
> about feeling bad — it's about calibrating future decisions.

---

```markdown
---

## What's Next

**Opened up by this ho:** [What's now possible that wasn't before?
What new work does this decision enable?]

**Remaining concerns:** [What's still unresolved? What was deferred?
What needs monitoring?]

**Next ho:** [If known — title and one-sentence goal]
```

> 📐 **AUTHOR — What's Next**
> *Cross-project structure. Project-specific content.*
>
> Ha-stage work often opens up multiple paths. The architecture redesign
> enables GUI development, YOLO fine-tuning, and deployment improvements —
> all at once. "What's next" may be a choice rather than a sequence, and
> the learner may need to decide which path to follow.
>
> "Remaining concerns" is important because ha-stage decisions often
> involve conscious deferral: "I chose buffer-based but I'm aware that
> RAM usage may be a problem on smaller machines — monitor this." This
> creates a debt register that can be revisited in future hos.

---

```markdown
---

*Ho authored by: [name or "self-directed"]*
*Template version: ha-v1 (draft)*
*Project: [project name]*
*Phase 1 duration: [thinking time]*
*Phase 2 duration: [execution time]*
*Phase 3 duration: [reflection time]*
```

> 📐 **AUTHOR — Metadata**
> *Cross-project structure.*
>
> The phase duration split is new. Tracking how long the learner spends
> thinking vs. executing vs. reflecting reveals patterns over time:
>
> - Consistently short Phase 1 → may be under-thinking before building
> - Consistently short Phase 3 → may be skipping reflection
> - Phase 1 taking most of the time → the decision IS the work (this is fine)
> - Phase 2 taking most of the time → may be shu-stage work in disguise
>
> "Self-directed" as an authoring credit is valid and expected in ha stage.
> Many ha-stage hos are not written by an external author — they're scoped
> by the learner themselves. The template provides structure; the learner
> provides content.

---

## Structural Constraints (Cross-Project, Non-Negotiable)

These constraints apply to ALL ha-stage hos regardless of project:

1. **Three phases: Think → Execute → Reflect.** This sequence is not optional. Phase 1 must produce a documented decision before Phase 2 begins.
2. **2–4 hour guideline.** Not a wall. If the session is productive past 4 hours, the learner continues consciously. If it's floundering, they stop and split.
3. **Decision documentation before implementation.** The Phase 1 decision section must be written BEFORE code is written. This is the single most important structural rule in the ha template.
4. **Multiple approaches evaluated.** The learner must consider at least 2 approaches. "There's only one way to do this" is almost never true and usually means the learner hasn't thought broadly enough.
5. **Agent task log maintained.** When work is delegated to agent AI, it's recorded. Not policed — recorded.
6. **Devlog entry required.** Written after completion, organized around the decision.
7. **Quality discipline maintained.** Tests and quality tools are the learner's responsibility, not the template's. But they must be run and reported on.
8. **Commit at natural breakpoints.** The learner determines granularity. Zero commits and one-giant-commit are both signals worth examining.

---

## Signals That This Should Be a Shu-Stage Ho Instead

Sometimes work that looks like ha is actually shu. Use the shu template when:

- The learner is entering a **genuinely new domain** (even if they're experienced elsewhere). Ha 05.6 was ha-stage for architecture redesign, but Ho 04 (Docker) was shu-stage despite coming later in the arc — because Docker was new territory.
- The learner **cannot articulate the problem** clearly enough to define approaches. If Phase 1 stalls because the learner doesn't know enough to evaluate options, they need more structured learning first.
- There is **one obviously correct approach** and the value is in learning to implement it well, not in choosing between alternatives.
- The learner **asks for step-by-step guidance.** This is not weakness — it's honest self-assessment. Give them the shu template.

## Signals That This Should Be a Ri-Stage Task Instead

And sometimes work that looks like ha is actually ri. Use the ri template when:

- The problem is **well-defined and the solution is known.** If the learner already knows what to build and how, Phase 1 adds friction without value.
- The work is **maintenance, debugging, or incremental improvement** rather than design.
- A full decision record would be **disproportionate** to the work. If the task is "fix the off-by-one in departure clip timing," a three-phase structure is overkill.
- The learner can **write the commit message before starting.** If they know the end state, it's execution, not decision-making.

---

## Checklist for Ho Authors

Before publishing a ha-stage ho, verify:

- [ ] The ho requires a genuine decision between competing approaches (not a predetermined path)
- [ ] Context section provides enough information to make the decision, without prescribing the answer
- [ ] Constraints are clearly stated (what's fixed, what's flexible)
- [ ] Understanding tiers are declared with appropriate Tier 3 expectations
- [ ] The template leaves room for the learner to define the problem, evaluate approaches, and make their own choice
- [ ] Phase 2 guidance is about process (how to execute well) not content (what to build)
- [ ] Devlog template is organized around the decision, not the learning
- [ ] Confidence scale is calibrated for judgment, not comprehension
- [ ] The ho is genuinely ha-stage — not shu-in-disguise or ri-in-disguise

---

## The Kanyō Evidence

The ha template is derived from what emerged naturally in the Kanyō pilot's middle phase:

**Ho 05.5 (Dev Testing Strategy):** Self-directed process improvement. The learner identified a workflow problem (45-minute iteration cycles), designed a solution (volume-mount development), and implemented it. No external author scoped this work. The output was a problem → solution → validation document.

**Ho 05.6 (Architecture Redesign):** The canonical ha-stage ho. ~4 hours of deep thinking + implementation. Problem definition → thinking process → alternatives table → decision → implementation → lessons learned. 800 lines removed. The document reads like a technical design record, not a learning journal.

**Ho 06 (GUI Architecture Planning):** Requirements analysis, technology evaluation, and deployment planning. The learner evaluated htmx vs. Alpine.js vs. React, designed interfaces for three different audiences, and made deployment decisions (local Docker vs. Cloudflare Pages).

What these hos share: the learner drove the problem definition, evaluated multiple approaches, made and documented a decision, then executed it. The template formalizes that pattern so it becomes repeatable rather than accidental.

What they lacked: explicit AI mode guidance (thinking vs. agent), agent task tracking, and structured Phase 3 reflection. The template adds these.

---

*This template is part of the Ho System framework.*
*For shu-stage work, see [[shu-ho-template|Shu Ho Template]](shu-ho-template.md).*
*For ri-stage work, see [[ri-ho-template|Ri Ho Template]](ri-ho-template.md).*
*For template selection guidance, see the [[template-selection-guide|Template Selection Guide]] (template-selection-guide.md).*
