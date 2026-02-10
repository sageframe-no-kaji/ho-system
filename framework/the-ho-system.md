# The Ho System (歩)

## A Collaborative Development Methodology for Building With AI

---

## What This Is

The Ho System is a structured methodology for building production-quality software using AI as an implementation partner. It's designed for people who think in systems — who can architect solutions, evaluate tradeoffs, and make design decisions — but who haven't followed a traditional developer path.

The core insight: the bottleneck in AI-assisted development isn't access to tools. Anyone can prompt an AI to generate code. The bottleneck is *judgment* — knowing what to build, whether what was built is correct, and when the AI is wrong. The Ho System develops that judgment through structured practice on real projects.

**Ho** (歩) means "step" in Japanese. Each unit of work is a deliberate, bounded step forward — not a sprint, not a leap.

---

## How It Works

### The Arc

Every project follows an arc: a sequence of sessions (hos) that takes you from initial setup to a working system.

```
Kamae (構え)          The Ho Sequence               Operations
─────────────    ──────────────────────────    ─────────────
Frame the         Build it, session by           Maintain and
project           session, with structure        extend
                  that adapts as you grow
```

**[[kamae-project-framing|Kamae]]** (framework/structure/kamae-project-framing.md) is the planning phase. You produce four documents — concept, system design, README, and ho overview — that turn a raw idea into a buildable plan.

**The ho sequence** is the build. Each ho is a bounded session (~2 hours in early stages, flexible later) that produces a real deliverable — working code, passing tests, a deployed service. Hos are numbered, committed to version control, and followed by structured reflection.

**Operations** is where the practitioner maintains the running system. The formal learning structure dissolves as habits are internalized.

The full arc is described in [[project-arc|The Project Arc]] (framework/structure/project-arc.md).

### The Three Stages

The methodology adapts to your development as a practitioner through three stages:

**Shu (守) — Follow the form.** You're new to the domain. A ho author prescribes the work: what to build, in what order, with what tools. Your job is to execute, verify, and reflect. Structure is tight because you're building foundational habits.

**Ha (破) — Break from the form.** You can identify problems and evaluate approaches. You scope your own work. The ho gives you a problem; you decide the solution. Structure shifts from prescriptive instructions to a decision-making framework: Think → Execute → Reflect.

**Ri (離) — Transcend the form.** You're a practitioner. You identify problems, design solutions, and execute independently. The ho is a minimal recording format — problem, solution, what changed, results. The structure has been internalized.

These stages don't progress in a straight line. You might be at ri for Python and shu for Docker in the same project. New domains reset you to shu regardless of overall experience.

The full model is in [[shu-ha-ri|Shu-Ha-Ri Progression]] (framework/structure/shu-ha-ri.md).

### Key Concepts

**The ho** is the fundamental unit of work. Bounded, produces a deliverable, committed to version control, followed by reflection, and sequenced within the arc. See [[ho-structure|Ho Structure]] (framework/structure/ho-structure.md) for the complete specification including the numbering system.

**Tiered understanding** is how you manage what you know. Not everything needs to be understood at the same depth. Tier 1 (Black Box): use it without investigating. Tier 2 (Functional): understand enough to modify and explain. Tier 3 (Deep): could redesign it. Declaring tiers honestly is a core skill. See [[tiered-understanding|Tiered Understanding]] (framework/structure/tiered-understanding.md).

**The devlog** is structured reflection written after each ho. It's the primary evidence of learning — the artifact that distinguishes "I built this" from "AI built this while I watched." The format evolves across stages: learning journal in shu, decision record in ha, absorbed into the work itself in ri. See [[devlog|The Devlog]] (framework/structure/devlog.md).

**Agent tasks** are what you hand to AI when you're delegating bounded implementation work. Not a ho — a task. "Build this feature" or "fix this exact bug," with enough specification that the output can be verified. Used at all stages but most common in ri. See [[agent-task-spec|Agent Task Specification]] (framework/templates/agent-task-spec.md).

---

## The Document Map

### Start Here

| Document | What it does |
|---|---|
| **You are here** | Orientation and wayfinding |
| [[kamae-project-framing|Kamae: Project Framing]] (framework/structure/kamae-project-framing.md) | How to plan a project before the first ho |
| [[template-selection-guide|Template Selection Guide]] (framework/templates/template-selection-guide.md) | Which template to use and when |

### Understand the System

These documents define how the Ho System works. Read them to understand the methodology.

| Document | What it covers |
|---|---|
| [[ho-structure|Ho Structure]] (framework/structure/ho-structure.md) | What makes a ho a ho. The five invariants. The numbering system. |
| [[shu-ha-ri|Shu-Ha-Ri Progression]] (framework/structure/shu-ha-ri.md) | The three stages. How structure adapts. Transition signals. |
| [[project-arc|The Project Arc]] (framework/structure/project-arc.md) | How hos sequence into complete projects. The five phases. What happens when plans meet reality. |
| [[tiered-understanding|Tiered Understanding]] (framework/structure/tiered-understanding.md) | Managing what you know and don't know. Honest self-assessment. |
| [[devlog|The Devlog]] (framework/structure/devlog.md) | The reflection practice. What makes a good devlog. How it evolves across stages. |

### Use the Templates

These are the tools. Copy the file, fill it in, commit it.

| Template | When to use it |
|---|---|
| [[shu-ho-template|Shu Ho Template]] (framework/templates/shu-ho-template.md) | Writing a prescriptive, author-guided ho session |
| [[ha-ho-template|Ha Ho Template]] (framework/templates/ha-ho-template.md) | Writing a decision-driven, learner-scoped ho session |
| [[ri-ho-template|Ri Ho Template]] (framework/templates/ri-ho-template.md) | Recording practitioner-level work |
| [[agent-task-spec|Agent Task Specification]] (framework/templates/agent-task-spec.md) | Delegating bounded work to AI agents |
| [[shu-devlog-template|Shu Devlog Template]] (framework/templates/shu-devlog-template.md) | Reflection after a shu-stage ho |
| [[ha-devlog-template|Ha Devlog Template]] (framework/templates/ha-devlog-template.md) | Reflection after a ha-stage ho |

Not sure which template? Start with the [[template-selection-guide|Template Selection Guide]] (framework/templates/template-selection-guide.md).

---

## Reading Order

**If you're starting a project:** Kamae → Ho Structure → your stage template → go.

**If you're learning the methodology:** Ho Structure → Shu-Ha-Ri → Project Arc → Tiered Understanding → Devlog.

**If you're writing hos for someone else:** Shu-Ha-Ri → the relevant stage template (read the author notes) → Template Selection Guide.

**If you're evaluating the methodology:** This document → Ho Structure → Project Arc → the Kanyō pilot examples.

---

## The Kanyō Pilot

The Ho System was developed and tested through the Kanyō (観鷹) project — an automated falcon detection and timeline system for Harvard's Memorial Hall peregrine cam. The pilot produced 19 ho documents across 7 major sessions and 12 emergent branches, over approximately 6 weeks.

Kanyō is referenced throughout the framework as evidence: the numbering examples in Ho Structure, the branching patterns in Project Arc, the stage transitions in Shu-Ha-Ri, the tier progressions in Tiered Understanding. It's a real project that produced a real system, documented in real time.

Curated examples from the pilot will be published in `examples/kanyo-pilot/`.

---

## What's Here and What Isn't

This repository contains the **methodology** — the structural framework, templates, and evidence that define how the Ho System works. It's published openly under Creative Commons BY-NC-ND 4.0.

What's **not** here is the **facilitation layer** — how to design effective ho sequences for specific learners, how to calibrate difficulty, how to read devlogs diagnostically, how to adapt the methodology to domains outside software. That's the consulting work.

The methodology tells you the structure. The facilitation teaches you the judgment.
