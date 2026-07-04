---
id: "8.3"
title: "Ho System Environment Architecture"
type: practitioner
stage: n/a
status: stable
tags: [ho-system, practitioner, environment, architecture]
---

# Ho System Environment Architecture

How the pieces fit. Read this once to understand the shape; come back when something feels off.

This document describes the environment a Ho System practitioner runs under, the relationships between its parts, and the principles that determine where things live. It is intentionally short. The skills hold the operational detail; this document holds the map.

---

## The four locations

The Ho System practitioner's setup spans four locations on disk. Each holds different content with different update cadences and different consumers.

### 1. The framework repo

Location: typically under the practitioner's vault structure. For Andrew Marcus: `~/Vaults/sageframe/no-kaji-dev/ho-system/`.

What it holds:

- The **canonical operating discipline** (`practitioner/operating-discipline.md`)
- Framework documents describing the methodology (Kamae chain, ho structure, shu-ha-ri arc)
- Templates and reference material for ho documents
- Anything else that defines what the Ho System IS

Who reads it: human practitioners, when they want to understand the methodology. Skills, when they need canonical content to place into a practitioner's `~/.claude/`.

Update cadence: slow. The framework evolves as the methodology evolves. Most weeks see no changes.

### 2. The practitioner's `~/.claude/`

Location: `~/.claude/` on the practitioner's machine.

What it holds:

- `CLAUDE.md`—the thin master, auto-loaded every session
- `settings.json`—permissions, theme
- `modules/`—the always-loaded universals (operating-discipline, practitioner, infrastructure) plus the language module library (loaded per-project)
- `templates/`—six baseline files, instantiated by the project skill into new projects
- `skills/`—installed skills (`ho-setup-personal-environment-collaborator`, `ho-setup-project-environment-collaborator`, plus whatever else the practitioner uses)
- `projects/`, `agents/`, `commands/`, `hooks/`—Claude Code's own infrastructure, untouched by Ho skills
- Conversation history and other session state, untouched by Ho skills

Who reads it: Claude Code, every session. The agent that runs in the practitioner's IDE.

Update cadence: rare for module content. More frequent for project-specific overrides if the practitioner adds project-level CLAUDE.md files outside `~/.claude/`. Rarest for the operating-discipline module if it's a copy of the canonical.

### 3. Per-project repositories

Location: wherever the practitioner stores code. For Andrew: `~/Vaults/sageframe/<account>/<project>/`.

What each repo holds:

- The project's source code
- Its own `pyproject.toml`, `.pre-commit-config.yaml`, `.gitignore`, `.env.example`—instantiated from `~/.claude/templates/`
- A project-level `CLAUDE.md` that imports the right language modules from `~/.claude/modules/`
- Any project-specific skills, references, or prompts the practitioner wants in scope only when working on this project

Who reads it: Claude Code when working in this directory; humans when developing the project; CI when verifying.

Update cadence: continuously, as the project develops.

### 4. The skills referenced from `~/.claude/skills/`

Location: `~/.claude/skills/<skill-name>/`.

What each skill holds:

- A `SKILL.md` with frontmatter (name, description) and body (instructions for Claude when the skill activates)
- A `references/` folder with bundled content the skill needs (templates, taxonomy docs, canonical content)

Who reads it: Claude Code when the skill's activation triggers fire.

Update cadence: rare. Skills evolve when the practitioner discovers a pattern that wants to be encoded, or when an existing pattern needs refinement.

---

## Relationships between the locations

```
         ┌──────────────────────────────────────────┐
         │            FRAMEWORK REPO                │
         │   practitioner/operating-discipline      │
         │  Methodology, Kamae chain, ho structure  │
         └──────────────┬───────────────────────────┘
                        │ canonical source
                        │
                        ▼
         ┌──────────────────────────────────────────┐
         │       env-setup skill (one-time)         │
         │  Reads framework, asks practitioner,     │
         │  produces ~/.claude/                     │
         └──────────────┬───────────────────────────┘
                        │
                        ▼
         ┌──────────────────────────────────────────┐
         │             ~/.claude/                   │
         │  CLAUDE.md, modules/, templates/, skills │
         └─────┬─────────────────────┬──────────────┘
               │ auto-load every     │ templates instantiated
               │ session             │ by project skill
               │                     │
               ▼                     ▼
         ┌──────────────┐    ┌──────────────────┐
         │ Every Claude │    │  per-project     │
         │ Code session │    │  repositories    │
         └──────────────┘    └──────────────────┘
```

---

## The base / configurable split

The single architectural principle that matters most. Internalize it.

**Base zones**—universal Ho System opinions. Asserted by the framework, encoded in skills, placed verbatim. The practitioner doesn't customize these:

- Operating discipline (entire content)
- Verification rhythm (LINT. PRODUCE TESTS. EVALUATE TESTS. LINT. COMMIT.)
- Universal Ho Python conventions (ruff, mypy strict, pytest ≥90%, src/ layout)
- Universal Ho web conventions (light)
- Per-project baseline templates (pyproject.toml shape, pre-commit stack, gitignore patterns)
- Settings.json deny list (sudo, secrets paths, dangerous file operations)
- The CLAUDE.md master shape

**Configurable zones**—per-practitioner specifics. Interrogated by the env-setup skill, filled in by the practitioner:

- Practitioner profile (name, role, background, communication preferences)
- Infrastructure (GitHub accounts, SSH machines, vault paths, deployment patterns)
- Practitioner overlay sections inside language modules (specific tools, version targets, library preferences)
- Settings.json allow list (paths, command patterns)
- Whether the operating-discipline.md is a copy or a symlink

Test: if the practitioner asking would change the answer, it's configurable. If the framework saying so makes the answer, it's base.

---

## Why modules and skills coexist

The system uses two separate mechanisms for "Claude Code knows X." They serve different purposes.

**Modules** (files in `~/.claude/modules/` imported by CLAUDE.md):

- Auto-loaded every session
- Token cost: paid up front, every session
- Reliable: guaranteed presence, no activation logic
- Right for: things always relevant when the practitioner is using Claude Code (operating discipline, who they are, infrastructure context)

**Skills** (folders in `~/.claude/skills/` activated by description):

- Loaded only when the description triggers
- Token cost: zero unless invoked
- Conditional: depends on Claude recognizing the activation context
- Right for: things relevant in specific situations (DI audit, docker deployment, scaffolding a new project, setting up a new practitioner)

The Ho System uses both. The line we've drawn:

- **Universal disciplines** (operating-discipline, practitioner, infrastructure)—modules. Always loaded; cost is small; reliability is critical.
- **Language conventions** (Python, web, Rust, Swift)—modules but loaded per-project, not globally. The library lives in `~/.claude/modules/`; per-project CLAUDE.md files import the relevant ones via `@~/.claude/modules/languages-<x>.md`.
- **Operations** (DI audit, Docker deployment, environment setup, project scaffolding)—skills. Triggered by intent.

This is one specific design choice; it could change as we learn more from running the system. Skills could absorb more responsibility if their activation proves reliable. Modules could absorb more if the token cost stays manageable.

---

## How the two new skills relate

**`ho-setup-personal-environment-collaborator`** runs once per practitioner-tool combination. It:

1. Asks who the practitioner is (configurable: profile)
2. Asks where their work lives (configurable: infrastructure)
3. Asks what languages they use (configurable: language overlays)
4. Places `~/.claude/CLAUDE.md`, `~/.claude/settings.json`, `~/.claude/modules/`, `~/.claude/templates/`
5. Verifies the install

After this skill runs, the practitioner has a working baseline for Ho System work in Claude Code. They re-run it when their stack changes or when the operating discipline evolves.

**`ho-setup-project-environment-collaborator`** runs once per new project. It:

1. Asks what kind of project (service, desktop, library, static-site, app frontend)
2. Asks for the project name and language stack
3. Reads the practitioner's `~/.claude/templates/` to know what baselines apply
4. Reads the practitioner's `~/.claude/modules/infrastructure.md` to know which GitHub account, vault path, deployment pattern
5. Instantiates the templates with project-specific values
6. Scaffolds source layout, tests, docs
7. Installs the verification stack and confirms it passes
8. Stages the first commit

The first skill produces the conditions; the second skill exploits them. Project setup assumes environment setup has happened. If the practitioner runs project setup against a non-Ho-configured `~/.claude/`, it bails out and redirects.

---

## Token economics

Always loaded:
- `CLAUDE.md` master itself (~30–50 lines)
- `@modules/operating-discipline.md` (~290 lines)
- `@modules/practitioner.md` (~30–50 lines)
- `@modules/infrastructure.md` (~80–150 lines depending on the practitioner)

Total always-on cost: ~3,000 tokens per session.

Loaded when in a Python project:
- Above plus `@modules/languages-python.md` (~190 lines)

Total Python project cost: ~5,500 tokens per session.

Loaded when working on web work (project-level CLAUDE.md imports it):
- Above plus `@modules/languages-web.md` (~95 lines)

Loaded when a skill activates:
- The skill's SKILL.md plus any references it pulls in
- Cost varies; typically 1,000–5,000 tokens

For perspective: a Claude Code session has 100,000–200,000 tokens of capacity. The always-on overhead is ~2% of that. Worth it.

If at some point the always-on overhead feels too high, the migration path is clear: move modules to skills with conditional activation. The content doesn't have to change; only the wrapper.

---

## Maintenance

When the **operating discipline** evolves (you edit the canonical at `practitioner/operating-discipline.md`):

- If the practitioner symlinked: nothing to do. The agent reads the new content next session.
- If the practitioner copied: re-copy the file into `~/.claude/modules/operating-discipline.md`.

When the **practitioner profile** changes (new role, different communication preferences, background update):

- Edit `~/.claude/modules/practitioner.md` directly. No skill invocation needed for small edits. Re-run the env-setup skill if the changes are structural.

When the **infrastructure** changes (new GitHub account, new homelab machine, new deployment pattern):

- Edit `~/.claude/modules/infrastructure.md` directly.

When a **new language enters the practitioner's stack**:

- Run the env-setup skill in update mode. It will produce a new `languages-<lang>.md` module.
- OR edit one of the placeholder modules manually. The first time the practitioner uses a language, the file should be filled in (not left as the placeholder).

When the **per-project templates** evolve:

- Edit the templates in `~/.claude/templates/` directly.
- New projects scaffolded after the edit will use the new templates.
- Existing projects don't auto-update. They have their own pyproject.toml, etc.; if the templates change, existing projects can be migrated manually or via a per-project migration ho.

When **a skill's content** evolves:

- Edit the skill's SKILL.md or references/ directly.
- The next time the skill activates, it uses the new content.

---

## Failure modes to watch for

**Drift between framework and `~/.claude/`.** The framework operating discipline evolves; the copy at `~/.claude/modules/operating-discipline.md` doesn't get re-synced. Symlink avoids this; otherwise periodic re-sync is part of practice maintenance.

**Drift between `~/.claude/templates/` and existing projects.** Templates evolve; old projects don't auto-update. Acceptable, but worth noting—a migration ho per project re-aligns when it matters.

**Conditional zones loaded as if base.** A practitioner ends up with an `infrastructure.md` someone else's setup applies to. This happens when a skill places content without interrogating, or when the practitioner copies someone else's `~/.claude/` wholesale.

**Modules loaded everywhere when they shouldn't be.** If an experiment moves a language module into the global CLAUDE.md @import, every session pays for it. Watch the token cost.

**Skills not activating when they should.** If `ho-setup-personal-environment-collaborator` doesn't fire when the practitioner says "set up my Claude Code," the skill description needs broader triggers. Test by asking variations of the activation phrasing.

**Skills activating when they shouldn't.** Inverse problem. If the project skill fires when the practitioner's just opening an existing project, narrow the description.

---

## Adding to the system

When a new pattern surfaces that wants to be encoded, decide where it goes:

- Universal Ho discipline (applies to every practitioner)? → Update the framework's operating discipline; place propagates via env-setup skill.
- Universal language convention (applies to every practitioner using that language)? → Update the language module's universal section in the env-setup skill's references.
- Practitioner-specific convention? → Update the practitioner's `~/.claude/modules/<file>.md` directly.
- Per-project convention? → Add to the project's CLAUDE.md.
- A workflow that triggers on intent? → Build a new skill.

When in doubt, draft it in the smallest scope first. Promote to broader scope when it proves itself across multiple instances. The reverse (starting universal and narrowing) tends to bake in assumptions that don't generalize.

---

## What this architecture optimizes for

- **Practitioner understanding.** Every file the practitioner has, they can articulate the purpose of. No black-box config.
- **Discipline consistency.** The verification rhythm runs the same way in every Ho project, on every Ho practitioner's machine.
- **Per-practitioner customization.** The configurable zones give individual practitioners room to adapt without compromising the universal rails.
- **Token economy.** Only-always-relevant content always loads. Sometimes-relevant content loads sometimes.
- **Update propagation.** Edits to the framework reach every practitioner via the symlink-or-copy pattern. Edits to a practitioner's profile don't ripple anywhere.
- **Reliable activation.** Modules guarantee presence; skills activate by description. Each is used where its strength matters.

What it doesn't optimize for (deliberately):

- **Setup speed for non-Ho practitioners.** This is a Ho System setup. Other practitioners have other setups.
- **Infinite flexibility.** The base zones aren't negotiable. That's the point.
- **Zero token overhead.** ~3,000 tokens per session is the always-on cost. Pay it for the consistency it buys.
