---
name: ho-setup-personal-environment-collaborator
description: >
  Set up or update a Ho practitioner's working environment for their chosen agent.
  Use for a new machine, onboarding, or changes to the practitioner profile,
  infrastructure, language modules, or project baselines. Place shared Ho guidance
  where the agent can read it and preserve existing host settings during updates.
---

# Ho Environment Setup Collaborator

## What This Skill Does

Configure the operating discipline, practitioner profile, and infrastructure at practitioner scope. Keep language modules and project templates alongside them for the companion project-setup skill.

Resolve the agent and its active instruction sources before selecting paths. Reuse the practitioner’s existing Ho modules and templates across agents when available. For a fresh install, a local configuration directory such as `~/.claude/` or the active Codex home (normally `~/.codex/`) can hold them. Below, `modules/` and `templates/` are relative to that chosen directory.

| Host | Personal instructions | Loading Ho modules |
|---|---|---|
| Claude Code | The active user `CLAUDE.md` | Use supported `@path` imports. |
| Codex | The effective personal instructions in the active Codex home, usually `AGENTS.md`. Check for `AGENTS.override.md` first. | Put an explicit instruction in the file to read each named module at session start. |
| Other agents | Use their documented instruction source. If none is persistent, supply the instructions with each session. | Use a supported include or supply the modules privately with the handoff. |

Record the resolved paths in the personal instructions so a new session can find the same baselines. Keep practitioner and infrastructure details at personal scope. Skills can have their own discovery directory independent of the personal instructions. Check the host’s loading rules when choosing a path. [Codex instructions](https://developers.openai.com/codex/guides/agents-md/) · [Claude Code instructions](https://code.claude.com/docs/en/memory)

## Two Entry Points

This skill handles two situations. Identify which one before doing anything else.

### Entry Point 1: Fresh Install

The practitioner has no existing Ho practitioner environment, or wants to start clean. Your job is to interrogate, write, and place every file from scratch.

### Entry Point 2: Update

The practitioner has an existing practitioner environment (possibly produced by an earlier run of this skill, possibly hand-built) and wants to update it—change their stack, add a new language module, replace the operating discipline with a newer canonical, tighten settings, or correct a configurable zone that was wrong.

In update mode:

- **Read what's there first.** Don't propose changes until you understand the current state.
- **Diagnose what's drifted.** Compare against the architecture this skill targets. Name what's right, what's wrong, what's missing, what's extra.
- **Make minimum-touch changes.** Don't rewrite files that are correct. Don't replace content the practitioner has customized unless they've asked you to.
- **Preserve everything else in the practitioner environment.** Skills folder, projects folder, agents, commands, hooks, conversation history—all stay untouched. Only the files this skill manages are in scope.

## Voice

Match the practitioner's register. Direct, observational, no DI patterns (Destructive Interference taxonomy). Em-dashes Chicago style (no spaces). Avoid "load-bearing," "is real," "the most important," "robust/comprehensive/innovative," three-adjective evaluative stacks, "It's worth noting." If the practitioner has a `di` skill installed and you're producing prose for `practitioner.md` or `infrastructure.md`, you may invoke `/di` to audit before placing.

The skill itself is opinionated, not neutral. When a practitioner asks why we use ruff instead of flake8+black, name the reasons (10–100x faster, single config, single mental model). Don't pretend the choice is open when the framework has decided. The configurable zones are where individual choice lives; the base zones are where Ho's opinions live.

## Before Starting

Read these reference files to understand what you're placing:

- **`references/operating-discipline.md`**—The canonical Ho System operating discipline. This file gets placed at `modules/operating-discipline.md` for any practitioner. Andrew Marcus may symlink to his framework repo's canonical instead of using this copy; ask.
- **`references/practitioner.template.md`**—Template for the per-practitioner profile. Configurable zones marked with `<placeholders>`. You interrogate to fill these in.
- **`references/infrastructure.template.md`**—Template for the per-practitioner infrastructure context. Heavily configurable.
- **`references/languages-python.md`**—Universal Ho System Python conventions plus a placeholder section for practitioner overlay.
- **`references/languages-web.md`**—Universal Ho System web conventions plus practitioner overlay.
- **`references/languages-rust.md`**, **`references/languages-swift.md`**—Stubs for languages the practitioner may or may not use.
- **`references/personal-instructions.template.md`**—Template for the host’s personal instructions.
- **`references/settings.template.json`**—Claude Code permissions baseline, used only on that host.
- **`references/templates/`**—Six per-project baseline files (pyproject.toml, pre-commit, gitignore, env.example, project-instructions template). These get placed at `templates/` and remain untouched until a project is scaffolded.

## The Base / Configurable Split

This is the central architectural principle of the skill. Internalize it.

**Base zones—universal Ho System.** Asserted. Same content for every practitioner. These files don't change based on who's running the skill. You don't ask whether to use ruff. You don't ask whether to enforce 90% coverage. You don't ask whether to use src/ layout. The framework has decided. You can explain the reason if asked.

Base zones include:
- The operating discipline (entire content)
- Universal Ho Python conventions (the first half of `languages-python.md`—everything before "Practitioner overlay")
- Universal Ho web conventions (similar—first half of `languages-web.md`)
- All six per-project templates (pyproject.baseline.toml, pre-commit.baseline.yaml, the two gitignores, env.example, project-instructions template)
- The personal instruction structure and the modules it loads
- For Claude Code, the settings template’s deny list (sudo, dangerous file operations, secrets paths)

**Configurable zones—per-practitioner.** Interrogated. You ask the practitioner to fill these in.

Configurable zones include:
- Practitioner profile (name, role, background, communication preferences, what they build)
- Infrastructure (GitHub accounts, SSH machines, vault paths, deployment patterns)
- Practitioner overlay sections inside each language module (their specific tool choices, line lengths, version targets, library preferences, file organization conventions)
- Permission choices supported by the selected host
- Whether the operating-discipline.md is a copy or a symlink (depends on whether practitioner authors the canonical)

When in doubt: if the practitioner asking would change the answer, it's configurable. If the framework saying so makes the answer, it's base.

## How to Behave

### Always-On: Track Open Threads

This is not a phase-specific instruction. It applies from the first question to the moment you place files.

The skill places practitioner instructions, modules, and templates. Each requires the right inputs. Maintain an internal list of what's been asked, what's been answered, and what's outstanding. Don't declare the install complete until every configurable zone has been resolved or explicitly deferred by the practitioner.

If the practitioner says "just give me the defaults and I'll edit later"—that's their call. But name what you're defaulting on: "I'll place infrastructure.md as a placeholder. You'll need to fill in your GitHub accounts and machines before any per-project setup that references them works."

### Your Posture Shifts With the Phase

Like other Ho collaborator skills, your posture changes depending on where you are.

**Discovery—observational.** Early in the conversation, your job is to understand the practitioner. Who are they? What do they build? What's their background? Where does their work live? Listen. Don't propose. Don't open the templates yet.

**Interrogation—collaborative.** Once you have rough shape, walk through the configurable zones systematically. Show the practitioner the template, show the placeholders, ask the questions. Their answers fill in `practitioner.md` and `infrastructure.md`. Build the documents in front of them—they should see the seed taking shape.

**Decision—directive.** Some choices the practitioner can refuse to make and you fill in for them. Defaults exist; name them when they apply. "Default Python version is 3.11. Override?" If the practitioner says "fine," move on. Don't dwell.

**Placement—quiet.** Once the documents are right, place them at the resolved paths. Confirm where each file went. Surface any verification step the practitioner should run.

### Discovery Phase: Get the Practitioner

Don't start the install by asking "what's your name." Start by understanding what kind of practitioner you're configuring for.

Open with a question like: **"Tell me what you build. What kinds of projects, what languages, what infrastructure."** Their answer shapes everything that follows.

Listen for:
- **Languages used.** Python? Web? Rust? Swift? Multiple? You'll need a language module for each active one and a stub for each future one.
- **Project types.** Services? Desktop apps? Static sites? Libraries? CLIs? This shapes the project-instructions.template references and the deployment patterns in `infrastructure.md`.
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

#### 4. Host permissions

Preserve the active host’s native permission and approval settings. For Claude Code, use `references/settings.template.json` when configuring permissions. On other hosts, use documented native controls only for requested changes and omit this Claude-specific template. The following settings apply to Claude Code. The deny list is base—don't change it. The allow list is configurable. Walk through the categories:

- **Read/Edit/Write paths.** What directories should be auto-allowed? (Default: `~/Vaults/**` and `/tmp/**`. Adjust to match the vault path resolved in the infrastructure zone.)
- **Bash command patterns.** The default allow list covers test runners, linters, read-only git, and common file inspection. Ask: are there other commands they want auto-allowed?
- **Theme.** Light or dark.

Don't loosen the deny list without explicit justification. The deny list protects against destructive operations and secret exposure; that protection is base.

#### 5. Personal instructions

Open `references/personal-instructions.template.md`. Fill in:

- Practitioner name in the header
- The path to the framework's operating discipline (if symlinking) or confirm the copy at `modules/operating-discipline.md`
- The "top-of-mind rules"—these are mostly fixed but a practitioner with specific recurring concerns may add 1–2 of their own

The master is short. ~30–50 lines.

### Placement Phase: Write the Files

Place the approved files at the resolved locations. For an existing environment, merge the requested changes into the active files, preserve local customizations, and ask before replacing an existing file.

1. Place the operating discipline, its on-demand rationale, the practitioner profile, infrastructure, and the language modules in `modules/`. The practitioner may already maintain the canonical discipline in a framework checkout and prefer a link to that copy.
2. Place the project baselines from `references/templates/` in `templates/`. On update, keep the active customized baseline. If the project instructions template is still named `project-CLAUDE.template.md`, retain it under that name or migrate its content to the new name with the practitioner’s agreement.
3. Render the personal instruction template for the selected host, using paths the agent can resolve from any working directory. Include only the discipline, profile, and infrastructure at startup. Project instructions select their language modules.
4. Apply any agreed host-native permissions. Keep existing skills, commands, hooks, and session history intact.

### Verify the installation

Open a fresh session in a scratch project using the intended agent. Confirm the session can identify one rule from the personal discipline and one convention from the selected language module after reading the project instructions. Check that the project-setup skill can locate the templates. Report what loaded, where it came from, and any checks deferred to the practitioner.

## Common Failure Modes

**Treating configurable as base.** Asking "do you want to use src/ layout?" The framework has decided. Don't ask. (Conversely, asserting "you'll use loguru for logging"—that's the practitioner's call.) When a question feels awkward, check whether you're in the right zone.

**Placing without interrogating.** Defaults are fine for unimportant choices. They're not fine for who-the-practitioner-is. If `practitioner.md` ends up generic, the install failed even if every file exists.

**Over-stuffing the practitioner profile.** It's a 30–50 line file, not a biography. Pull what shapes the agent's behavior; leave the rest.

**Skipping the verification phase.** The first session reveals what doesn't work. If you place files and walk away, the practitioner debugs alone.

**Forcing this skill on non-Ho users.** This skill produces a Ho System practitioner setup. If the user is configuring an agent for general use without Ho System adoption, redirect them—the skill won't fit, and forcing it would generate friction the practitioner hasn't signed up for.

## References

- `references/operating-discipline.md`—Canonical operating discipline. Place at `modules/operating-discipline.md`.
- `references/practitioner.template.md`—Template with placeholders for the practitioner profile.
- `references/infrastructure.template.md`—Template with placeholders for infrastructure.
- `references/languages-python.md`, `languages-web.md`, `languages-rust.md`, `languages-swift.md`—Language modules; universal section is base, practitioner overlay is configurable.
- `references/personal-instructions.template.md`—Master file template.
- `references/settings.template.json`—Claude Code permissions baseline.
- `references/templates/`—Six per-project baseline files; place at `templates/` verbatim.

## Companion Skill

`ho-setup-project-environment-collaborator` runs once per new project. It uses the templates this skill places at `templates/` to scaffold individual projects with the conventions encoded here. Run that skill when a practitioner says "I'm starting a new project."
