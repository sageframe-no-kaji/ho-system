# Shu-Stage Ho Template

## Template for Prescriptive Learning Sessions

**Stage:** Shu (守) — Follow the Form **Use when:** The learner is new to the domain, tool, or technique being introduced. **See:** [Shu-Ha-Ri Progression Model](https://claude.ai/structure/shu-ha-ri.md) for when to use this template vs. others.

---

## How to Use This Template

This template has two layers:

1. **The ho structure** — sections marked with `#` headers that appear in the finished ho document.
2. **Authoring notes** — blocks marked with `> 📐 AUTHOR` that explain _why_ a section exists, what's cross-project (fixed) vs. project-specific (adapt), and how AI should handle it.

When creating a ho from this template:

- Keep all `#` header sections (they are structurally required)
- Read the authoring notes, then delete them from the finished document
- Adapt project-specific content to the actual project
- Cross-project elements should be preserved as-is unless you have a strong reason to change them

### Pre-Ho Documents

Before writing individual hos, the project should have these documents (in order of creation):

1. **Seed** — Raw ideation. What's the idea? What problem does it solve? What's exciting about it? This is brainstorming output — messy is fine. _(Example: The Hōzō concept document.)_
2. **Architecture Overview** — High-level system design. What are the major components? How do they connect? What's the technology stack? What's the deployment model? This is the document you'd show someone to explain _what you're building_. _(Example: The Kanyō system architecture.)_
3. **README** — The polished project scope. Written as if the project already exists. What does it do, how do you install it, how do you use it. This forces clarity about what "done" looks like. _(Example: The Hōzō README.md.)_
4. **Ho Overview** — The sequence plan. What hos are needed, in what order, with what dependencies? This is the project arc made concrete. Feed the seed, architecture overview, and README to the AI and ask it to generate a ho sequence.

This chain is defined in [Kamae: Project Framing](../structure/kamae-project-framing.md).

Individual hos are then written from the Ho Overview, with this template as the structural guide.

---

## Template Begins Here

> _Everything below this line is the ho document structure. Authoring notes are inline._

---

```markdown
# Ho [N]: [Title]

## [Subtitle — one sentence describing what this ho accomplishes]
```

> 📐 **AUTHOR — Title & Subtitle** _Cross-project structure. Project-specific content._
>
> The title should be memorable and specific. "Git Good" is better than "Git Setup." The subtitle is the elevator pitch for the session — if the learner reads nothing else, this sentence should tell them what they're building.
>
> Numbering convention:
>
> - Whole numbers (1, 2, 3) for planned arc hos
> - Decimal numbers (5.5, 5.6) for hos that emerge mid-project
> - Sub-decimals (5.71, 5.72) for follow-up fixes and refinements (typically ri-stage)

---

```markdown
**Duration:** ~2 hours
**Goal:** [Single clear sentence — what is TRUE at the end that wasn't true at the start]
**Deliverable:** [Concrete artifact — what EXISTS at the end that didn't exist before]
```

> 📐 **AUTHOR — Duration / Goal / Deliverable** _Cross-project structure. Cross-project constraints._
>
> **Duration** is ~2 hours for all shu-stage hos. This is not negotiable. If the content doesn't fit in 2 hours, split it into multiple hos. The boundary protects learners who don't yet know when they're in a rabbit hole.
>
> **Goal** is a state change, not an activity. "Understand Docker" is a bad goal. "Have a running Docker container serving the application on the deployment machine" is a good goal. It's testable.
>
> **Deliverable** is a _thing_, not knowledge. Working code, passing tests, a deployed service, a configured tool. The deliverable is the evidence that the goal was met.

---

```markdown
---

## Why This Ho Matters

**Before this ho:** [What the learner can do / what exists now]

**After this ho:** [What the learner can do / what exists after]

[1-2 paragraphs of systemic context. Where does this ho fit in the project arc?
Why does this come NOW rather than earlier or later? What does it enable that
wasn't possible before?]
```

> 📐 **AUTHOR — Why This Ho Matters** _Cross-project structure. Project-specific content._
>
> The before/after framing is important. It makes the value of the session concrete and gives the learner a way to evaluate whether they succeeded.
>
> This section should connect to the project arc — the learner should understand that this isn't an isolated exercise but a step in building something real. Reference previous hos and preview upcoming ones where relevant.

---

````markdown
---

## Prerequisites

### Required Before Starting

- [ ] [Specific verifiable state — e.g., "Ho 1 completed and all tests passing"]
- [ ] [Specific tool installed — e.g., "Docker Desktop running, verified with `docker --version`"]
- [ ] [Specific artifact exists — e.g., "config.yaml created with at least one job definition"]

### Verify Your Environment

Run these commands and confirm the expected output:

\```bash
[command]

# Expected: [output]

[command]

# Expected: [output]

\```

If any check fails, stop and resolve it before continuing. Building on an
unstable foundation creates compounding problems.
````

> 📐 **AUTHOR — Prerequisites** _Cross-project structure. Project-specific content._
>
> Prerequisites must be _verifiable_, not vague. "Comfortable with git" is not a prerequisite. "Can run `git log --oneline` and see at least 3 commits from Ho 0" is a prerequisite.
>
> Include actual commands to run with expected outputs. The learner should be able to confirm readiness in under 2 minutes. If they can't, they're not ready.
>
> The "stop and resolve" instruction is deliberate — shu-stage learners often push through broken prerequisites and then hit cascading failures 45 minutes later.

---

```markdown
---

## Understanding Tiers for This Ho

Before starting, know what depth of understanding to target for each concept:

**Tier 1 — Black Box** (use it, don't investigate it):

- [Component]: [What you need to know to use it, nothing more]
- [Component]: [What you need to know to use it, nothing more]

**Tier 2 — Functional Understanding** (understand enough to configure, modify, and explain):

- [Component]: [What understanding looks like at this level]
- [Component]: [What understanding looks like at this level]

**Tier 3 — Deep Understanding** (architectural level, if applicable):

- [Component]: [What deep understanding means here, if relevant to this ho]
```

> 📐 **AUTHOR — Understanding Tiers** _Cross-project structure. Project-specific content._
>
> This section is doing critical cognitive work: giving the learner _permission_ to not understand everything. Without explicit tier assignments, beginners either try to understand everything (overwhelm) or understand nothing (passivity).
>
> Most components in a shu-stage ho should be Tier 1 or Tier 2. If you're assigning Tier 3 to multiple components, the ho is probably too ambitious for shu stage.
>
> The tiered understanding model is documented in detail at [tiered-understanding.md](https://claude.ai/structure/tiered-understanding.md).

---

```markdown
---

## AI Collaboration Guide

This ho uses AI as a **thinking partner and implementation assistant**. Here's how
to work with it effectively during this session.

### Mode: Thinking Partner (Claude / Chat AI)

Use conversational AI for:

- Understanding concepts before implementing them
- Reviewing code before accepting it
- Asking "why" questions when something isn't clear
- Discussing alternatives when you're unsure about an approach

**How to prompt well in this ho:**

- "[Specific prompting guidance for this ho's content]"
- "Before we write code, explain [concept] to me like I understand [adjacent concept they know]"
- "I'm about to [action]. What could go wrong?"
- "Review this code and tell me what each section does before I run it"

### Mode: Implementation Agent (Claude Code / Copilot / Agent Tooling)

Use agent-mode AI for:

- Generating boilerplate code after you understand the pattern
- Running linting and formatting
- Executing repetitive file operations
- Creating test scaffolding

**Ground rules for this ho:**

- Do NOT let the agent write code you haven't discussed first
- Review every file the agent creates or modifies
- If the agent produces something you don't understand, STOP and switch to thinking mode
- Commit only after you can explain what changed and why

### The Verification Habit

After every AI interaction that produces code:

1. Read it. (Not skim — read.)
2. Can you explain what it does to someone else? If no → ask the AI to explain.
3. Can you predict what will happen when you run it? If no → ask before running.
4. Run it. Did it match your prediction? If no → understand the difference.
```

> 📐 **AUTHOR — AI Collaboration Guide** _Cross-project structure with project-specific prompting examples._
>
> This section is NEW relative to the Kanyō pilot — it didn't exist there and its absence was felt. The thinking/agent distinction is the most important structural addition to the shu template.
>
> The "Ground rules" are deliberately strict for shu stage. In ha stage, the learner has earned enough judgment to relax them. In ri stage, they're unnecessary.
>
> The prompting guidance should be specific to this ho's domain. Generic prompting advice ("be specific") is less useful than "before implementing the SSH connection, ask the AI to explain the difference between paramiko and subprocess SSH."
>
> The Verification Habit (read → explain → predict → run → compare) is the core learning loop. It's what separates "building with understanding" from "vibe coding."

---

````markdown
---

## Part 1: [Title] (~[X] minutes)

### What We're Doing

[1-2 sentences: what this part accomplishes and why it comes first]

### Key Concept

[Brief explanation of the core idea the learner needs to understand.
Not a textbook explanation — enough to make the implementation meaningful.]

### Implementation

[Step-by-step instructions. Be explicit about:

- What to type / what commands to run
- What output to expect
- What to do if the output doesn't match]

\```bash

# [Explanation of what this command does]

[command]

# Expected output:

[expected output]
\```

### Verify

[How to confirm this part worked. Specific commands with expected outputs.]

\```bash
[verification command]

# You should see: [expected result]

\```

### Commit

\```bash
git add [specific files]
git commit -m "[conventional commit message]"
\```

> **Understanding check:** [One question the learner should be able to answer
>
> > before moving on. Not "did it work?" but "why did we do it this way?"]
````

> 📐 **AUTHOR — Parts** _Cross-project structure. Project-specific content._
>
> A shu-stage ho has 4–9 parts, each 10–25 minutes. Parts are the heartbeat of the session — they provide rhythm, checkpoints, and recovery points.
>
> **Each part follows the same internal structure:**
>
> 1. Context (what and why)
> 2. Key concept (just enough theory to make the work meaningful)
> 3. Implementation (explicit steps)
> 4. Verification (confirm it worked)
> 5. Commit (save the checkpoint)
> 6. Understanding check (confirm the learner is tracking)
>
> **Cross-project elements** (always present):
>
> - Verify step after every part
> - Commit after every part
> - Understanding check at the end of every part
>
> **Project-specific elements** (author adapts):
>
> - The actual content, commands, code
> - The key concept explanation
> - The understanding check question
>
> **Common authoring mistakes:**
>
> - Parts that are too long (>25 min) — split them
> - Parts that don't build on each other — resequence
> - Parts with no verification — the learner can't tell if they succeeded
> - Verification that's only "did it run?" — also check understanding
>
> **The commit discipline is non-negotiable in shu stage.** Every part gets a commit. This builds the habit AND creates recovery points. If Part 4 breaks something, the learner can revert to Part 3's commit.

---

> 📐 **AUTHOR — Repeat Part structure for Parts 2 through N**
>
> Each subsequent part follows the same template. The last part should be the one that produces the ho's stated deliverable. Don't put the deliverable in the middle and then do cleanup parts after — end on the achievement.

---

```markdown
---

## Completion Checklist

Confirm each item before marking this ho complete:

- [ ] [Deliverable exists and works — specific test]
- [ ] [Key artifact 1 created/modified]
- [ ] [Key artifact 2 created/modified]
- [ ] [All tests pass: `[test command]`]
- [ ] [Quality tools pass: `[quality command]`]
- [ ] [Specific functional test — e.g., "Run `hozo --help` and see usage output"]
- [ ] [Git log shows clean commits for each part]
- [ ] Devlog entry completed (see below)
```

> 📐 **AUTHOR — Completion Checklist** _Cross-project structure. Project-specific criteria._
>
> The checklist is binary: done or not done. No partial credit. If the learner can check every box, the ho succeeded. If they can't, something needs fixing before moving on.
>
> Always include:
>
> - The deliverable test (cross-project)
> - Tests passing (cross-project)
> - Quality tools passing (cross-project)
> - Clean git history (cross-project)
> - Devlog entry (cross-project)
> - Project-specific functional verification (adapt per ho)

---

```markdown
---

## Understanding Verification

You should be able to answer these questions. If you can't, revisit the
relevant part before moving on.

### Tier 2 Questions (you should be able to explain these):

1. [Question about WHY a design decision was made, not just WHAT was done]
2. [Question that tests whether the learner can predict behavior in a new scenario]
3. [Question about how components connect to each other]

### Tier 1 Acknowledgments (confirm you know these are black boxes):

1. [Component] — I use this by [how], I don't need to understand [what's inside]
2. [Component] — I use this by [how], I don't need to understand [what's inside]
```

> 📐 **AUTHOR — Understanding Verification** _Cross-project structure. Project-specific questions._
>
> These are NOT quizzes. They are self-assessment tools. The learner checks their own understanding honestly. Nobody else sees the answers.
>
> **Good verification questions:**
>
> - "Why did we use a ring buffer instead of writing to disk continuously?" (Tests architectural reasoning)
> - "If the SSH timeout is set to 120 seconds but the machine takes 180 to boot, what happens?" (Tests ability to predict behavior)
> - "What would you change if you needed to back up 3 machines instead of 1?" (Tests ability to extend)
>
> **Bad verification questions:**
>
> - "What command creates a virtual environment?" (Tests memory, not understanding)
> - "Did the tests pass?" (Tests completion, not comprehension)
>
> The Tier 1 acknowledgments are equally important — they validate that the learner has made _conscious decisions_ about what NOT to understand, rather than simply not understanding things they should.

---

````markdown
---

## Common Issues & Solutions

### [Problem 1: Descriptive title]

**Symptoms:** [What the learner sees]
**Cause:** [Why it happens]
**Fix:**
\```bash
[specific fix commands]
\```

### [Problem 2: Descriptive title]

**Symptoms:** [What the learner sees]
**Cause:** [Why it happens]
**Fix:**
\```bash
[specific fix commands]
\```
````

> 📐 **AUTHOR — Common Issues** _Cross-project structure. Project-specific content._
>
> This section is built from experience — you fill it in after the ho has been run at least once. For first drafts, include issues you _anticipate_ based on the technology stack. After testing, add the issues that _actually occurred_.
>
> The format (symptoms → cause → fix) is deliberate. It teaches diagnostic thinking, not just "try this command."
>
> Some common cross-project issues worth including:
>
> - Virtual environment not activated (symptoms: wrong Python, missing packages)
> - Git conflicts from uncommitted changes
> - Port already in use
> - Permission errors on file operations

---

````markdown
---

## Devlog Entry

Create a new file: `devlog/ho-[N]-[slug].md`

Use this structure:

\```markdown

# Ho [N]: [Title] — Devlog

**Date:** [today's date]
**Duration:** [actual time spent, honestly]
**Status:** Complete / Partial / Blocked

## What Was Built

[List the concrete artifacts produced — files, configs, running services]

## Understanding Tiers Achieved

**Tier 1 (Black Box):**

- [What I used without investigating — and I'm okay with that]

**Tier 2 (Functional):**

- [What I now understand well enough to modify and explain] ✓/✗

**Tier 3 (Deep):**

- [What I understand at an architectural level, if anything]

## Challenges Encountered

[What went wrong, what took longer than expected, what confused you]

## Key Learnings

[What you know now that you didn't know 2 hours ago.
Not "I learned how to use pytest" but "I learned that pytest discovers
tests by filename convention, which means naming matters."]

## AI Collaboration Reflection

[How did you use AI in this session? What worked? Where did you need to
slow down and think instead of accepting AI output? Did you catch any
AI mistakes?]

## Files Created/Modified

[Complete list — this becomes useful reference later]

## Confidence Level: [1–5]

1 = I followed instructions but don't really understand what happened
2 = I understand the broad strokes but couldn't reproduce without guidance
3 = I could explain this to someone and troubleshoot basic issues
4 = I understand this well and could extend it independently
5 = I could teach this and make non-obvious design decisions

## What I'd Do Differently

[Hindsight reflection — optional but valuable]
\```
````

> 📐 **AUTHOR — Devlog** _Cross-project structure. Cross-project template._
>
> The devlog is the most important learning artifact in the Ho System. It's where understanding gets externalized, tested, and preserved. It's also the primary evidence that learning happened — not the code, not the passing tests, but the learner's own honest account of what they understood and what they didn't.
>
> **The AI Collaboration Reflection section is new.** It didn't exist in the Kanyō pilot and it should have. Reflecting on _how_ you used AI — not just what you built with it — develops the meta-skill of effective AI collaboration.
>
> **The confidence scale is calibrated to be useful, not flattering.** Most shu-stage learners will honestly be at 2–3 for their first few hos. A learner who reports 5 on their second ho is either exceptional or not being honest. Both cases are worth paying attention to.
>
> Devlog entries should be written _immediately_ after completing the ho, while the experience is fresh. Not the next day. Not "later."

---

```markdown
---

## What's Next

**Next ho:** [Ho N+1: Title]

[1-2 sentences previewing what the next ho builds, and how it depends on
what was just completed. This creates forward momentum and helps the learner
see the arc.]

**What to do before next session:**

- [ ] [Any between-session tasks — reading, environment prep, etc.]
- [ ] [Or simply: "Nothing — you're ready to start Ho N+1"]
```

> 📐 **AUTHOR — What's Next** _Cross-project structure. Project-specific content._
>
> This section serves two purposes: it maintains motivation (there's a clear next step) and it creates dependency links (the next ho expects this ho's outputs to exist).
>
> Keep between-session tasks minimal. The work happens in hos, not between them.

---

## Template Metadata

```markdown
---

_Ho authored by: [name or "AI-generated from Ho Overview"]_
_Template version: shu-v1 (draft)_
_Project: [project name]_
_Position in arc: Ho [N] of ~[total] planned_
_Pre-ho documents: [Seed] → [Architecture Overview] → [README] → [Ho Overview]_
```

> 📐 **AUTHOR — Metadata** _Cross-project structure._
>
> The metadata serves the framework, not the learner. It tracks provenance (who wrote this ho, from what source material) and position (where in the arc). This matters when reviewing and revising ho sequences across projects.
>
> The pre-ho document chain (Seed → Architecture Overview → README → Ho Overview) should be referenced so that anyone reading the ho can trace back to the design decisions that shaped it.

---

## Structural Constraints (Cross-Project, Non-Negotiable)

These constraints apply to ALL shu-stage hos regardless of project:

1. **~2 hour duration.** If it doesn't fit, split it. No exceptions.
2. **4–9 parts.** Each 10–25 minutes. Each with verify + commit.
3. **Single clear deliverable.** One testable artifact, not a collection of half-finished things.
4. **Prerequisites are verifiable.** Commands with expected output, not vibes.
5. **Understanding tiers declared up front.** The learner knows what depth to target before starting.
6. **AI collaboration guidance included.** Thinking mode vs. agent mode, explicit for this ho's content.
7. **Commit after every part.** Non-negotiable in shu stage.
8. **Devlog entry required.** Written immediately after completion.
9. **Verification questions test understanding, not completion.** "Why" not "did."
10. **Common issues section.** Populated after first run, anticipated before.

---

## Checklist for Ho Authors

Before publishing a shu-stage ho, verify:

- [ ] Goal is a testable state change, not an activity
- [ ] Deliverable is a concrete artifact, not knowledge
- [ ] Every part has: context → concept → implementation → verify → commit → understanding check
- [ ] Prerequisites include actual commands with expected outputs
- [ ] Understanding tiers are declared for all significant components
- [ ] AI collaboration guide has project-specific prompting examples
- [ ] Verification questions test reasoning, not recall
- [ ] Completion checklist is binary (pass/fail, no partial credit)
- [ ] The ho is completable in ~2 hours (have you timed it or estimated honestly?)
- [ ] Devlog template is included with the AI Collaboration Reflection section
- [ ] What's Next section connects to the project arc
- [ ] Metadata references pre-ho documents

---

_This template is part of the Ho System framework._ _For ha-stage work, see [ha-ho-template.md](https://claude.ai/chat/ha-ho-template.md)._ _For ri-stage work, see [ri-ho-template.md](https://claude.ai/chat/ri-ho-template.md)._ _For template selection guidance, see the [Template Selection Guide](https://claude.ai/chat/README.md)._
