# Dandori (段取り) — embedded in ho-kamae-5

Dandori is the practitioner's prep discipline — the carpenter's setup, the chef's mise en place. Embedded here as the toolkit the `ho-kamae-5-authoring-collaborator` uses to produce surgical agent task specs that descend from a ho's Think phase.

The skill (kamae-5) decides *whether* to decompose a ho into agent tasks. Dandori (these files) describes *how* to author each task once the decision is yes.

## Three files

- **`KOKOROE.md`** — the five guidelines the executing agent operates under. Practitioner-authority discipline distilled. Universal across dandori; foregrounded here for ho work.
- **`FORMAT.md`** — the agent task format, scoped to the Ho System. Ho-NN-AT-MM naming, `ho-process/agent-tasks/` location, ho-prefixed commit format, translation moves from a ho's Think phase to executable spec.
- **`examples/`** — three worked specs:
  - `ho-child-implementation.md` — typical ha-shaped child task (Ho-NN-AT-MM).
  - `ho-child-with-stop-condition.md` — ho-child exploration that may surface practitioner decisions; halts when it does.
  - `standalone-utility.md` — Standalone-AT, no parent ho. Maintenance and surgical fixes between hos.

## Standalone dandori vs. ho-embedded dandori

A standalone version of dandori lives at `~/Vaults/sageframe-no-kaji-dev/dandori/` and is published as its own skill — leaner, no Ho System assumptions. The version embedded here in `ho-kamae-5-authoring-collaborator/dandori/` carries the Ho System's specific requirements:

- **Naming**: `Ho-NN-AT-MM.md` for tasks descending from a ho (vs. dandori's `agent-task-YYYY-MM-DD-slug.md`).
- **Location**: `ho-process/agent-tasks/` (vs. dandori's `agent-tasks/`).
- **Frontmatter**: `parent-ho` and `task` fields for child tasks (vs. dandori's `parent` slug or path).
- **Commit format**: `ho-NN: <summary>` style with the `Ho-NN-AT-MM` spec referenced in the body.
- **Translation moves**: framed as "from the ho's Think phase decisions to executable spec" rather than "from planning conversation to spec."

The behavioral discipline is identical — the five kokoroe guidelines apply unchanged. The format is shaped to fit the Ho System's chain of authorship.

## When this gets loaded

`ho-kamae-5-authoring-collaborator/SKILL.md` decides whether a ho's work decomposes into bounded agent tasks (the criteria for that decision live in the SKILL itself, so this dandori folder loads only when warranted, not pre-emptively to evaluate the decision). When the decision is yes, the skill loads:

- `dandori/FORMAT.md` for the format reference and translation moves.
- `dandori/KOKOROE.md` to confirm the executing-agent discipline is in place (in the project's `CLAUDE.md` or embedded in the spec).
- `dandori/examples/` for shape calibration.

This DANDORI.md is the orienting document — read first when the dandori folder loads, to understand the relationship between dandori the discipline and kamae-5 the ho-authoring skill.
