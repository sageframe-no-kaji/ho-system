# Verification Practices

## How the Ho System Ensures Quality in AI-Augmented Development

---

## 1. Why Verification Is Foundational

The Ho System's central claim is that a human can maintain architectural authority over AI-generated implementation. That claim is hollow without verification. If you can't confirm that what the AI built matches what you specified, you don't have authority — you have optimism.

This is not a secondary concern. It is the structural mechanism that makes the entire methodology work.

The research bears this out. METR's 2025 randomized controlled trial found that developers were 19% slower with AI tools while believing they were faster. CodeRabbit's analysis of 4 million pull requests showed AI-generated code contains 1.7 times more quality issues. The consistent finding across studies is that humans overestimate the quality of AI-assisted work. Verification is the antidote to that overestimation.

The Ho System addresses this through a layered verification stack — not a single check, but a progression of increasingly rigorous practices that develop alongside the practitioner's capabilities. In shu stage, verification is built into the template. In ha stage, the practitioner develops their own verification habits. In ri stage, multi-agent verification becomes the primary quality mechanism.

This document defines the verification stack, explains when each layer applies, and establishes verification as a first-class practice within the methodology.

---

## 2. The Verification Stack

Verification in the Ho System operates at five layers. Each layer catches different classes of problems. Used together, they form a comprehensive quality practice that scales from beginner to advanced practitioner.

### Layer 1a: Test Coverage

**What it is:** Automated tests that verify the system's behavior against declared expectations.

**Why it's foundational:** Tests are the only verification mechanism that operates independently of the person doing the checking. A test suite doesn't have optimism bias. It doesn't get tired. It doesn't assume correctness because the code looks right. When an AI agent modifies your system, the test suite tells you — immediately, automatically, unambiguously — whether the modification broke something.

Without test coverage, the practitioner cannot verify AI-generated code at the only level that matters: behavior. You can read the code and think it looks right. You can ask the AI if it did what you asked and it will say yes. Only the tests tell you whether the system actually does what it's supposed to do.

**The practice:**

Test coverage in the Ho System is not aspirational. It is a non-negotiable structural element, established in the first ho of every project arc and maintained throughout.

The first ho that produces functional code must also produce tests for that code. This is not "write the feature, then add tests later." It is "the feature is not complete until it is tested." The completion checklist in every ho template includes "all tests pass" — but that understates the requirement. The real standard is: _tests exist that would fail if the feature were broken._

What to test and at what depth is an architectural decision — one that belongs to the practitioner, not the AI. The Kanyō pilot established a practical heuristic:

- **Always test:** State machines, event logic, data transformations, anything with conditional behavior. These are where silent bugs live.
- **Always test:** Integration points — where components connect. A unit that works in isolation can fail at the seam.
- **Test proportionally:** Configuration loading, utility functions, formatting logic. Test them if they're non-trivial; skip them if they're one-liners that would be obvious if broken.
- **Don't chase metrics:** 100% coverage is not the goal. Meaningful coverage is. 54% coverage with all critical paths tested is better than 90% coverage that misses the state machine.

**Across shu-ha-ri:**

| Stage | Test practice                                                                                                                                                                 | Who drives it                                                                             |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Shu   | Test framework set up in Ho 01. Each ho's completion checklist requires tests to pass. The ho author includes test structure in implementation steps.                         | The ho author designs the test structure; the learner implements it.                      |
| Ha    | The practitioner writes their own tests as part of Phase 2 (Execute). Test design becomes an architectural skill — what to test, how to test it, when coverage is sufficient. | The practitioner, with AI assistance for implementation.                                  |
| Ri    | Tests are written before or alongside implementation, often by the AI agent as part of the task spec. The practitioner reviews the tests for coverage gaps.                   | The practitioner specifies what to test; the agent implements tests and feature together. |

### Layer 1b: Linting and Static Analysis

**What it is:** Automated tools that verify code quality, type correctness, formatting consistency, and structural hygiene — independent of whether the code produces correct behavior.

**Why it's a separate layer:** Tests verify that the system _does the right thing._ Linting verifies that the code _is written correctly_ — consistent style, valid types, no dead imports, no unreachable branches, no formatting drift. These are different failure classes. Code can pass every test while containing type errors that will surface as bugs in untested paths, unused variables that signal incomplete refactoring, and formatting inconsistencies that make review harder.

AI-generated code is particularly prone to these issues. An agent producing 200 lines of implementation will frequently introduce inconsistent naming, miss type annotations, import modules it doesn't use, or drift from the project's established patterns. The code works. It passes tests. But it's subtly degraded — and the degradation compounds across sessions.

**The practice:**

The linting pipeline is established in the first ho of every project arc, alongside testing. The specific tools will vary by language and project, but the principle is constant: automated enforcement of quality standards that the human should not have to check manually.

For Python projects (the Ho System's primary implementation language as of this writing), a proven pipeline:

- **black** — Code formatting. Eliminates all style debates. The code looks one way.
- **isort** — Import organization. Consistent, alphabetical, grouped by source.
- **flake8** — Style and structural checking. Catches issues black doesn't — unused imports, undefined names, overly complex functions.
- **mypy** — Type checking. Catches type errors at analysis time rather than runtime. This is where real bugs hide.

The pipeline runs in a fixed order. Formatting first (black, isort), then analysis (flake8, mypy). Formatting tools change the code; analysis tools only report. Running them in the wrong order produces noise.

**The Code-Lint-Test Cycle:**

The discipline is not "code, then lint at the end." It is a cycle that runs continuously during development:

```
Code (implement the change)
  │
  ▼
Lint (black → isort → flake8 → mypy)
  │
  ├── Issues found ──▶ Fix ──▶ Lint again
  │
  Clean
  │
  ▼
Test (pytest with coverage)
  │
  ├── Failures ──▶ Fix ──▶ Test again ──▶ Lint again
  │
  All pass
  │
  ▼
Commit
```

The cycle closes with _both_ lint and test clean before commit. Not one or the other. Both. A commit that passes tests but fails linting introduces technical debt. A commit that passes linting but fails tests introduces bugs. Neither is acceptable.

This cycle applies to the practitioner's own code AND to AI-generated code. When an agent produces an implementation, the first thing the practitioner does — before reading it, before self-review, before anything — is run the lint-test cycle. If it fails, the agent fixes it. Code that doesn't pass the automated quality gate doesn't deserve human attention yet.

**Git through CLI, always.** Git operations are executed through the command line, never through GUI tools (GitKraken, Sourcetree, VSCode's git panel). This is not snobbery — it's verification infrastructure. CLI git forces the practitioner to see exactly what is being staged, committed, and pushed. GUI tools abstract those operations into clicks that obscure what's actually happening. In shu stage, this builds genuine understanding of version control. In ri stage, it maintains visibility over what agents are committing. If you can't read the git output, you can't verify the git operation.

**The Kanyō evidence:**

The Kanyō pilot established this pipeline in Ho 01 and maintained it throughout. The first mypy run produced 21 errors — mostly configuration issues and missing type stubs, but one real bug (a missing value in a Literal type union). That bug would have caused a runtime error in a specific, infrequently-triggered code path. Tests might not have caught it for weeks. Mypy caught it in seconds.

By Ho 06, the linting pipeline was habitual — the practitioner ran it reflexively after every coding session, and the devlogs stopped mentioning it because it had become invisible infrastructure. That's the goal: linting as breathing, not as ceremony.

**Across shu-ha-ri:**

| Stage | Linting practice                                                                                                                                                                                                              | Who drives it                                             |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| Shu   | Pipeline established in Ho 01. The ho template includes lint commands in verify steps. The completion checklist requires "quality tools pass." The learner builds the habit through repetition.                               | The ho author specifies the tools and commands.           |
| Ha    | The practitioner runs the pipeline automatically. They may add project-specific linting rules (custom flake8 plugins, stricter mypy settings) as the codebase matures.                                                        | The practitioner, as part of their development rhythm.    |
| Ri    | The pipeline is part of every agent task's implicit contract. Agent-produced code that fails linting is rejected before review. The practitioner may integrate linting into pre-commit hooks or CI for automated enforcement. | Automated, with practitioner oversight of rule evolution. |

---

### Layer 2: Recursive Self-Review

**What it is:** After the AI agent completes a task, instructing it to review its own output against the original specification before the human reviews it.

**Why it matters:** This is the cheapest, fastest quality gate available. It costs one additional prompt. It catches a surprising number of issues — missed requirements, incomplete implementations, inconsistencies between what was specified and what was built, and outright errors that the agent introduced but didn't notice during generation.

The mechanism is simple but the principle is important: you are establishing that _completion is not the same as correctness._ The agent's first output is a draft. The review turns it into a verified submission. This mirrors the human practice of re-reading your own work before handing it in — except that AI agents will skip this step unless explicitly directed.

**The practice:**

After any non-trivial agent task, before accepting the output:

1. Direct the agent to re-read the original task specification
2. Direct it to compare its implementation against every stated requirement
3. Direct it to identify anything it missed, any requirement it partially fulfilled, or any deviation from the spec
4. Direct it to fix issues before presenting the final output

The instruction can be as simple as: _"Before I review this — re-read the task spec and check your work against every requirement. Flag anything you missed or handled differently than specified."_

**Watch the agent work.** CLI output and agent thinking should be monitored in real time, not reviewed after the fact. When an agent executes a task, the practitioner watches the terminal — seeing which files are being modified, which commands are being run, which decisions are being made in the moment. This is not micromanagement. It is situational awareness. A practitioner who walks away and comes back to "done" has lost the ability to catch wrong turns early. Monitoring is cheapest verification there is — it costs only attention.

This is not the same as asking "did you do it right?" (which always gets "yes"). It is directing a specific re-evaluation process against a specific document. The specificity matters. An agent asked to "check your work" will produce a generic affirmation. An agent asked to "compare your implementation against requirements 1-7 in the task spec and report status on each" will produce a useful audit.

**Across shu-ha-ri:**

| Stage | Self-review practice                                                                                                                                                                                   | How it manifests                              |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------- |
| Shu   | The ho template's verify step after each part serves this function. The learner runs commands and compares output against expected results. The ho author has pre-specified what "correct" looks like. | Built into the template structure.            |
| Ha    | The practitioner develops the habit of asking for self-review on any delegated work. The ha template's Agent Task Log tracks what was delegated and what was reviewed.                                 | Practitioner-initiated after each delegation. |
| Ri    | Self-review is a standard part of every agent task. The practitioner includes it in their workflow automatically — it's as natural as running the tests.                                               | Part of the agent task execution protocol.    |

### Layer 3: Cross-Agent Verification

**What it is:** Using a different AI model (typically a more capable one) to evaluate the output of the coding agent, specifically checking whether the task was implemented correctly and completely.

**Why it matters:** This addresses a fundamental limitation of self-review: the same model that made an error may not catch it on re-examination. Its blind spots are consistent. A different model brings different strengths — particularly a model optimized for reasoning and evaluation rather than code generation.

The practical pattern that has proven effective: use a fast, competent model for agentic coding (task execution, file creation, implementation work), then use a more powerful reasoning model for evaluation (checking correctness, identifying architectural issues, assessing whether requirements were truly met vs. superficially addressed).

This is the AI-age equivalent of code review. Just as a human code reviewer catches things the author missed — not because the reviewer is smarter, but because they bring fresh eyes and different assumptions — a cross-agent review catches things the implementing agent missed.

**The practice:**

After the implementing agent completes a task and passes self-review:

1. Provide the evaluation agent with: the original task specification, the implementation (code diffs, new files, or a summary of changes), and the test results
2. Ask the evaluation agent to assess: Does the implementation satisfy every requirement in the spec? Are there edge cases not covered? Are there architectural concerns — patterns that work today but will cause problems as the system grows? Did the implementing agent actually do what was asked, or did it do something adjacent that looks similar?
3. Address any findings before accepting the work

The evaluation prompt might look like:

_"Here is a task specification and the implementation produced by a coding agent. Review the implementation against the spec. For each requirement, confirm it was met or flag the gap. Identify any edge cases, architectural concerns, or areas where the implementation is superficially correct but misses the intent."_

**Across shu-ha-ri:**

| Stage | Cross-agent practice                                                                                                                                                                                | When to use it                                                                    |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| Shu   | Rarely applicable — the learner is implementing with guidance, not delegating to agents. The ho structure itself serves the verification function.                                                  | Not typical for shu stage.                                                        |
| Ha    | Emerging practice. The practitioner begins using evaluation models to review significant delegated work, particularly when the ha session involves architectural decisions that need validation.    | For significant implementation work within Phase 2 (Execute).                     |
| Ri    | Standard practice for all non-trivial agent tasks. The practitioner's workflow becomes: specify → implement (coding agent) → self-review → cross-verify (evaluation agent) → human review → accept. | Every task that touches critical paths or involves more than ~50 lines of change. |

**Model selection guidance:**

The specific models will change over time, but the principle is stable: separate the execution model from the evaluation model, and choose the evaluation model for reasoning depth rather than speed.

As of early 2026, an effective pairing is:

- **Execution:** Claude Sonnet (fast, competent, follows specs well, good for agentic coding)
- **Evaluation:** Claude Opus (deeper reasoning, better at catching subtle issues, better at assessing architectural fit)

The evaluation model doesn't need to be "better" at coding. It needs to be better at _thinking about_ code — identifying whether an implementation truly satisfies intent, not just whether it compiles and passes tests.

### Layer 4: Human Architectural Review

**What it is:** The practitioner's own review of the verified implementation, focused on architectural fit rather than line-by-line correctness.

**Why it matters:** The first three layers verify that the implementation is correct. This layer verifies that it is _right_ — that it fits the system's architecture, serves the project's goals, and doesn't introduce patterns that will cause problems later. This is the review that only the architect can do, because only the architect holds the full mental model of the system.

**The practice:**

After the implementation has passed tests, self-review, and cross-agent verification, the practitioner reviews with these questions:

- Does this fit how the system is supposed to work, or does it introduce a different pattern?
- Will this be easy or hard to extend later?
- Does this create dependencies I didn't intend?
- Is this the simplest solution that actually solves the problem?
- Would I be comfortable explaining this to someone else?

This review is not about finding bugs — the previous layers should have caught those. It's about ensuring that each piece of work contributes to a coherent whole rather than producing a patchwork of individually correct but architecturally inconsistent pieces.

**Across shu-ha-ri:**

| Stage | Human review focus                                                                                                                                                                               |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Shu   | "Does this match what the ho described?" — the learner is verifying against the author's design. Understanding verification questions test whether the learner can reason about what they built. |
| Ha    | "Does this match what I designed?" — the practitioner is verifying against their own Phase 1 decisions. The devlog's decision record captures the reasoning.                                     |
| Ri    | "Does this fit the system?" — the practitioner is verifying architectural coherence, not correctness. Brief, focused, habitual.                                                                  |

---

## 3. The Full Verification Workflow

In mature practice (ha/ri stage), the complete verification workflow for a non-trivial task looks like this:

```
Task Specification
       │
       ▼
Implementation (coding agent)
       │
       ▼
Lint (black → isort → flake8 → mypy) ── FAIL ──▶ Fix and re-lint
       │
      CLEAN
       │
       ▼
Automated Tests ── FAIL ──▶ Fix ──▶ Re-test ──▶ Re-lint
       │
      PASS
       │
       ▼
Recursive Self-Review ── Issues found ──▶ Fix, re-lint, re-test
       │
    Clean
       │
       ▼
Cross-Agent Verification ── Concerns raised ──▶ Address, re-lint, re-test
       │
    Clean
       │
       ▼
Human Architectural Review ── Doesn't fit ──▶ Rethink approach
       │
    Accepted
       │
       ▼
Commit (lint + tests confirmed clean)
```

Not every task needs every layer. The practitioner develops judgment about which layers to apply based on risk:

| Task type                                            | Layers applied                           |
| ---------------------------------------------------- | ---------------------------------------- |
| Trivial fix (typo, config change)                    | Lint + tests                             |
| Small feature (new endpoint, UI element)             | Lint + tests + self-review               |
| Significant feature (new module, state change)       | Lint + tests + self-review + cross-agent |
| Critical path change (state machine, data integrity) | All layers                               |

Note that lint and tests are _always_ applied. They are the floor, not a layer you choose to skip. The judgment is about which _human and AI review_ layers to add above that floor.

---

## 4. Verification as a Developing Skill

Verification is not just a practice — it is a skill that develops through the shu-ha-ri progression, just like any other aspect of the methodology.

### In Shu: Verification Is Built In

The shu-stage practitioner doesn't need to think about verification because the ho template does it for them. Every part has a verify step. The completion checklist is binary — pass or fail. The linting pipeline runs after every implementation step. The verification questions test understanding. The commit discipline creates recovery points.

The learner is building the _habit_ of the code-lint-test cycle without bearing the _design burden_ of deciding what to verify and how. By the end of shu stage, running `black → isort → flake8 → mypy → pytest` should feel as natural as saving a file. This is the scaffolding in action.

What the shu-stage practitioner should internalize:

- Nothing is complete until it's verified — both lint-clean AND test-passing
- "It looks right" is not verification — running the tools is verification
- Linting errors are not cosmetic — mypy catches real bugs, flake8 catches structural problems
- Verification questions are for you, not for anyone else — be honest

### In Ha: Verification Becomes Judgment

The ha-stage practitioner is making their own decisions about what to verify and how rigorously. The template provides prompts (the Agent Task Log, the self-assessment) but the practitioner designs their own verification approach.

This is where the skill develops: knowing when self-review is sufficient and when cross-agent verification is necessary. Knowing which tests to write for a new feature and which edge cases matter. Knowing the difference between "this passes the tests" and "this is correct."

What the ha-stage practitioner should develop:

- The instinct to write tests _before_ accepting AI-generated code
- Comfort with directing agent self-review as a standard practice
- Beginning cross-agent verification for significant work
- The ability to articulate _why_ they trust (or don't trust) an implementation

### In Ri: Verification Is Workflow

The ri-stage practitioner has internalized verification as the way work gets done. The four-layer stack is not a checklist they consult — it's the natural rhythm of their process. They specify a task, the agent implements it, they run the verification layers appropriate to the task's risk level, and they commit.

The danger at ri stage is _efficiency erosion_ — skipping verification because "it's a small change" or "I'm confident the agent got it right." This is the exact cognitive trap the research documents: overconfidence in AI-assisted output. The ri-stage practitioner must maintain the discipline even when the task feels routine.

What the ri-stage practitioner should maintain:

- The code-lint-test cycle runs on every change, no exceptions — this is the floor, not a choice
- Self-review on anything non-trivial
- Cross-agent verification on anything touching critical paths
- Periodic full-stack verification even on "simple" tasks — to keep the habit alive
- Pre-commit hooks or CI enforcement to make the floor truly non-negotiable

---

## 5. Verification and the Devlog

Verification practices should be reflected in devlog entries. Not as a compliance exercise, but because the verification record is itself valuable documentation.

### What to Capture

In the devlog's "What Was Built" or "AI Collaboration Reflection" section:

- **What verification was applied:** "Full lint pipeline clean (black, isort, flake8, mypy), test suite passing (114 tests), cross-checked the state machine changes with Opus, reviewed architectural fit myself"
- **What verification caught:** "mypy flagged a type mismatch in the new timeout parameter — was passing string instead of int. Self-review caught a missing edge case in the timeout logic. Cross-agent review flagged that the new state wasn't being tested. All issues fixed before commit."
- **What was NOT verified and why:** "Skipped cross-agent verification on the CSS changes — low risk, no behavioral impact." (This is honest documentation, not a confession.)

### Why This Matters

The verification record serves three purposes:

1. **Quality evidence.** When someone reviews the project (employer, client, collaborator), the devlog shows not just what was built but that it was _verified_. This is the "I built this with judgment" signal that distinguishes Ho System work from vibe coding output.

2. **Pattern recognition.** Over time, the practitioner's verification records reveal patterns: which types of tasks produce issues at which verification layers, where their own review catches things the agents miss, and where they've been under-verifying.

3. **Methodology evidence.** For the Ho System itself, verification records across multiple practitioners and projects demonstrate that the methodology produces verifiably correct systems. This is data, not claims.

---

## 6. Verification in Kinhin

Kinhin, the Ho System workbench, should support verification practices as structured data rather than prose in devlogs. Specific features to consider:

- **Verification method tracking per ho:** Which layers were applied (tests, self-review, cross-agent, human review), what was caught, what was accepted
- **Agent task verification fields:** Each agent task record includes verification method used and findings
- **Verification trends in the trajectory dashboard:** Over time, does the practitioner's verification practice deepen? Do they apply appropriate rigor to appropriate risk levels?
- **Export format for verification records:** Structured data that can be presented as evidence of quality practice — useful for portfolios, client engagements, and methodology evaluation

These are post-MVP features, but the data model should anticipate them. A verification_method field on devlog entries and agent task records costs nothing at schema design time and enables this entire layer later.

---

## 7. Principles

Five principles govern verification practice in the Ho System:

**1. Tests and linting are structural, not optional.** Test coverage verifies behavior. Linting verifies code quality. Together, they are how you maintain architectural authority over AI-generated code. Without them, you don't have verification — you have hope. Establish both in Ho 01 and maintain them throughout. The code-lint-test cycle (code → lint → test → fix → re-test → re-lint → commit) is the heartbeat of every development session.

**2. Completion is not correctness.** The agent saying "done" is the beginning of verification, not the end. Self-review, cross-agent verification, and human review each catch different classes of problems.

**3. Verification scales to risk.** Not every task needs every layer. But the default should be more, not less. A two-minute cross-agent check is always cheaper than a two-hour production debugging session.

**4. The verification skill develops.** In shu, it's built into the template. In ha, it becomes judgment. In ri, it's workflow. The progression is from scaffolded habit to internalized practice.

**5. Document what you verified.** The verification record is evidence — of quality, of judgment, of the methodology working. Capture it in devlogs, not because someone requires it, but because future-you will be glad you did.

---

_This document is part of the Ho System framework. It should be read alongside [[ho-structure|Ho Structure]](ho-structure.md) for the unit of work definition, [[shu-ha-ri|Shu-Ha-Ri Progression]](shu-ha-ri.md) for how practices adapt across stages, and [[agent-task-spec|Agent Task Specification]](../templates/agent-task-spec.md) for the delegation format that verification practices operate on._

_The verification practices described here were developed through the Kanyō pilot and refined through continued practice. They represent tested patterns, not theoretical recommendations._
