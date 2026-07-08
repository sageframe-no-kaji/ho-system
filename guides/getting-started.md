---
id: "4.1"
title: "Getting Started with the Ho System"
type: guide
stage: n/a
status: draft
tags: [ho-system, onboarding, getting-started]
---

# Getting Started with the Ho System

This guide walks your first arc: from an empty directory to a first ho closed and committed. It assumes you can already write code, use git, and run an agentic IDE. It does not assume you know the Ho System. Read [[the-ho-system|The Ho System]] (framework/the-ho-system.md) first if you haven't — this guide is the doing, that document is the what and why.

The shape of the first arc is: frame the project, scaffold the repo, then run bounded sessions until it works. Framing happens in chat. Building happens in your IDE. The two talk to each other through documents. That separation is the practice, not an accident — read [[operating-discipline|The Operating Discipline]] (practitioner/operating-discipline.md) once before you start, because everything below assumes it.

## Install the skills

The methodology is operationalized as a set of skills — self-contained collaborators you invoke by name. You don't need all of them on day one, but you do need two kinds: the one that configures your environment, and the Kamae chain that frames and builds a project. The full catalog is in [[ho-skill-overview|Ho System Skills — Overview]] (skills/ho-skill-overview.md).

Run `ho-setup-personal-environment-collaborator` once. It interrogates your stack — language, IDE, model access — reads the operating discipline, and writes the configuration that makes your agent conform to it across every project: `CLAUDE.md`, a lint stack, pre-commit hooks, commit conventions. After this, your environment knows the discipline and you don't have to re-explain it each session.

Everything else is per-project. You install nothing further; you invoke the Kamae skills as you reach each link in the chain.

## The first conversation: a seed

Start in chat, not the IDE. Invoke `ho-kamae-1-seed-collaborator` and talk through your idea.

The **seed** is the founding document — the problem, what already exists in the space, the vision, who it's for, the constraints reality imposes, and your first architectural opinions. It is not a brainstorming dump and not a spec. It's the *parti*: the core idea every later decision gets judged against. The seed collaborator probes — it pushes back on thin framings and names what's missing — so expect to be questioned, not transcribed. What "done" looks like is set out in [[kamae-project-framing|Kamae: Project Framing]] (framework/structure/kamae-project-framing.md) §2.1.

When the seed holds, you have the reference point for the rest of the chain.

## Walk the chain: 2 → 3 → 4

Each remaining framing skill reads the documents before it and raises the level of commitment. Run them in order, each in chat:

- `ho-kamae-2-system-design-collaborator` turns the seed's opinions into **decisions** — components, data flow, technology stack, deployment. This is the one document in the chain that freezes once committed; you change it later only through a dated addendum, never a silent edit.
- `ho-kamae-3-readme-collaborator` turns those decisions into **scope** — the README, written in present tense as if the project already exists. Writing it now forces you to say what "done" looks like before any code exists.
- `ho-kamae-4-overview-collaborator` turns scope into a **sequence** — phases first, then the hos inside each phase, with release tags at the boundaries.

The chain is not always linear. Writing the system design may reveal the seed was too broad; writing the overview may reveal a component needs splitting. Loop back and revise when a later document exposes a problem in an earlier one. The seed persists as the thing you check against: does this revision still serve the core idea?

## Scaffold, then open the first ho

Back in the IDE, run `ho-setup-project-environment-collaborator`. It builds the repo the first ho needs — source layout, `pyproject.toml` (or your stack's equivalent), the lint and test stack, pre-commit hooks, the `ho-process/` directories, and an initial commit.

Now open your first **ho** — a bounded session that produces a real deliverable and leaves a record. The five properties that make a ho a ho (bounded, deliverable, traceable, reflective, sequenced) are in [[ho-structure|Ho Structure]] (framework/structure/ho-structure.md). Invoke `ho-kamae-5-authoring-collaborator` to author the per-ho document — the scope for this one session: what's in, what's out, what "done" means, which decisions it resolves.

Your first ho is **ho-00**, the orientation ho. It doesn't build a feature; it establishes the project's conventions and hands off to the first building ho. After it, the arc's first real work is ho-01.

## Close it

A ho isn't done when the code runs. It's done when it's verified and committed. Before you close:

1. **Lint** and **type-check** clean.
2. **Tests** pass, at the coverage the discipline sets.
3. **Commit** — atomic, with a message that names the ho.
4. Flip the per-ho document's `status:` to `complete`, and record the closing commit.

Then stop. Don't roll the next ho into the same session. Bounded sessions keep the agent's context clean and keep you in the work rather than reviewing a wall of output after the fact.

## What the record looks like after

Open the repo after your first building ho closes and you should see: the four framing documents (the seed, system design, and ho overview under `ho-process/`, the README at the repo root), a per-ho document for ho-00 and one for ho-01 with `status: complete`, a commit history where the messages name the hos, and working code with passing tests behind it. Anyone — a collaborator, an agent arriving fresh, you in six months — can read that record top to bottom and understand what was built and why.

That's the first arc. The next ho starts a fresh session, in planning mode, against the next slot in the overview.

## Where to go next

- [[choosing-a-project|Choosing Your First Project]] (guides/choosing-a-project.md) — if you don't yet know what to build.
- [[ai-collaboration|Working with AI as Implementation Partner]] (guides/ai-collaboration.md) — the operating posture inside each session.
- [[shu-ha-ri|Shu-Ha-Ri Progression]] (framework/structure/shu-ha-ri.md) — how the structure loosens as you develop.
