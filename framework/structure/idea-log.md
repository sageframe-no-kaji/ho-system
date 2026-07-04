---
id: "2.13"
title: "The Idea Log"
type: structure
stage: n/a
status: stable
tags: [ho-system, structure, idea-log, ho-process]
---

# The Idea Log

## A Findable, Forward-Only Backlog of What the Project Might Do Next

The idea log is the per-project backlog where a real thought that surfaces mid-work gets
**filed, not lost** — and gets reviewed at every ho boundary until it becomes a ho, becomes a
sidequest, or is deliberately dropped. One file per project at `ho-process/ideas.md`, never
scattered, never obtusely named: every idea carries a stable, greppable `IDEA-NNN` id.

It is the counterpart to two neighbours it must not be confused with:

- Unlike `notes/` (dated *evidence behind you* — findings a ho consumes), the idea log holds
  *intentions ahead of you*.
- Unlike a **sidequest** (committed work, an arc), an idea is a *candidate* that graduates into
  a ho or a sidequest, or is dropped.

It generalizes "discoveries crossing back" (ho-task-decomposition 2.8 §6) from within-a-ho to
across-the-project.

## 1. The entry

```
### IDEA-007 — short title
- captured: 2026-07-02
- surfaced-in: ho-04            # the ho it popped up in, or "—"
- status: independent          # see dispositions
- tags: [feature]

The real thought, in enough detail to reconstruct later.
```

The `IDEA-NNN` id is permanent and greppable — findability is the whole point. Ids are never
reused; abandoned ideas keep their number (forward-only).

## 2. The four dispositions

- **independent** — filed, no ho home yet. The default at capture.
- **linked → ho-NN** — assigned to a planned ho in the ho overview (Kamae 4).
- **resolved → ho-NN** — folded into a ho that shipped. Terminal, kept.
- **dropped: `<reason>`** — decided against. Terminal, kept with the reason.

Entries are **never deleted**. They move forward through states; the log is a permanent record
of what was considered and why the dropped ones fell (forward-only). A resolved or dropped idea
stays on the page as part of the project's decision history.

## 3. The two rituals

**Capture.** An idea that surfaces outside the current ho's scope is filed here immediately as
`independent`, with `surfaced-in` noting where it came from. This protects the ho from
scope-creep and the idea from being lost — the two failure modes the log exists to prevent.

**Review at each ho boundary.** Ho authoring (Kamae 5) and overview replan checkpoints (Kamae 4)
scan the log: pull `linked → this-ho` entries into the ho's Think phase, and re-scan
`independent` entries for any now relevant. This is the guarantee that nothing filed is
forgotten — an idea captured ten hos ago resurfaces exactly when the work reaches it.

## 4. Location and findability

One file per project: `ho-process/ideas.md`. Never split across files, never renamed to
something a `grep IDEA-` won't find. The stable id is the contract: any document may cite
`IDEA-NNN` and the reference stays valid for the life of the project. The framework repo
dogfoods the convention in its own `ho-process/ideas.md`.

## 5. Related Framework Documents

- [[kamae-project-framing|Kamae: Project Framing]] (framework/structure/kamae-project-framing.md) §2.6 — the ho-process layout the log lives in
- [[ho-task-decomposition|Ho-Task Decomposition]] (framework/structure/ho-task-decomposition.md) §6 — discoveries crossing back, which the log generalizes
- [[artifact-type-registry|Artifact Type Registry]] (framework/structure/artifact-type-registry.md) — `notes/` (evidence) versus the idea log (intentions)

---

_This document is part of the Ho System framework. The idea log is a native ho-process
convention: a findable, forward-only backlog reviewed at every ho boundary. Its live instance is
the framework repo's own `ho-process/ideas.md`._
