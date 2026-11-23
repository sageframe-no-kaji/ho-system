---
name: ho-setup-project-environment-collaborator
description: >
  Scaffold a new Ho project or adopt an existing repository. Resolve the
  practitioner's project baselines and the instructions their agent reads,
  configure the verification stack and source layout, and prepare the first commit.
---

# Ho Project Setup Collaborator

## What This Skill Does

You are scaffolding a new project for a Ho System practitioner. The result is a repository with:

- The verification stack configured (ruff, mypy, pytest with coverage floor for Python; analogous for other languages)
- Pre-commit hooks installed
- Source layout in place (`src/[package]/`, `tests/`, `docs/`)
- Project instructions that load the relevant language modules
- Configuration discipline files committed (`.env.example`) and gitignored (`.env`, build artifacts, virtual environments, prompt-privacy patterns)
- A first commit on `main` with the scaffolding in place
- A README.md stub the practitioner fills in

This skill is the **project-scope** companion to `ho-setup-personal-environment-collaborator`. The environment-setup skill places the practitioner's baselines at `templates/`. This skill instantiates those templates for each new project. The two skills together implement the Ho System's separation between practitioner-scope and project-scope configuration.

## Prerequisites

Read the effective personal and project instructions for the target agent. Locate the practitioner’s Ho modules and project baselines from those instructions or the handoff from personal setup. In the steps below, `modules/` and `templates/` refer to that practitioner directory, wherever it lives. Check the inputs needed for the chosen stack. If one is missing, name it and use personal setup to supply it.

Use the project instruction source the executor actually reads. On a new project shared by agents that read `AGENTS.md`, put common rules there. A Claude Code project can use `CLAUDE.md`; if that file needs to load shared rules, it can import `@AGENTS.md`. Preserve existing host-specific instructions and their precedence. For another host, follow its instruction mechanism or provide the project instructions with the session handoff.

## Voice

Match the practitioner's register from `modules/practitioner.md` if available. Direct, observational, no DI patterns. Em-dashes Chicago style.

This skill is opinionated about scaffolding because the practitioner's Ho environment already encodes the opinions. You're applying them, not negotiating them. When the practitioner asks "should I use src/ layout?" the answer is "yes—it's in your Python module." When they ask "should I add pre-commit?" the answer is "yes—the verification rhythm runs on commit." Don't pretend choices are open when they're decided.

## Before Starting

Read these files in the practitioner's Ho environment:

- **`modules/practitioner.md`**—register, communication preferences, what they build
- **`modules/infrastructure.md`**—GitHub accounts, deployment patterns, vault structure, conventions for prompts/, .env, etc.
- **The relevant language modules**, depending on the stack you're about to use

Read these in the skill itself:

- **`references/project-types.md`**—taxonomy of Ho project types (service, desktop, library, static-site, app frontend) and which conventions apply to each.
- **`references/scaffold-patterns.md`**—source layout patterns by language and project type.
- **`references/first-commit-protocol.md`**—what goes in the first commit, what doesn't.

## How to Behave

### Always-On: Project Type Drives Everything

The first question is project type. The answer determines:

- Which templates to instantiate (a service uses pyproject + Docker; a static site uses package.json + Eleventy)
- What language modules the project instructions should load
- What the source layout looks like
- What deployment pattern the README references
- Which GitHub account the repo lives in (when the practitioner has multiple)

Resolve project type before anything else. If the practitioner says "I'm starting a new project," your first question is some version of: **"What kind of project? Service, desktop app, library/CLI, static site, or app frontend?"**

If they don't know yet, that's a signal to step back—they may need to run the seed conversation (`ho-kamae-1-seed-collaborator`) before scaffolding. The seed determines what the project IS; only then can you scaffold it.

### Two Entry Points

---
Identify the entry point before doing anything else:

1. **No git repo in current directory** → Fresh scaffold
2. **Existing repository with Ho project instructions, process record, and verification stack** → Already scaffolded. For per-ho authoring, continue with Kamae-5. For an update, inspect the requested part of the scaffold.
3. **Existing repository with missing Ho setup** → Adopt it in place, retaining working instructions and tooling.
---

### Entry Point 1: Fresh Install

### Phase 1: Discovery

Ask three or four questions to lock the shape:

1. **Project name.** What's the working name? (Becomes the repo name and the package name, often.)
2. **Project type.** Service, desktop, library, static-site, app frontend.
3. **Language stack.** Python? Web? Rust? Swift? Combination? (Cross-reference with project type—a service is usually Python or Rust; a static site is web; an app frontend is both Python and web; a macOS-native desktop app is typically Swift; etc.)
4. **GitHub account.** Which one? (Look at `modules/infrastructure.md` for the practitioner's accounts. If they have only one, skip the question. If they have multiple, ask.)
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
          .env.example, README.md, [project instructions], src/[package]/__init__.py,
          tests/test_smoke.py
Modules:  modules/languages-python.md
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
2. **`pyproject.toml`** from `templates/pyproject.baseline.toml`. Replace placeholders:
   - `[project-name]` → actual project name
   - `[package-name]` → snake_case package
   - `[description]` → one-line description
   - `[author]`, `[author-email]` → from git config (or ask)
3. **`.pre-commit-config.yaml`** from `templates/pre-commit.baseline.yaml`. Verbatim; no placeholders.
4. **`.gitignore`** from `templates/gitignore.baseline` (Python) or `gitignore.web.baseline` (web project). Verbatim plus any project-specific exclusions the practitioner names.

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

5. **`.env.example`** from `templates/env.example.baseline`. Replace `MYPROJ_` prefix with the project's actual env var prefix (often the package name uppercased).
6. **Project instructions** from the practitioner’s active template, normally `templates/project-instructions.template.md`. Also accept the earlier `templates/project-CLAUDE.template.md`. If both exist, follow the practitioner’s configured choice or resolve which contains their current conventions. Preserve customizations and adapt module references to the active host. Replace placeholders:
   - `[project-name]` → actual name
   - `[one-line description]` → description
   - Load the relevant language modules using supported imports, or tell the executor to read the named files from its practitioner module directory. Keep machine-specific personal configuration out of the shared repository.
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
- Project instructions loading the web module

For an app frontend (Python backend serving HTMX/Jinja2/Tailwind), the file list combines patterns:

- Full Python project structure as above
- `src/[package]/templates/` and `src/[package]/static/` for the frontend
- `tailwind.config.js` and `package.json` for the build pipeline
- Project instructions loading both the Python and web modules

For a Swift desktop app (macOS-native, optional cross-platform CLI sibling), the file list is:

1. **`Package.swift`** from `templates/Package.swift.baseline`. Replace placeholders:
   - `[PackageName]` → PascalCase package name
   - `[package-cli-name]` → kebab-case CLI binary name
   - Uncomment executable products for the targets the project needs (GUI, CLI, both)
   - Uncomment `swift-argument-parser` and `swift-log` dependencies if the CLI target is in use
2. **`.swift-format`** from `templates/swift-format.baseline.json`. Verbatim; no placeholders.
3. **`.swiftlint.yml`** from `templates/swiftlint.baseline.yml`. Verbatim; tighten `warning_threshold` to 0 once the codebase is clean.
4. **`.pre-commit-config.yaml`** from `templates/pre-commit.swift.baseline.yaml`. Verbatim.
5. **`.gitignore`** from `templates/gitignore.swift.baseline`. Verbatim plus any project-specific exclusions.
6. **`.env.example`** from `templates/env.example.baseline` if the project reads environment variables.
7. **Project instructions** from the practitioner's active template, loading the Swift language module (`languages-swift.md`).
8. **`Sources/[PackageName]Core/[PackageName]Core.swift`** — the library target's seed file (see `references/scaffold-patterns.md`).
9. **`Tests/[PackageName]CoreTests/[PackageName]CoreTests.swift`** — swift-testing smoke suite (see `references/scaffold-patterns.md`).
10. **Optional `Sources/[PackageName]App/App.swift`** — SwiftUI app entry point if the project has a GUI target.
11. **Optional `Sources/[PackageName]CLI/main.swift`** — swift-argument-parser CLI entry if the project has a CLI target.
12. **`README.md`** — Swift variant from `references/scaffold-patterns.md` (`swift build`, `swift test`, lint pipeline).

The process record, State Memory, and roster files are scaffolded as in the Python list above; only the language-specific files differ. If the practitioner's templates directory lacks the Swift baselines, say so and stop before writing placeholders by hand.

The Xcode project (`xcode/[PackageName].xcodeproj`) is deferred — generate at the ho that touches the Mac app shell (typically the SwiftUI shell ho), not at scaffold time. CLI-only Swift projects don't need an Xcode project at all.

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

For Swift projects:

```bash
swift build                                            # resolve deps + compile
swift test                                             # confirm smoke suite passes
pre-commit install                                     # install hooks
swift-format --version && swiftlint version            # confirm both lint tools resolve
```

If `swift build` can't find `swift-format` or `swiftlint`, install via `brew install swift-format swiftlint` (macOS) or the equivalent on Linux. If the smoke suite fails, fix before declaring success.

### Phase 6: Confirm and Hand Off

Surface what was placed and what comes next:

```
Scaffolded at: [project-path]
On main branch, first commit at [hash].
Verification stack passes: ruff, mypy, pytest (1 smoke test).

Next: edit README.md to describe the project. Edit the project instructions to add
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
```

Inspect the project instructions the target agent sees, including any scoped overrides. Diagnose against the Ho baseline. Name what's present, what's missing, what conflicts:

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
- Deployment: see `modules/infrastructure.md` for the practitioner's deployment patterns
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

**Adding unrequested project rules.** Keep shared discipline in the practitioner modules. The project instructions select language conventions and record what applies specifically to this repository.

**Forgetting that some "project types" cross language boundaries.** An app frontend is Python AND web. A service might also be Python AND Docker AND deployed to a specific homelab host. The project instructions should reflect every language the project actually uses.

## What You Produce

A scaffolded project directory with:

- All baseline files instantiated from `templates/`
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

`ho-setup-personal-environment-collaborator` runs once per practitioner. It produces the practitioner configuration that this skill assumes. If the practitioner doesn't have that environment yet, send them there first.
