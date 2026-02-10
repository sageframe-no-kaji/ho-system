# Tiered Understanding

## A Framework for Calibrated Comprehension

---

## 1. The Core Idea

Not everything needs to be understood at the same depth. This is not a concession — it's a principle. The learner who tries to deeply understand every component they touch will drown. The learner who understands nothing will build on sand. The Ho System's answer is explicit, deliberate triage: for every component in a project, decide how deeply you need to understand it _right now_, and declare that decision openly.

The tiered understanding model provides three levels:

**Tier 1 — Black Box.** You use it. You don't investigate it. You know _what_ it does and _how to invoke it_, but not _how it works internally_. This is a deliberate choice, not ignorance — you're triaging your attention toward what matters most.

**Tier 2 — Functional Understanding.** You understand it well enough to configure it, troubleshoot common issues, modify its behavior, and explain its role in the larger system. You could teach someone else to use it. This is the target level for most work in a ho.

**Tier 3 — Deep Understanding.** You understand it at an architectural or theoretical level. You could redesign it, extend it in non-obvious ways, or teach it formally. This level develops cumulatively — rarely within a single ho, often across a project arc.

The model does three things:

1. **Reduces cognitive load.** The learner knows what to focus on and what to defer. In a ho that introduces Docker, YOLO, and Python logging simultaneously, the learner doesn't need to understand all three at the same depth.
2. **Prevents false confidence.** Tier 1 is openly acknowledged, not hidden. "I don't understand how YOLO works internally" is a valid, honest, _useful_ statement — it tells you and anyone reading your devlog exactly where the boundary of your knowledge is.
3. **Creates a progression map.** Components move between tiers across successive hos as the learner's needs and capabilities evolve. What starts as Tier 1 may become Tier 2 when you need to debug it, and Tier 3 when you redesign it.

---

## 2. The Three Tiers in Detail

### Tier 1 — Black Box

**Definition:** The learner uses this component without understanding its internals. They know the inputs, the outputs, and the invocation pattern. They trust that it works.

**What Tier 1 looks like in practice:**

- "I call `model(frame)` and get detections back. I don't know how YOLO's neural network identifies objects."
- "I run `ffmpeg -i input -c:v libx264 output` and get a compressed video. I don't know how H.264 encoding works."
- "I use `collections.deque(maxlen=N)` as a ring buffer. I don't know how deque is implemented internally."

**What Tier 1 is NOT:**

- It's not "I've heard of it." Tier 1 requires functional usage — you can invoke it, you can read its output, you can pass it the right inputs.
- It's not "I'm confused by it." If you can't use it at all, you don't have Tier 1 — you have a prerequisite gap.

**When Tier 1 is appropriate:**

- Dependencies and libraries you consume but don't modify
- Infrastructure components you configure but don't build (Docker, CI/CD, CDN)
- Algorithms and protocols you use through standard interfaces
- Anything where the investment to reach Tier 2 doesn't pay off for the current work

**The honest self-assessment test:** "If this component broke in a way I've never seen before, I would need help." That's Tier 1, and that's fine.

### Tier 2 — Functional Understanding

**Definition:** The learner understands the component well enough to configure it, troubleshoot common issues, modify its behavior within its design parameters, and explain its role in the larger system to someone else.

**What Tier 2 looks like in practice:**

- "I understand Python virtual environments — isolation, activation, dependency management, and why they matter. I can troubleshoot path issues and explain the concept to a colleague."
- "I understand the detection confidence threshold — what it controls, how to tune it, the tradeoff between sensitivity and false positives, and why 0.5 is a reasonable starting point for our use case."
- "I understand Docker volumes — how they persist data, how mount paths work, the difference between named volumes and bind mounts, and when to use read-only mounts."

**What Tier 2 is NOT:**

- It's not "I could rebuild this from scratch." That's Tier 3.
- It's not "I've read the docs." Tier 2 requires demonstrated ability — you've configured it, troubleshot it, or modified it successfully.

**When Tier 2 is appropriate:**

- Components that are central to the work of the current ho
- Tools and patterns the learner will use repeatedly
- Configuration and tuning decisions that affect system behavior
- Anything the learner needs to modify, not just use

**The honest self-assessment test:** "I could explain this to someone else and answer their follow-up questions." That's Tier 2.

### Tier 3 — Deep Understanding

**Definition:** The learner understands the component at an architectural or theoretical level. They could redesign it, extend it in non-obvious ways, make structural changes, or teach it formally. They understand not just _what_ it does but _why_ it's designed that way and _what alternatives were considered_.

**What Tier 3 looks like in practice:**

- "I designed the event detection state machine. I understand every transition, the rationale for each timeout value, why we chose 3 states instead of 4, and how to extend it for new event types."
- "I understand the buffer-based clip architecture — why we moved from segments, the RAM/complexity tradeoff, how the ring buffer interacts with the state machine, and when this approach would break."
- "I understand the volume-mount development workflow — why it's faster than image rebuilds, when you need to rebuild anyway, the security implications of read-only mounts, and how it changes the deployment model."

**What Tier 3 is NOT:**

- It's not "I've memorized every detail." Tier 3 is structural understanding, not encyclopedic knowledge.
- It's not required for most components. A project might have 2–3 components at Tier 3 and dozens at Tier 1 and 2.

**When Tier 3 is appropriate:**

- Components the learner designed or redesigned
- Architectural decisions that shape the entire system
- The learner's primary domain (the thing they're becoming expert in)
- Components that will be extended significantly in future work

**The honest self-assessment test:** "I could redesign this if the requirements changed, and I could explain _why_ my redesign is better than alternatives." That's Tier 3.

---

## 3. How Tiers Are Used Across Stages

The tiered understanding model interacts with the shu-ha-ri progression. How tiers are declared, tracked, and expected shifts as the learner develops.

### Shu Stage: Tiers Are Declared by the Author

In shu, the ho author assigns tier levels for every component the learner will encounter. This is explicit scaffolding — it tells the learner "focus here, not there" before the session begins.

The declaration appears in the ho document:

```markdown
## Understanding Tiers for This Ho

**Tier 2 — Functional Understanding (the work of this ho):**
- Virtual environments: Understand isolation, activation, dependency management
- Git workflow: Understand staging, committing, pushing, branching basics

**Tier 1 — Black Box (use but don't investigate):**
- pytest internals: Just run `pytest` and read the output
- YOLOv8 installation: `pip install ultralytics` and trust it works

**Tier 3 — Deep Understanding (if this ho develops it):**
- (Not expected for this ho)
```

**Why it matters in shu:** Without tier declarations, the learner doesn't know where to spend their attention. They might spend 45 minutes trying to understand pytest's plugin system when all they need is `pytest` → green/red. The tier declaration prevents this.

**Devlog reflection in shu:** The learner reports on tiers achieved vs. declared:

```markdown
## Understanding Check

**Tier 2 (Functional):**
- Virtual environments ✓ — I understand why and how
- Git workflow ✓ — staging/committing makes sense, branching still fuzzy

**Tier 1 (Black Box):**
- pytest — I run it, I read the output, that's enough for now
- ffmpeg — "still feels like incantation" (honest Tier 1 assessment)
```

That "ffmpeg still feels like incantation" from Kanyō Ho 02 is a _perfect_ Tier 1 self-assessment. The learner knows they don't understand it, knows they don't need to yet, and says so clearly.

### Ha Stage: Tiers Are Declared by the Learner

In ha, the learner sets their own tier expectations. The template provides the structure, but the learner fills it in based on what they think the work requires.

Ha-stage work produces more Tier 3 understanding than shu-stage work, because the learner is making architectural decisions — evaluating tradeoffs, choosing between approaches, designing system interactions. You can't make good decisions about a component at Tier 1.

**Devlog reflection in ha:** The learner reports not just achievement but _judgment quality_:

```markdown
## Understanding Tiers Achieved

**Tier 2 (Functional):**
- Ring buffer implementation ✓ — I understand deque, thread safety, time-based retrieval

**Tier 3 (Deep):**
- Clip architecture ✓ — I designed the buffer approach, understand why it's better than tee,
  and know when the tradeoffs would reverse (RAM-constrained environments)

**Tier 1 (Black Box):**
- OpenCV VideoWriter internals — I trust the fourcc/fps/resolution API
```

The Tier 3 entry is the important one. "I know when the tradeoffs would reverse" is the signal of genuine deep understanding — not just "I built it and it works" but "I understand the design space well enough to know when my design would be wrong."

### Ri Stage: Tiers Are Not Declared

In ri, the practitioner self-assesses automatically. They know what they know. Declaring tiers in a ri ho document would be busywork — the practitioner isn't learning to calibrate their understanding; they've internalized the habit.

Tiers still _exist_ in ri — the practitioner still has Tier 1 components they use without investigating. But the explicit declaration mechanism has been absorbed into how they think, not how they document.

---

## 4. Tier Progression Across a Project Arc

Components don't stay at fixed tiers. They move as the project demands and the learner develops. Tracking this movement reveals the learning trajectory.

### The Kanyō Evidence

**YOLOv8:**

- Ho 02 (Falcon Vision): Tier 1 — `model(frame)` returns detections, trust it
- Ho 07 (YOLO Training): Tier 2 — understanding training data, annotation, model evaluation, confidence calibration
- Stayed Tier 1 for _architecture_ throughout — never needed to understand the neural network internals

**Python virtual environments:**

- Ho 01 (Git Good): Tier 2 — learned isolation, activation, dependency management
- Remained Tier 2 for the rest of the arc — never needed to go deeper, never forgot it

**State machine:**

- Ho 03 (Live Detection): Tier 2 — understood the states and transitions as given
- Ho 05.6 (Architecture Redesign): Tier 3 — redesigned the architecture
- Ho 05.7 (State Redesign): Tier 3 — simplified from 4 states to 3, removed 938 lines
- This progression (Tier 2 → 3) over multiple hos is the model working as intended

**Docker:**

- Ho 04 (Docker Deploy): Tier 2 — learned containerization, volumes, compose
- Ho 05.5 (Dev Testing): Tier 3 — designed the volume-mount development workflow, understood when to rebuild images vs. use mounts
- A _new domain_ that started at shu stage (within a project that was otherwise in ha/ri) and progressed to deep understanding through use

**ffmpeg:**

- Ho 02: Tier 1 — "feels like incantation"
- Ho 05.6: Still Tier 1 — used for clip extraction but didn't investigate internals
- Ho 06.51: Still Tier 1 — used in Cloudflare tunnel but didn't need to understand it deeper
- Some things _stay_ Tier 1 for an entire project, and that's correct

### The Progression Pattern

```
             Ho 01   Ho 02   Ho 03   Ho 04   Ho 05   Ho 06   Ho 07
             ─────   ─────   ─────   ─────   ─────   ─────   ─────
YOLOv8        —       T1      T1      T1      T1      T1      T2
venv          T2      T2      T2      T2      T2      T2      T2
State machine —       —       T2      T2      T3      T3      T3
Docker        —       —       —       T2      T3      T3      T3
ffmpeg        —       T1      T1      T1      T1      T1      T1
Git           T2      T2      T2      T2      T2      T2      T2
```

Reading across a row tells you the learning story for that component. Reading down a column tells you the cognitive load at that point in the arc.

---

## 5. How to Assign Tiers (For Ho Authors)

When writing a ho, the author must decide which tier each component belongs at. Here's the decision process:

**Ask: "What does the learner need to DO with this component in this ho?"**

- **Use it through a standard interface** → Tier 1
- **Configure it, modify it, or troubleshoot it** → Tier 2
- **Design it, redesign it, or make architectural decisions about it** → Tier 3

**Ask: "What's the cost if the learner doesn't understand this component?"**

- **Nothing — it works or it doesn't, and they can get help** → Tier 1
- **Confusion or wasted time when something goes wrong** → Tier 2
- **Bad architectural decisions with downstream consequences** → Tier 3

**Ask: "Will the learner encounter this component again?"**

- **Probably not in this project** → Tier 1 (don't invest)
- **Repeatedly, as a tool** → Tier 2 (invest once, reuse)
- **As something they own and extend** → Tier 3 (invest deeply)

### Common Author Mistakes

**Over-tiering:** Declaring everything as Tier 2 because "the learner should understand what they're using." This creates impossible hos — the learner can't develop functional understanding of eight components in two hours. Be ruthless about what's Tier 1.

**Under-tiering:** Declaring a critical component as Tier 1 when the learner actually needs to modify it. If the ho asks the learner to tune the detection confidence threshold, that's Tier 2 for the confidence system — not Tier 1.

**Missing Tier 3 opportunities:** Some hos naturally produce deep understanding (architecture redesigns, system design sessions), and the author should recognize and declare this. Not every ho needs Tier 3, but the ones that produce it should say so.

**Static assignments:** Assigning tiers once and never revisiting. A component that was Tier 1 in Ho 2 might need to be Tier 2 in Ho 5 because the work demands it. Check tier assignments against the actual work of each ho.

---

## 6. The Relationship to Confidence Scales

The templates include confidence self-assessment scales in the devlog. These are related to but distinct from tiers:

**Tiers measure what you understand.** "The state machine is Tier 3 for me — I designed it and could redesign it."

**Confidence measures how certain you are.** "I'm at confidence level 4 — I understand this well and could extend it independently."

They usually correlate (Tier 3 + high confidence, Tier 1 + low confidence) but not always. A learner might have Tier 2 understanding with low confidence ("I think I understand virtual environments but I'm not sure I could troubleshoot a path issue without help"). That's valuable information — it tells the facilitator where to reinforce.

The confidence scales are calibrated differently across stages:

**Shu confidence** measures _understanding_:

1. I followed instructions but don't really understand what happened
2. I understand the broad strokes but couldn't reproduce without guidance
3. I could explain this to someone and troubleshoot basic issues
4. I understand this well and could extend it independently
5. I could teach this and make non-obvious design decisions

**Ha confidence** measures _judgment_:

1. I made a decision but I'm not sure it was right
2. I think the decision was reasonable but I can't fully defend it
3. I can explain my decision, its tradeoffs, and when to revisit it
4. I'm confident in the decision and could apply the same reasoning to a new problem
5. I developed a principle or heuristic I'll use again

The shift from "understanding" to "judgment" mirrors the shift from "what tier am I at?" to "how good was my architectural thinking?" — both are forms of honest self-assessment, but they measure different things.

---

## 7. For Self-Directed Learners

If you're using the Ho System without a facilitator, the tiered understanding model is your most important self-regulation tool. Here's how to use it:

**Before each ho:** Declare your tiers. Write them down. Be honest about what's Tier 1 — there's no shame in it, and pretending you understand something you don't will cost you later.

**During the ho:** When you find yourself going down a rabbit hole, check the tiers. If the component is Tier 1 for this ho, stop investigating and use it as given. You can promote it to Tier 2 in a future ho if needed.

**After the ho:** Report honestly. Did you achieve the tier you targeted? If you aimed for Tier 2 and landed at Tier 1, that's information — maybe the ho was too ambitious, or maybe you need a follow-up session focused on that component.

**Across the arc:** Track tier progression. If nothing is moving from Tier 1 to Tier 2 over multiple hos, you might be staying too comfortable. If everything is at Tier 2 or 3, you might be over-investing in understanding at the expense of shipping.

The goal is not to get everything to Tier 3. The goal is to have the _right things_ at the _right tiers_ for the work you're doing.

---

## 8. Related Framework Documents

- [[design-seed|Design Seed]] (framework/design-seed.md) — §3.3 introduces the tiered understanding model
- [[shu-ha-ri|Shu-Ha-Ri Progression]] (framework/structure/shu-ha-ri.md) — How tier usage changes across stages
- [[shu-ho-template|Shu Ho Template]] (framework/templates/shu-ho-template.md) — Author-declared tier assignments
- [[ha-ho-template|Ha Ho Template]] (framework/templates/ha-ho-template.md) — Learner-declared tier assignments
- [[ri-ho-template|Ri Ho Template]] (framework/templates/ri-ho-template.md) — Tiers internalized, no longer declared

---

_This document is part of the Ho System framework. It describes the structural specification for the Tiered Understanding Model. For guidance on using tiered understanding in facilitation — including how to coach learners through honest self-assessment and how to detect miscalibrated tier assignments — see the facilitation layer (proprietary)._