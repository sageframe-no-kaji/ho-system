---
created: 2026-07-02
type: idea-log
project: ho-system
status: living
---

# Idea Log

The findable, forward-only backlog of ideas and features that surface during the work —
so a real thought uncovered mid-ho gets **filed, not lost**, and gets reviewed at every
ho boundary until it becomes a ho, becomes a sidequest, or is deliberately dropped.

One file per project (`ho-process/ideas.md`; for the framework repo, this root file).
Never scattered, never obtusely named — every idea has a stable `IDEA-NNN` id you can
grep and cite.

## How it works

**Entry shape:**

```
### IDEA-007 — short title
- captured: 2026-07-02
- surfaced-in: ho-04            # the ho it popped up in, or "—"
- status: independent          # see dispositions
- tags: [feature]

The real thought, in enough detail to reconstruct later.
```

**Dispositions (the discipline):**

- `independent` — filed, no ho home yet. The default at capture.
- `linked → ho-NN` — assigned to a planned ho in the ho-overview (kamae-4).
- `resolved → ho-NN` — folded into a ho that shipped. Terminal, kept.
- `dropped: <reason>` — decided against. Terminal, kept with the reason.

Entries are **never deleted**. They move forward through states; the log is a permanent
record of what was considered and why the dropped ones fell (forward-only).

**Two rituals:**

1. **Capture.** An idea surfaces that isn't the current ho's scope → file it here
   immediately as `independent`, note `surfaced-in`. Protects the ho from scope-creep
   and the idea from being lost.
2. **Review at each ho boundary.** Ho authoring (kamae-5) and overview replan
   checkpoints (kamae-4) scan this log — pull `linked → this-ho` entries into the ho's
   Think, and re-scan `independent` entries for any now relevant. This is the guarantee
   that nothing is lost.

**Not its neighbours:** unlike `notes/` (dated *evidence behind you*), these are
*intentions ahead of you*. Unlike a **sidequest** (committed work), an idea is a
*candidate* that graduates into a ho or a sidequest, or is dropped. It is "discoveries
crossing back" (ho-task-decomposition §6) generalised from within-a-ho to
across-the-project.

---

## Ideas

### IDEA-001 — Idea-log as a native Ho-process convention
- captured: 2026-07-02
- surfaced-in: Fable-audit merge session
- status: linked → idea-log-convention ho (see merge-decisions D16)
- tags: [framework, process]

Canonize the convention this file instantiates: a findable per-project
`ho-process/ideas.md`, capture-on-surface, review-at-each-ho, forward-only dispositions.
Add `ideas.md` to kamae-project-framing §2.5 (ho-process layout), a short structure doc,
and the review hook in the kamae-5 authoring skill and kamae-4 replan checkpoints. This
live file is the dogfood instance; the ho makes it doctrine.

### IDEA-002 — `ho-kamae-design-collaborator` skill (possibly a looser skill)
- captured: 2026-07-02
- surfaced-in: design-work review (merge-decisions D10)
- status: independent
- tags: [skill, design]

We are actively moving toward a design-work skill, possibly looser than the Kamae
collaborators (interrogate the question list, generate isolated session prompts in the
project's constraint vocabulary, maintain the living register). Until it exists, the
design-work doctrine doc + `~/.claude/modules/design-work.md` are the clear interim
reference. Revisit the skill's shape when a third project runs the design modality.

### IDEA-003 — Kinhin exposes the idea log as tools
- captured: 2026-07-02
- surfaced-in: idea-log design
- status: linked → kinhin
- tags: [kinhin, tooling]

`kinhin_record` captures an idea into `ideas.md`; a read verb (`kinhin_ideas`) and a
surfacing at `kinhin_next` / ho boundaries drive the review ritual. The file stays the
source of truth; Kinhin is the hands. Fold into Kinhin's tool surface (deliverable 3).

### IDEA-004 — Promote `ho-smoke-collaborator`; write the smoke/dogfood structure doc
- captured: 2026-07-02
- surfaced-in: smoke/dogfood correction (merge-decisions D15)
- status: independent
- tags: [framework, smoke]

shodo's smoke/dogfood discipline (graded smoke test + dogfood finding, grader/observer,
floors) is unusually deep and currently single-project. Promote the project-local skill
to the framework catalog with an entry; write the structure doc when a **second** project
runs a smoke pass. Register the two artifacts provisionally in the type registry now.

### IDEA-005 — Name the 11 remaining unnamed concepts
- captured: 2026-07-02
- surfaced-in: Fable audit (concepts-without-names)
- status: independent
- tags: [framework, naming]

A decide-and-name session. Highest value: #7 the two-tier separation of thinking from
coding (belongs on the site's front page the day it has a name). Watch: #4 the living
register must not deepen the collision with the architectural/executable "register" in
ho-task-decomposition. #5 (severable-capability chain) is already named: **sidequest**.
Gates several glossary entries and the registry's open list.
