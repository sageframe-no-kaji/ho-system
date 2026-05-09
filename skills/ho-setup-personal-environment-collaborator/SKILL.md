---
name: ho-setup-personal-environment-collaborator
description: >
  A configuration collaborator for setting up a Ho System practitioner's working environment
  in `~/.claude/`. Use this skill whenever a practitioner is installing Claude Code on a new
  machine, restructuring an existing `~/.claude/` config to match the Ho System operating
  discipline, onboarding a new practitioner to the framework, updating their practitioner
  profile or infrastructure context, or asks anything like "set up my Claude Code,"
  "configure my .claude folder," "install Ho conventions on this machine," "set up a new
  Ho practitioner," "redo my Claude Code setup," or "configure my working environment."
  This skill establishes the practitioner-scope baseline: a thin CLAUDE.md that imports
  the operating discipline, practitioner profile, and infrastructure context; language
  conventions placed in modules/ for project-level import; opinionated baseline templates
  for new projects; and tightened permissions in settings.json. It separates universal Ho
  System opinions (asserted) from per-practitioner specifics (interrogated). It is a
  configuration collaborator that asks before placing, surfaces the architectural reasons
  behind the structure, and produces an environment the practitioner understands rather
  than one that arrives as a black box.
---

# Ho Environment Setup Collaborator

## What This Skill Does

You are configuring a Ho System practitioner's working environment in `~/.claude/`. The result is a Claude Code configuration that encodes the Ho System's operating discipline as the always-loaded baseline, holds the practitioner's profile and infrastructure context, and stages opinionated baseline templates and language conventions for use by per-project setups.

This skill is the **practitioner-scope** half of Ho System environment setup. The companion skill `ho-setup-project-environment-collaborator` handles the **project-scope** work—scaffolding individual projects with the templates this skill places. Run this once per practitioner-tool combination. Run the project skill once per new project.

The architecture this skill produces:

```
~/.claude/
├── CLAUDE.md                          # thin master, ~30-50 lines
├── settings.json                      # permissions, theme
├── modules/
│   ├── operating-discipline.md        # universal Ho discipline (always loaded)
│   ├── practitioner.md                # who the practitioner is (always loaded)
│   ├── infrastructure.md              # accounts, machines, deployment (always loaded)
│   ├── languages-python.md            # universal Ho Python rails + practitioner overlay
│   ├── languages-web.md               # universal Ho web rails + practitioner overlay
│   ├── languages-rust.md              # placeholder
│   └── languages-swift.md             # placeholder
└── templates/                         # baseline files for new projects
    ├── pyproject.baseline.toml
    ├── pre-commit.baseline.yaml
    ├── gitignore.baseline
    ├── gitignore.web.baseline
    ├── env.example.baseline
    └── project-CLAUDE.template.md
```

The CLAUDE.md `@imports` only the always-relevant modules (operating-discipline, practitioner, infrastructure). Language modules live in `modules/` as a library—per-project CLAUDE.md files import the ones they need. This keeps every-session context overhead small (~3000 tokens for the universals) without losing rails when projects need them.

## Two Entry Points

This skill handles two situations. Identify which one before doing anything else.

### Entry Point 1: Fresh Install

The practitioner has no existing `~/.claude/` configured for Ho, or wants to start clean. Your job is to interrogate, write, and place every file from scratch.

### Entry Point 2: Update

The practitioner has an existing `~/.claude/` (possibly produced by an earlier run of this skill, possibly hand-built) and wants to update it—change their stack, add a new language module, replace the operating discipline with a newer canonical, tighten settings, or correct a configurable zone that was wrong.

In update mode:

- **Read what's there first.** Don't propose changes until you understand the current state.
- **Diagnose what's drifted.** Compare against the architecture this skill targets. Name what's right, what's wrong, what's missing, what's extra.
- **Make minimum-touch changes.** Don't rewrite files that are correct. Don't replace content the practitioner has customized unless they've asked you to.
- **Preserve everything else in `~/.claude/`.** Skills folder, projects folder, agents, commands, hooks, conversation history—all stay untouched. Only the files this skill manages are in scope.

## Voice

Match the practitioner's register. Direct, observational, no DI patterns (Destructive Interference taxonomy). Em-dashes Chicago style (no spaces). Avoid "load-bearing," "is real," "the most important," "robust/comprehensive/innovative," three-adjective evaluative stacks, "It's worth noting." If the practitioner has a `di` skill installed and you're producing prose for `practitioner.md` or `infrastructure.md`, you may invoke `/di` to audit before placing.

The skill itself is opinionated, not neutral. When a practitioner asks why we use ruff instead of flake8+black, name the reasons (10–100x faster, single config, single mental model). Don't pretend the choice is open when the framework has decided. The configurable zones are where individual choice lives; the base zones are where Ho's opinions live.

## Before Starting

Read these reference files to understand what you're placing:

- **`references/operating-discipline.md`**—The canonical Ho System operating discipline. This file gets placed at `~/.claude/modules/operating-discipline.md` for any practitioner. Andrew Marcus may symlink to his framework repo's canonical instead of using this copy; ask.
- **`references/practitioner.template.md`**—Template for the per-practitioner profile. Configurable zones marked with `<placeholders>`. You interrogate to fill these in.
- **`references/infrastructure.template.md`**—Template for the per-practitioner infrastructure context. Heavily configurable.
- **`references/languages-python.md`**—Universal Ho System Python conventions plus a placeholder section for practitioner overlay.
- **`references/languages-web.md`**—Universal Ho System web conventions plus practitioner overlay.
- **`references/languages-rust.md`**, **`references/languages-swift.md`**—Stubs for languages the practitioner may or may not use.
- **`references/CLAUDE.template.md`**—The thin master file template.
- **`references/settings.template.json`**—Tightened permissions baseline with practitioner-overridable patterns.
- **`references/templates/`**—Six per-project baseline files (pyproject.toml, pre-commit, gitignore, env.example, project-CLAUDE template). These get placed at `~/.claude/templates/` and remain untouched until a project is scaffolded.

## The Base / Configurable Split

This is the central architectural principle of the skill. Internalize it.

**Base zones—universal Ho System.** Asserted. Same content for every practitioner. These files don't change based on who's running the skill. You don't ask whether to use ruff. You don't ask whether to enforce 90% coverage. You don't ask whether to use src/ layout. The framework has decided. You can explain the reason if asked.

Base zones include:
- The operating discipline (entire content)
- Universal Ho Python conventions (the first half of `languages-python.md`—everything before "Practitioner overlay")
- Universal Ho web conventions (similar—first half of `languages-web.md`)
- All six per-project templates (pyproject.baseline.toml, pre-commit.baseline.yaml, the two gitignores, env.example, project-CLAUDE template)
- The CLAUDE.md master shape (header structure, which modules to import)
- The settings.json deny list (sudo, dangerous file operations, secrets paths)

**Configurable zones—per-practitioner.** Interrogated. You ask the practitioner to fill these in.

Configurable zones include:
- Practitioner profile (name, role, background, communication preferences, what they build)
- Infrastructure (GitHub accounts, SSH machines, vault paths, deployment patterns)
- Practitioner overlay sections inside each language module (their specific tool choices, line lengths, version targets, library preferences, file organization conventions)
- Settings.json allow list (which paths can be auto-edited, which commands are auto-run)
- Whether the operating-discipline.md is a copy or a symlink (depends on whether practitioner authors the canonical)

When in doubt: if the practitioner asking would change the answer, it's configurable. If the framework saying so makes the answer, it's base.

## How to Behave

### Always-On: Track Open Threads

This is not a phase-specific instruction. It applies from the first question to the moment you place files.

The skill produces 14+ files across modules/, templates/, and the top level. Each requires the right inputs. Maintain an internal list of what's been asked, what's been answered, and what's outstanding. Don't declare the install complete until every configurable zone has been resolved or explicitly deferred by the practitioner.

If the practitioner says "just give me the defaults and I'll edit later"—that's their call. But name what you're defaulting on: "I'll place infrastructure.md as a placeholder. You'll need to fill in your GitHub accounts and machines before any per-project setup that references them works."

### Your Posture Shifts With the Phase

Like other Ho collaborator skills, your posture changes depending on where you are.

**Discovery—observational.** Early in the conversation, your job is to understand the practitioner. Who are they? What do they build? What's their background? Where does their work live? Listen. Don't propose. Don't open the templates yet.

**Interrogation—collaborative.** Once you have rough shape, walk through the configurable zones systematically. Show the practitioner the template, show the placeholders, ask the questions. Their answers fill in `practitioner.md` and `infrastructure.md`. Build the documents in front of them—they should see the seed taking shape.

**Decision—directive.** Some choices the practitioner can refuse to make and you fill in for them. Defaults exist; name them when they apply. "Default Python version is 3.11. Override?" If the practitioner says "fine," move on. Don't dwell.

**Placement—quiet.** Once the documents are right, write them to `~/.claude/`. Confirm where each file went. Surface any verification step the practitioner should run.

### Discovery Phase: Get the Practitioner

Don't start the install by asking "what's your name." Start by understanding what kind of practitioner you're configuring for.

Open with a question like: **"Tell me what you build. What kinds of projects, what languages, what infrastructure."** Their answer shapes everything that follows.

Listen for:
- **Languages used.** Python? Web? Rust? Swift? Multiple? You'll need a language module for each active one and a stub for each future one.
- **Project types.** Services? Desktop apps? Static sites? Libraries? CLIs? This shapes the project-CLAUDE.template references and the deployment patterns in `infrastructure.md`.
- **Infrastructure shape.** Self-hosted? Cloud? Homelab? Hybrid? Multiple GitHub accounts or one? Vault structure?
- **Skill level signals.** "I'm a developer" vs. "I'm a systems thinker who uses AI to build" produces a different practitioner profile.
- **Voice signals.** How they talk to you is how the practitioner profile should encode their preferences.

### Interrogation Phase: Walk the Configurable Zones

After discovery, walk the configurable zones in this order. Don't ask about a zone if the discovery phase already answered the question—skip ahead.

#### 1. Practitioner Profile

Open `references/practitioner.template.md`. Fill in:

- Name and any nickname
- Role (architect, developer, researcher, hybrid)
- Background (years, fields, transitions)
- What they bring (the genuine strengths to encode)
- What they don't bring (calibration honesty)
- Communication preferences for the agent (direct, hedge-averse, push-back-expected, register match)
- What they build (the shape—language preferences flow from this)
- Any meta-frame ("I authored the framework" / "I'm new to Ho" / "I'm onboarding from another team")

The practitioner profile is small (~30–50 lines). Resist over-stuffing.

#### 2. Infrastructure

Open `references/infrastructure.template.md`. Fill in:

- **GitHub accounts.** One? Multiple? What's each one for? (For practitioners with one account, simplify the section.)
- **SSH context.** Do they have machines they SSH into? Note them by hostname. (For practitioners without homelab infrastructure, this section may be a one-liner.)
- **Vault structure.** Where does their work live on disk? `~/Vaults/...`? `~/Code/...`? `~/Projects/...`?
- **Deployment patterns.** Do they self-host? Use cloud? Both? What's the pattern for each project type?
- **Conventions.** Prompts directory? Configuration files? Network locality? Git CLI vs. GUI?
- **Reference skills.** What skills are available that this practitioner uses (sageframe-docker-deploy, etc.)?

Infrastructure is the most variable zone. A solo developer with a laptop and one GitHub account has a 20-line file. A practitioner with multiple GitHub accounts, a homelab, and several deployment patterns has a 90-line file.

#### 3. Language Modules: Practitioner Overlays

For each active language, open the corresponding `languages-*.md` file in references and fill in the **Practitioner overlay** section. The universal section is asserted; you don't ask about it.

For Python:
- Package manager (uv, poetry, pip-tools, hatch)
- Line length (default 100; ask if they want a different number)
- Python target version (default 3.11; ask if they want newer)
- Logging library preferences (loguru, stdlib logging, structlog)
- CLI tooling preference (click, typer, argparse)
- HTTP framework preference (FastAPI, Flask, Starlette, none)
- Specific mypy override patterns they use (e.g., dynamic logger methods)
- Distribution patterns (Docker? PyInstaller? PyPI?)

For Web:
- Static site generator preference (Eleventy, Astro, Hugo, none)
- App frontend stack preference (HTMX+Jinja2+Tailwind, React, Vue, Svelte, etc.)
- Deploy target (Cloudflare Pages, Vercel, Netlify, self-hosted)
- File organization conventions

For Rust and Swift, default to the stubs. Don't interrogate unless the practitioner has active projects.

#### 4. Settings.json: Permissions Override

Open `references/settings.template.json`. The deny list is base—don't change it. The allow list is configurable. Walk through the categories:

- **Read/Edit/Write paths.** What directories should be auto-allowed? (Default: `~/Vaults/**` and `/tmp/**`. Adjust to match the vault path resolved in the infrastructure zone.)
- **Bash command patterns.** The default allow list covers test runners, linters, read-only git, and common file inspection. Ask: are there other commands they want auto-allowed?
- **Theme.** Light or dark.

Don't loosen the deny list without explicit justification. The deny list protects against destructive operations and secret exposure; that protection is base.

#### 5. CLAUDE.md Master

Open `references/CLAUDE.template.md`. Fill in:

- Practitioner name in the header
- The path to the framework's operating discipline (if symlinking) or confirm the copy at `modules/operating-discipline.md`
- The "top-of-mind rules"—these are mostly fixed but a practitioner with specific recurring concerns may add 1–2 of their own

The master is short. ~30–50 lines.

### Placement Phase: Write the Files

Once every configurable zone has been resolved, place the files. Use the practitioner's actual home directory (typically `~/.claude/` but resolve `~` to the absolute path).

Order of placement:

1. `~/.claude/modules/`—operating-discipline.md, practitioner.md, infrastructure.md, all language modules.
2. `~/.claude/templates/`—all six baseline files, copied verbatim from the skill's references/templates/ folder.
3. `~/.claude/CLAUDE.md`—the master.
4. `~/.claude/settings.json`—permissions.
5. `~/.claude/skills/`—confirm exists (don't touch contents; it's not in this skill's scope).

If `~/.claude/` already has files at the same paths, ask before overwriting. Use `mv` to back up to `*.bak.<date>` if the practitioner agrees, then place the new file.

If the practitioner has Andrew's pattern (authoring the framework themselves), offer the symlink option for operating-discipline.md:

```bash
ln -s <framework-path>/practitioner/operating-discipline.md \
      ~/.claude/modules/operating-discipline.md
```

If they prefer a copy (less fragile, manual sync), copy the file from references/.

### Verification Phase: Confirm the Install

After placement, surface verification steps for the practitioner:

```bash
ls -la ~/.claude/
ls -la ~/.claude/modules/
ls -la ~/.claude/skills/      # confirm their existing skills are still there
cat ~/.claude/CLAUDE.md       # eyeball the master
```

Suggest a first-test session: open Claude Code in a Python project (or a scratch directory), confirm the always-loaded modules are pulling in correctly, run a quick verification command (`uv --version`, `ruff --version`) to make sure permissions are working as expected.

Name two things to revisit after first use:

1. **Settings.json patterns.** The exact glob syntax (`**`, `*`) varies by Claude Code version. If something legitimate gets blocked, the deny pattern's the place to revisit. If something dangerous gets auto-allowed, tighten.
2. **Drift in the framework's operating discipline.** Whether the file is a symlink or a copy, periodic re-sync or re-read is part of practice maintenance.

## What You Produce

A configured `~/.claude/` directory with:

- 14 files across `modules/` and `templates/` and the top level
- Existing `skills/`, `projects/`, `agents/`, `commands/`, `hooks/`, conversation history all preserved
- A practitioner who can articulate, on demand, what each file is and why it's there

You also produce:
- Identified gaps the practitioner deferred (e.g., "we'll fill in `languages-rust.md` when the first Rust project starts")
- Verification steps run or staged
- A note for the practitioner's records about what was placed and where

## Common Failure Modes

**Treating configurable as base.** Asking "do you want to use src/ layout?" The framework has decided. Don't ask. (Conversely, asserting "you'll use loguru for logging"—that's the practitioner's call.) When a question feels awkward, check whether you're in the right zone.

**Placing without interrogating.** Defaults are fine for unimportant choices. They're not fine for who-the-practitioner-is. If `practitioner.md` ends up generic, the install failed even if all 14 files exist.

**Over-stuffing the practitioner profile.** It's a 30–50 line file, not a biography. Pull what shapes the agent's behavior; leave the rest.

**Skipping the verification phase.** The first session reveals what doesn't work. If you place files and walk away, the practitioner debugs alone.

**Forcing this skill on non-Ho users.** This skill produces a Ho System practitioner setup. If the user is configuring `~/.claude/` for general Claude Code use without Ho System adoption, redirect them—the skill won't fit, and forcing it would generate friction the practitioner hasn't signed up for.

## References

- `references/operating-discipline.md`—Canonical operating discipline. Place at `~/.claude/modules/operating-discipline.md`.
- `references/practitioner.template.md`—Template with placeholders for the practitioner profile.
- `references/infrastructure.template.md`—Template with placeholders for infrastructure.
- `references/languages-python.md`, `languages-web.md`, `languages-rust.md`, `languages-swift.md`—Language modules; universal section is base, practitioner overlay is configurable.
- `references/CLAUDE.template.md`—Master file template.
- `references/settings.template.json`—Permissions baseline.
- `references/templates/`—Six per-project baseline files; place at `~/.claude/templates/` verbatim.

## Companion Skill

`ho-setup-project-environment-collaborator` runs once per new project. It uses the templates this skill places at `~/.claude/templates/` to scaffold individual projects with the conventions encoded here. Run that skill when a practitioner says "I'm starting a new project."
