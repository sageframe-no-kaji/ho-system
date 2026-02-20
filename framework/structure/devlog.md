# The Devlog

## Reflection as Practice

---

## 1. What a Devlog Is

A devlog is a structured reflection written by the learner after completing a ho. It records what was built, what was understood, what was hard, and what's still unclear. It's not a project log ("what I did today") and it's not a commit message ("what changed"). It's a learning document — the artifact that distinguishes "I built this" from "AI built this while I watched."

The devlog is the primary evidence of learning in the Ho System. Code can be generated. Tests can be generated. Configuration files can be generated. The devlog cannot be generated — it requires the learner to look back at their own experience and honestly assess what happened. An AI can write your code; it cannot write your reflection for you.

Three properties define a devlog:

**Honest.** The devlog rewards accuracy, not performance. "I don't understand how ffmpeg encoding works — it still feels like incantation" is more valuable than "I now have a solid grasp of the encoding pipeline." The first tells you exactly where the knowledge boundary is. The second tells you nothing.

**Immediate.** Written right after completing the ho, while the experience is fresh. Not the next day. Not "later." The details that matter most — the moment of confusion that resolved into understanding, the AI suggestion you overrode and why, the gotcha that cost twenty minutes — evaporate quickly.

**Structured.** Not a freeform journal entry. The template provides sections that ensure the reflection covers what matters: what was built, what was understood, what was difficult, and how AI was used. The structure prevents the learner from writing only about what went well and skipping what didn't.

---

## 2. Why the Devlog Exists

The devlog serves four audiences:

**The learner, right now.** The act of writing forces externalization. You can _feel_ like you understood something during the ho and discover, while writing the devlog, that you can't articulate it. That gap between felt understanding and articulated understanding is precisely what the devlog reveals.

**The learner, later.** Six months from now, when something breaks, the devlog tells you what you understood at the time, what you were uncertain about, and what you left as a black box. "I was at Tier 1 for ffmpeg encoding" tells your future self exactly where to look when the encoding breaks.

**The facilitator.** The devlog is the primary diagnostic tool. A facilitator reading a series of devlogs can see where the learner's confidence is growing, where it's stagnating, where tier assignments are honest vs. inflated, and whether the learner is developing the meta-skill of accurate self-assessment.

**The methodology.** Devlogs from completed arcs become evidence for the Ho System itself. They demonstrate the learning trajectory, provide examples for future learners, and help refine the methodology. The Kanyō devlogs are quoted throughout this framework because they document real learning in real time.

---

## 3. How the Devlog Transforms Across Stages

The devlog is one of the framework elements that changes most across the shu-ha-ri progression. Not because the principle changes — honest reflection is always the point — but because what's worth reflecting on shifts as the learner develops.

### Shu: The Learning Journal

In shu, the devlog asks: **What do I understand now?**

The learner is building foundational skills. The interesting question isn't "what did I decide" (the ho author made most decisions) but "what did I learn, and how well do I understand it?" The shu devlog is organized around understanding — tiers achieved, confidence level, gaps identified.

Key sections:

- What Was Built (concrete artifacts)
- Understanding Tiers Achieved (Tier 1/2/3 for each component, with ✓/✗)
- Challenges Encountered (what was hard)
- Key Learnings (specific insights, not vague summaries)
- AI Collaboration Reflection (how AI was used, where the learner thought independently)
- Files Created/Modified (reference list)
- Confidence Level (1–5, calibrated for understanding)
- What I'd Do Differently (hindsight)

The confidence scale measures _understanding_:

| Level | Meaning                                                                |
| ----- | ---------------------------------------------------------------------- |
| 1     | I followed instructions but don't really understand what happened      |
| 2     | I understand the broad strokes but couldn't reproduce without guidance |
| 3     | I could explain this to someone and troubleshoot basic issues          |
| 4     | I understand this well and could extend it independently               |
| 5     | I could teach this and make non-obvious design decisions               |

Most shu-stage learners will honestly be at 2–3 for their first few hos. A learner who reports 5 on their second ho is either exceptional or not being honest. Both are worth paying attention to.

### Ha: The Decision Record

In ha, the devlog asks: **Was my decision good?**

The learner is making real architectural decisions. The interesting question isn't "what did I learn" (they're past foundational learning) but "what did I decide, why, and did the implementation confirm or challenge that decision?" The ha devlog is organized around the decision.

Key sections:

- The Decision (problem as defined, approach chosen, whether it held up)
- What Was Built (concrete artifacts)
- What Was Removed (equally important — removal is a ha-stage signature)
- Understanding Tiers Achieved (with more Tier 3 expected)
- AI Collaboration Reflection (split into thinking mode and agent mode)
- Lessons Learned (principles extracted, not facts absorbed)
- Confidence Level (1–5, recalibrated for judgment)
- What I'd Do Differently (especially important when real choices were made)

The confidence scale shifts to measure _judgment_:

| Level | Meaning                                                                           |
| ----- | --------------------------------------------------------------------------------- |
| 1     | I made a decision but I'm not sure it was right                                   |
| 2     | I think the decision was reasonable but I can't fully defend it                   |
| 3     | I can explain my decision, its tradeoffs, and when to revisit it                  |
| 4     | I'm confident in the decision and could apply the same reasoning to a new problem |
| 5     | I developed a principle or heuristic I'll use again                               |

The "What Was Removed" section deserves emphasis. In shu, the learner adds code. In ha, the learner starts removing code — simplifying, consolidating, deleting dead paths. Kanyō Ho 05.6 removed 800 lines. Ho 05.7 removed 938. This is celebrated in the devlog, not hidden. The ability to remove is evidence of understanding — you can only simplify what you comprehend.

The AI Collaboration Reflection splits into two modes because ha-stage work uses AI differently in different phases:

- **Thinking mode (Phase 1):** Conversational AI as a thinking partner. What questions helped? Where did you override a suggestion?
- **Agent mode (Phase 2):** Agent AI as an implementation tool. What did you delegate? What did you catch? Where did the agent exceed your expectations?
- **The balance:** Did you think enough before building? Build too long without reflecting?

Over multiple ha-stage devlogs, this section should show increasing sophistication in how the learner uses AI across modes.

### Ri: Dissolved

In ri, the devlog doesn't exist as a separate document. The ri ho document itself — Problem, Solution, What Changed, Results, Notes — IS the record. Writing a separate devlog would duplicate it.

This isn't the practitioner skipping reflection. It's reflection absorbed into the work itself. A ri practitioner who writes "Problem: departure clips extract from end-of-file instead of last detection time. Solution: anchor clip start to first suspected arrival frame" _is_ reflecting — they're articulating what was wrong and why their fix is correct. The habit is internalized; it doesn't need a separate template.

---

## 4. Filing Convention

Devlog entries live in the project's `devlog/` directory, named to match their ho:

```
devlog/
├── ho-00-overview.md        (if kamae produces reflection)
├── ho-0_5-tool-mastery.md
├── ho-01-git-good.md
├── ho-02-falcon-vision.md
├── ho-03-live-detection.md
├── ho-04-docker-deploy.md
├── ho-05-deployment-verification.md
├── ho-05_5-dev-testing.md
├── ho-05_6-architecture-redesign.md
...
```

The filename matches the ho document filename with the same numbering convention (see [[ho-structure|Ho Structure]] (framework/structure/ho-structure.md) §3.5). One devlog per ho. The devlog is committed alongside the ho's code changes — it's a first-class project artifact, not metadata stored elsewhere.

For ri-stage work, there's no separate devlog file. The ri ho document itself (in the `devlog/` or `hos/` directory) serves both purposes.

---

## 5. What Makes a Good Devlog

### Good: Specific and Honest

```markdown
## Key Learnings

Frame processing at every frame is too slow for real-time.
Dropping to every 30th frame works because falcons move slowly —
the detection window is seconds, not milliseconds. But this means
we'll miss very brief flyovers. Acceptable tradeoff for now.

## Confidence Level: 3

I understand the pipeline end-to-end. I could explain it to someone.
But I'm not confident I could tune the detection thresholds without
trial and error — the relationship between confidence threshold and
false positive rate isn't intuitive to me yet.
```

This is good because it captures a _specific_ insight (frame interval tradeoff), names the _specific_ gap (threshold tuning), and the confidence assessment is calibrated — 3, not 5, with an explanation of what's missing.

### Bad: Vague and Performative

```markdown
## Key Learnings

I learned a lot about how detection works. The pipeline is really
interesting and I feel like I have a much better understanding now.

## Confidence Level: 4

I feel pretty good about this.
```

This is bad because it could describe any ho. It captures no specific insight, names no specific gap, and the confidence level has no reasoning behind it. A facilitator reading this learns nothing about the learner's actual understanding.

### Good: Honest About Difficulty

```markdown
## Challenges Encountered

Docker volumes confused me for about 45 minutes. I kept mounting
to the wrong path inside the container and couldn't figure out why
my config file wasn't being picked up. The issue was that I was
mounting to /app/config but the code expected /config. The distinction
between host path and container path isn't natural yet.

## Understanding Tiers

**Tier 2 (Functional):**

- Docker compose: ✓ — I understand services, volumes, networking basics
- Volume mounts: ✗ — I got it working but I'm not confident I could
  troubleshoot a different mount issue without help
```

The learner distinguishes between "Docker compose" (achieved Tier 2) and "volume mounts" (didn't quite get there). That ✗ is more valuable than a ✓ would be — it tells the facilitator exactly where to reinforce.

### Bad: Skipping Difficulty

```markdown
## Challenges Encountered

No major issues. Everything went smoothly.
```

Either the ho was too easy (scope problem) or the learner isn't reflecting honestly (honesty problem). Real work has friction. "No challenges" is almost always a signal that reflection was skipped.

### Good: AI Collaboration Insight

```markdown
## AI Collaboration Reflection

Claude suggested lowering the confidence threshold to 0.3 to catch
more detections. I pushed back — the problem was too MANY false
positives, not too few detections. Raising to 0.7 was the right call.

This was the first time I overrode an AI suggestion based on my own
understanding of the domain. The AI was pattern-matching on "detection
not working" → "lower threshold." I knew the actual problem was different.
```

This captures a _learning moment about AI collaboration_ — the meta-skill of knowing when the AI is wrong and having the confidence to override it.

### Recording Verification

A thorough devlog captures which verification layers were applied, what they caught, and — critically — what was not verified and why. This creates accountability for quality decisions without requiring perfection.

**Good verification record:**

```markdown
## Verification Applied

Layer 1: All 14 tests pass. Added 2 tests for the new state transition.
Layer 1b: black, flake8, mypy all clean. One type annotation added.
Layer 2: Prompted agent to self-review before I read — it caught a missed
edge case in the clip offset math.
Layer 3: Not applied — small config change with a clear acceptance test.
Skipping was a deliberate choice.
```

The "not applied" entries matter as much as the "applied" entries. They document conscious choices, not oversights. A layer skipped by default is a gap; a layer skipped deliberately is a decision.

**Weak verification record:**

```markdown
Tests pass. Looks good.
```

This tells you nothing about test coverage, whether higher layers were applied, or whether anything was caught and fixed. It's the verification equivalent of "no challenges" in the challenges section.

See [[verification-practices|Verification Practices]] (framework/structure/verification-practices.md) §5 for guidance on what each layer costs and when to apply it selectively.

---

## 6. The Devlog as Diagnostic Tool

For facilitators (or self-directed learners reviewing their own arc), devlogs reveal patterns across time:

**Rising confidence with rising specificity** = genuine development. The learner is understanding more AND getting better at articulating what they understand.

**Rising confidence with declining specificity** = inflation. The learner is rating themselves higher but the explanations are getting vaguer. This usually means the work is getting harder but the learner isn't admitting it.

**Stable confidence across many hos** = plateau. The learner isn't being challenged enough (if stable at 4–5) or isn't progressing (if stable at 2–3). Either case may need a stage adjustment.

**Tier declarations getting more nuanced** = developing judgment. Early devlogs have clean ✓/✗. Later devlogs have "✓ for basic usage, ✗ for the edge cases around reconnection timing." This is the learner developing the calibration skill itself.

**AI Collaboration Reflection getting more strategic** = developing the meta-skill. Early: "I used Claude to write the code." Middle: "I used Claude for the boilerplate but wrote the state machine logic myself." Late: "I thought through the architecture in conversational mode, then handed the implementation plan to Claude Code as an agent task, then reviewed the output against my design." The progression from passive acceptance to strategic delegation is visible across devlogs.

---

## 7. Templates

Two standalone devlog templates are provided as tools — copy the file, fill it in, commit it:

- [[shu-devlog-template|Shu Devlog Template]] (framework/templates/shu-devlog-template.md) — Learning journal format. Understanding tiers, confidence calibrated for comprehension, AI collaboration basics.
- [[ha-devlog-template|Ha Devlog Template]] (framework/templates/ha-devlog-template.md) — Decision record format. The Decision section, What Was Removed, AI collaboration split into thinking/agent modes, confidence calibrated for judgment.
- **Ri devlog:** No separate template. The [[ri-ho-template|Ri Ho Template]] (framework/templates/ri-ho-template.md) document IS the reflection.

The ho templates ([[shu-ho-template|Shu Ho Template]] (framework/templates/shu-ho-template.md) and [[ha-ho-template|Ha Ho Template]] (framework/templates/ha-ho-template.md)) also contain embedded devlog sections with authoring notes explaining why each section exists and what to look for. The standalone templates are the tool; the embedded versions are the documentation.

---

## 8. Related Framework Documents

- [[ho-structure|Ho Structure]] (framework/structure/ho-structure.md) — §1.4 (Reflective) establishes the devlog as a ho invariant
- [[tiered-understanding|Tiered Understanding]] (framework/structure/tiered-understanding.md) — The tier model that devlogs report against
- [[shu-ha-ri|Shu-Ha-Ri Progression]] (framework/structure/shu-ha-ri.md) — How the reflection practice evolves across stages
- [[project-arc|The Project Arc]] (framework/structure/project-arc.md) — How devlogs create a readable learning trajectory across an arc

---

_This document is part of the Ho System framework. It describes the structural specification for the devlog as a reflection practice. For guidance on reading devlogs diagnostically — including calibration coaching, honesty development, and facilitation interventions when devlog quality declines — see the facilitation layer (proprietary)._
