---
name: ho-setup-project-environment-collaborator
description: >
   A scaffolding collaborator for starting new Ho System projects. Use when a practitioner asks
   to set up a new project or repo, initialize a codebase, create boilerplate, or configure the
   verification stack. It determines project type and language stack, instantiates project
   baselines from `~/.claude/templates/`, scaffolds source layout, configures lint/type-check/
   tests/pre-commit, creates a project-level CLAUDE.md with the right language imports, and
   prepares the first commit. This is the project-scope companion to
   `ho-setup-personal-environment-collaborator` (which sets up practitioner-scope `~/.claude/`).
   If that environment is missing, redirect there first.
---

# Ho Project Setup Collaborator

## What This Skill Does

You are scaffolding a new project for a Ho System practitioner. The result is a repository with:

- The verification stack configured (ruff, mypy, pytest with coverage floor for Python; analogous for other languages)
- Pre-commit hooks installed
- Source layout in place (`src/[package]/`, `tests/`, `docs/`)
- A project-level `CLAUDE.md` that imports the relevant language modules from `~/.claude/modules/`
- Configuration discipline files committed (`.env.example`) and gitignored (`.env`, build artifacts, virtual environments, prompt-privacy patterns)
- A first commit on `main` with the scaffolding in place
- A README.md stub the practitioner fills in

This skill is the **project-scope** companion to `ho-setup-personal-environment-collaborator`. The environment-setup skill places the practitioner's baselines at `~/.claude/templates/`. This skill instantiates those templates for each new project. The two skills together implement the Ho System's separation between practitioner-scope and project-scope configuration.

## Prerequisites

This skill assumes `~/.claude/` is configured for Ho System work. Before doing anything, confirm:

```bash
ls ~/.claude/CLAUDE.md
ls ~/.claude/modules/operating-discipline.md
ls ~/.claude/templates/pyproject.baseline.toml
```

If these don't exist, redirect: "It looks like your Claude Code environment isn't configured for Ho System yet. Run the `ho-setup-personal-environment-collaborator` skill first—it sets up `~/.claude/` with the operating discipline, language modules, and the per-project templates this skill uses. Once that's done, come back here and we'll scaffold the project."

Don't proceed without the environment. The templates this skill instantiates live there; without them, this skill has nothing to instantiate.

## Voice

Match the practitioner's register from `~/.claude/modules/practitioner.md` if available. Direct, observational, no DI patterns. Em-dashes Chicago style.

This skill is opinionated about scaffolding because the practitioner's `~/.claude/` already encodes the opinions. You're applying them, not negotiating them. When the practitioner asks "should I use src/ layout?" the answer is "yes—it's in your Python module." When they ask "should I add pre-commit?" the answer is "yes—the verification rhythm runs on commit." Don't pretend choices are open when they're decided.

## Before Starting

Read these files in the practitioner's `~/.claude/`:

- **`~/.claude/modules/practitioner.md`**—register, communication preferences, what they build
- **`~/.claude/modules/infrastructure.md`**—GitHub accounts, deployment patterns, vault structure, conventions for prompts/, .env, etc.
- **The relevant language modules**, depending on the stack you're about to use

Read these in the skill itself:

- **`references/project-types.md`**—taxonomy of Ho project types (service, desktop, library, static-site, app frontend) and which conventions apply to each.
- **`references/scaffold-patterns.md`**—source layout patterns by language and project type.
- **`references/first-commit-protocol.md`**—what goes in the first commit, what doesn't.

## How to Behave

### Always-On: Project Type Drives Everything

The first question is project type. The answer determines:

- Which templates to instantiate (a service uses pyproject + Docker; a static site uses package.json + Eleventy)
- What language modules the project's CLAUDE.md should import
- What the source layout looks like
- What deployment pattern the README references
- Which GitHub account the repo lives in (when the practitioner has multiple)

Resolve project type before anything else. If the practitioner says "I'm starting a new project," your first question is some version of: **"What kind of project? Service, desktop app, library/CLI, static site, or app frontend?"**

If they don't know yet, that's a signal to step back—they may need to run the seed conversation (`ho-kamae-1-seed-collaborator`) before scaffolding. The seed determines what the project IS; only then can you scaffold it.

### Two Entry Points

---
Identify the entry point before doing anything else:

1. **No git repo in current directory** → Fresh scaffold
2. **Git repo exists, no CLAUDE.md, no pyproject.toml** → Adopt into Ho
3. **Git repo exists, CLAUDE.md present with Ho @imports** → Already scaffolded;
   redirect to ho-kamae-5-authoring-collaborator
---

### Entry Point 1: Fresh Install

### Phase 1: Discovery

Ask three or four questions to lock the shape:

1. **Project name.** What's the working name? (Becomes the repo name and the package name, often.)
2. **Project type.** Service, desktop, library, static-site, app frontend.
3. **Language stack.** Python? Web? Both? (Cross-reference with project type—a service is usually Python or Rust; a static site is web; an app frontend is both Python and web; etc.)
4. **GitHub account.** Which one? (Look at `~/.claude/modules/infrastructure.md` for the practitioner's accounts. If they have only one, skip the question. If they have multiple, ask.)
5. **Process-record posture.** One question, keyed first to repo visibility and then to publication intent. Every project gets a Kamae 6 State Memory inside its canonical process root — always present, private by default (`framework/structure/cross-session-continuity.md` §10; `framework/structure/process-publication-boundary.md`):
   - **Private project repo** → track canonical `ho-process/` with the project. Versioned, private, zero extra machinery.
   - **Public repo, private process** → ignore canonical `ho-process-private/` in the parent and version it as a **nested private repo**. Publish no process tree. This is the recommended public-repo baseline.
   - **Public repo, public process showcase** → canonical nested-private `ho-process-private/` plus generated tracked `ho-process/`. Scaffold the exporter, source-revision manifest, public checker, and private pre-push gate required by 2.15.
   - **Floor** → leave the canonical process root unversioned. Legitimate; loses process history and weakens recovery.

   Default to tracked `ho-process/` for a private project and nested-private `ho-process-private/` for a public project. Add the generated public tree only when the practitioner explicitly elects a public process showcase.

Compute defaults from the answers:

- **Package name.** Convert project name to a Python-importable identifier (replace `-` with `_`). Show the practitioner: "Package name will be `my_project`. Override?"
- **Repo path.** From the infrastructure module's vault structure: `~/[vault-root]/[account]/[project-name]/`. Confirm.
- **Project description.** One line. Either pull from the seed if it exists, or ask.

### Phase 2: Confirm the Build Plan

Before placing anything, name what you're about to do:

```
About to scaffold:

Path:     ~/Vaults/sageframe/no-kaji-dev/[project-name]/
GitHub:   sageframe-no-kaji
Type:     [type]
Stack:    Python (uv-managed)
Files:    pyproject.toml, .pre-commit-config.yaml, .gitignore,
          .env.example, README.md, CLAUDE.md, src/[package]/__init__.py,
          tests/test_smoke.py
Modules:  @~/.claude/modules/languages-python.md
Pre-commit hooks: ruff format, ruff check, mypy
```

Wait for confirmation before proceeding. The practitioner may want to adjust scope (skip the smoke test, include or exclude README, etc.).

### Phase 3: Place the Files

Once confirmed, instantiate the templates with the project's specifics. For a Python service:

1. **Create the project directory.** Set `[process-root]` from the chosen posture: `ho-process` for a private project repository, otherwise `ho-process-private`.
   ```bash
   mkdir -p [project-path]/src/[package]/utils \
             [project-path]/tests \
             [project-path]/docs \
             [project-path]/[process-root]/hos \
             [project-path]/metadata
   ```
   `metadata/` at the repo root is the derived-machine-state zone (`framework/structure/kamae-project-framing.md` §2.4, §2.6) — home for the ho-status roster and the keisaku cache. It is generated, never hand-edited.
2. **`pyproject.toml`** from `~/.claude/templates/pyproject.baseline.toml`. Replace placeholders:
   - `[project-name]` → actual project name
   - `[package-name]` → snake_case package
   - `[description]` → one-line description
   - `[author]`, `[author-email]` → from git config (or ask)
3. **`.pre-commit-config.yaml`** from `~/.claude/templates/pre-commit.baseline.yaml`. Verbatim; no placeholders.
4. **`.gitignore`** from `~/.claude/templates/gitignore.baseline` (Python) or `gitignore.web.baseline` (web project). Verbatim plus any project-specific exclusions the practitioner names.

   > The canonical process record is private by default. A private project repository tracks
   > `ho-process/`; a public project ignores `ho-process-private/` in the parent and versions
   > that directory as a nested private repository when the practitioner takes the recommended
   > posture. A public showcase tracks only the generated `ho-process/` derivative and must
   > never weaken the private source boundary.
   >
   > The root `metadata/` zone inherits the same posture — a full ho-status roster is at least
   > as sensitive as the hos it lists (`framework/structure/kamae-project-framing.md` §2.4, §2.6).
   > Gitignore `metadata/` with the canonical private process when it contains exact project
   > state. A public showcase may export a separately sanitized metadata derivative, but must
   > not copy the private roster by implication. The cross-project dashboard reads working
   > trees, so it aggregates the roster regardless of parent-repo tracking.

5. **`.env.example`** from `~/.claude/templates/env.example.baseline`. Replace `MYPROJ_` prefix with the project's actual env var prefix (often the package name uppercased).
6. **`CLAUDE.md`** from `~/.claude/templates/project-CLAUDE.template.md`. Replace placeholders:
   - `[project-name]` → actual name
   - `[one-line description]` → description
   - Uncomment the relevant language module imports (Python, web, both, etc.)
   - Project-specific rules section: leave as a placeholder for the practitioner to fill in, OR ask for one or two project-specific rules now (private prompt path, project-specific lint rules, etc.)
   - Ho process section:
     ```
     ## Ho process

     Canonical Ho documents for this project live in `[process-root]/` and follow the canonical Kamae filename convention (see `framework/structure/kamae-project-framing.md` §2.5):
     - `[process-root]/kamae-1-[project]-seed.md` — Kamae 1
     - `[process-root]/kamae-2-[project]-system-design.md` — Kamae 2
     - `README.md` (repo root) — Kamae 3
     - `[process-root]/kamae-4-[project]-ho-overview.md` — Kamae 4
     - `[process-root]/hos/` — per-ho documents (Kamae 5)
     - `[process-root]/kamae-6-[project]-state-memory.md` — Kamae 6 (State Memory)
     - `[process-root]/agent-tasks/` — child agent task specs
     ```
7. **`[process-root]/kamae-6-[project]-state-memory.md`**—the project's State Memory (Kamae 6), created at init so the fixed canonical path exists from day one (`framework/structure/cross-session-continuity.md` §4, §10). Frontmatter: `created` (today), `type: state-memory`, `project` (slug), `kamae: 6`, `status: living`. Body is a fresh state-summary block — verbatim labels, fixed order — that reflects a just-scaffolded project:
   ```markdown
   ---
   created: [today]
   type: state-memory
   project: [project]
   kamae: 6
   status: living
   ---

   # [project] — State Memory (Kamae 6)

   The build's living cross-session memory. Always present, read first by every returning
   session to get back into stance. The block below is refreshed at every ho close and every
   session end. HOT and non-canonical — the cold record (git, per-ho Reflect, the K4
   build-record) wins on conflict. See `framework/structure/cross-session-continuity.md` (2.14).

   **STATE-SUMMARY**
   - **COMPLETED** — project scaffolded (verification stack, source layout, first commit).
   - **NEXT** — first ho.
   - **ACTION ITEMS / BLOCKS** — none.
   - **PROJECT LIFECYCLE** — kamae.
   ```
   The body grows beneath the block by event-gated accretion only as the build needs it (§4.2) — nothing more is scaffolded now.
8. **`metadata/ho-status.md`** and **`metadata/ho-status.json`**—the ho-status roster, seeded empty at init so the fixed path exists from day one (`framework/structure/kamae-project-framing.md` §2.4). The `.md` opens with a `GENERATED — do not hand-edit` header naming its sources (the K4 overview + `[process-root]/hos/`) and no rows; the `.json` carries the `ho-status/1` schema envelope with empty `phases` and a zeroed `summary`:
   ```json
   {
     "schema": "ho-status/1",
     "project": "[project]",
     "generated_at": null,
     "source": { "overview": "[process-root]/kamae-4-[project]-ho-overview.md", "hos_dir": "[process-root]/hos/" },
     "summary": { "hos": 0, "drafted": 0, "complete": 0, "planned": 0 },
     "phases": []
   }
   ```
   Both are derived artifacts, regenerated on ho close (there is no generator yet, so early refreshes are by hand). Their tracking follows the process-record posture chosen in Phase 1; a public showcase needs an explicit sanitized metadata export rather than inheriting publication from `ho-process/`.
9. **`src/[package]/__init__.py`**—empty file with package docstring.
10. **`tests/test_smoke.py`**—one trivial passing test so pytest has something to run and the verification stack can prove itself before any real code is written.
11. **`README.md`**—one-line description plus a "Setup" section showing the practitioner's commands (uv sync, pre-commit install, pytest). The practitioner expands later.

For a web static site, the file list shifts:

- `package.json` (no template; ask for npm dependencies based on the static site generator)
- `.eleventy.js` or analogous config
- `wrangler.toml` if Cloudflare Pages
- `src/` source directory with index page stub
- `CLAUDE.md` importing `@~/.claude/modules/languages-web.md`

For an app frontend (Python backend serving HTMX/Jinja2/Tailwind), the file list combines patterns:

- Full Python project structure as above
- `src/[package]/templates/` and `src/[package]/static/` for the frontend
- `tailwind.config.js` and `package.json` for the build pipeline
- `CLAUDE.md` importing both `@~/.claude/modules/languages-python.md` and `@~/.claude/modules/languages-web.md`

### Phase 4: Initialize Git

After files are placed:

```bash
cd [project-path]
git init -b main
git add -A
git status                  # show what's about to be committed
```

Show the practitioner the list. Confirm. Then:

```bash
git commit -m "Initial scaffold via ho-setup-project-environment-collaborator"
```

If the practitioner chose either nested-private posture (public repo, Phase 1 question 5), `ho-process-private/` is ignored by the parent and gets its own Git repository so the complete process history is versioned separately from the public project:

```bash
git -C [project-path]/ho-process-private init -b main
git -C [project-path]/ho-process-private add -A
git -C [project-path]/ho-process-private commit -m "Initial canonical Ho process"
```

The K6 State Memory and the cold process record now live together in the nested repository, tracked and private. No remote exists yet; the practitioner adds a private remote when ready.

If the practitioner elected the public process showcase, complete the 2.15 boundary before the parent repository's first push: add the explicit redaction policy and deterministic exporter to the private repository; add the public checker to the parent pre-commit and CI gates; generate `ho-process/` with its source-revision manifest; install the private pre-push hook; and prove that a stale or privacy-bearing export is refused. The generated public tree is the only process material the parent tracks.

Don't push. The practitioner connects to GitHub themselves (or you can offer to do it explicitly):

```bash
gh repo create [account]/[project-name] --private --source=. --remote=origin
git push -u origin main
```

Ask before the gh repo create. This is an explicit-permission action; it makes the project visible (or pre-created in your private space) on GitHub.

For a nested process repository, create and push a separate private remote such as `[project-name]-ho-process`. In the dual-record posture, push the public parent first and the private source second because the installed private pre-push hook verifies that publication order.

### Phase 5: Install Dependencies and Verify

```bash
uv venv .venv                                          # Python projects
uv sync --all-groups                                   # install runtime + dev deps
pre-commit install                                     # install hooks
pytest                                                 # confirm smoke test passes
```

Watch the output. If `pytest` fails, fix before declaring success—the smoke test should pass; if it doesn't, something's broken.

For web projects: `npm install`; for the app frontend: both.

### Phase 6: Confirm and Hand Off

Surface what was placed and what comes next:

```
Scaffolded at: [project-path]
On main branch, first commit at [hash].
Verification stack passes: ruff, mypy, pytest (1 smoke test).

Next: edit README.md to describe the project. Edit CLAUDE.md to add
project-specific rules. Connect to GitHub via:
  gh repo create [account]/[project-name] --private --source=. --remote=origin
  git push -u origin main

To start the first ho, run the per-ho skill (when available) or write the
ho document by hand against `framework/templates/ho-template.md`.
```

### Entry Point 2: Update

---
### Entry Point 3: Adopt Existing Repo into Ho

The practitioner has an existing repo that predates Ho System scaffolding and wants to
bring it into the discipline without breaking what's there.

Read the repo state first:

```bash
git remote -v
ls pyproject.toml
cat .git/config
ls .pre-commit-config.yaml
cat CLAUDE.md 2>/dev/null
```

Diagnose against the Ho baseline. Name what's present, what's missing, what conflicts:

- **Present and correct** — leave it alone
- **Present but drifted** — show the drift, ask before touching
- **Missing** — add it
- **Conflicts** — surface to practitioner before resolving

Don't reinit git. Don't create a new remote. Don't overwrite a pyproject.toml that has
real dependencies. Minimum-touch only.

Common adoption targets: projects that predate this skill (Kanyō, m4bmaker, Glassroom).
These have real configuration. The job is to layer Ho discipline on top, not replace
what works.
---

## Project Type Variations

### Service

- Python project with `[project.scripts]` entry point
- Deployment: see `~/.claude/modules/infrastructure.md` for the practitioner's deployment patterns
- May include `Dockerfile` and `docker-compose.yml` if Docker deployment is the target—generate these from the practitioner's docker-deploy skill if available, or from a project-specific template
- May include `configs/config.template.yaml` if the project uses YAML configuration

### Desktop App

- Python project with PyInstaller spec files (when the language is Python)
- `man/` directory if the app ships a CLI
- `installer/` directory for platform-specific installer configs (Inno Setup for Windows, notarization scripts for macOS)
- Distribution: GitHub Releases

### Library or CLI

- Python project with `[project.scripts]` for CLI entry points
- More restrictive `[project.dependencies]`—library should depend on as little as possible
- Distribution: PyPI when public, GitHub Releases when private

### Static Site

- Web project (no Python)
- Eleventy or similar static site generator
- Cloudflare Pages or similar deploy target
- No verification stack beyond the toolchain's lint/format defaults

### App Frontend

- Combined Python + web project
- Python backend with FastAPI (or practitioner's HTTP framework preference)
- HTML templates served by the backend, with Tailwind/Alpine/HTMX (or practitioner's frontend stack)
- Verification stack covers Python; web side runs through the toolchain's defaults

## Common Failure Modes

**Scaffolding before the project type is clear.** If the practitioner doesn't know whether they're building a service or a library, they don't know what to scaffold. Send them to `ho-kamae-1-seed-collaborator` first.

**Skipping the verification step.** A scaffold that doesn't verify clean is a broken scaffold. Run pytest after install. If the smoke test doesn't pass, find out why before declaring done.

**Hardcoding the practitioner's specifics.** This skill works for any Ho practitioner. Read their `infrastructure.md` for vault paths, account names, deployment patterns. Don't hardcode `sageframe-no-kaji` or `~/Vaults/sageframe/...`—those are Andrew's, not universal.

**Adding rules to the project-CLAUDE.md the practitioner didn't ask for.** The project's CLAUDE.md is short by design. It imports the universal stuff via `@~/.claude/modules/...` and adds project-specific rules. If the practitioner doesn't have project-specific rules yet, leave the section blank with a placeholder comment.

**Forgetting that some "project types" cross language boundaries.** An app frontend is Python AND web. A service might also be Python AND Docker AND deployed to a specific homelab host. The CLAUDE.md `@imports` should reflect every language the project actually uses.

## What You Produce

A scaffolded project directory with:

- All baseline files instantiated from `~/.claude/templates/`
- `src/[package]/`, `tests/`, `docs/` directory structure (Python projects)
- A Kamae 6 State Memory at `[process-root]/kamae-6-[project]-state-memory.md`, seeded with a fresh state-summary block, versioned per the chosen posture
- Verification stack installed and passing
- First commit on `main`
- A practitioner who can describe what each file is and why it's there

You also produce:
- The `gh repo create` command for the practitioner to run when ready
- A note about what's next (README, first ho, project-specific rules)
- Any deferred decisions (project-specific lint rules, Dockerfile, etc.) flagged for the practitioner to come back to

## References

- `references/project-types.md`—Project type taxonomy and which conventions apply to each.
- `references/scaffold-patterns.md`—Source layout patterns by language and project type.
- `references/first-commit-protocol.md`—What goes in the first commit and why.

## Companion Skill

`ho-setup-personal-environment-collaborator` runs once per practitioner. It produces the `~/.claude/` config that this skill assumes. If the practitioner doesn't have that environment yet, send them there first.
