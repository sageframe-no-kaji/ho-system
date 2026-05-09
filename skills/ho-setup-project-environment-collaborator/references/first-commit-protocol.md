# First Commit Protocol

The first commit on a new project's `main` branch establishes the baseline. What goes into that commit shapes how the project evolves.

## What Goes In

The first commit contains everything the project needs to verify itself:

- `pyproject.toml` (or analogous: `package.json`, `Cargo.toml`, `Package.swift`)
- `.pre-commit-config.yaml` (Python projects)
- `.gitignore`
- `.env.example`
- `README.md` (stub)
- `CLAUDE.md` (project-level, importing the right modules)
- `src/<package>/__init__.py` (Python projects)
- `tests/test_smoke.py` (Python projects)
- `docs/` (empty directory; commit a `.gitkeep` if the SCM doesn't preserve empty dirs)

For projects with explicit build pipelines:

- `tailwind.config.js`, `package.json` for app frontends
- `.eleventy.js`, `wrangler.toml` for static sites
- `Dockerfile`, `docker-compose.yml` for Dockerized services (optional in first commit; can wait until first deploy is being prepared)

## What Stays Out

The first commit does NOT include:

- **`uv.lock` or other lockfiles for unrun dependencies.** Don't commit a lockfile until `uv sync` (or equivalent) has actually been run and the lockfile reflects a real environment.
- **`.venv/`, `node_modules/`, build outputs.** Always gitignored.
- **`.env` (with real values).** The `.env.example` template is committed; the actual `.env` is gitignored.
- **Project-specific configuration files with real values.** `configs/config.template.yaml` is committed; `configs/config.yaml` is gitignored.
- **IDE files.** `.vscode/`, `.idea/`—gitignored.
- **Operating system noise.** `.DS_Store`, `Thumbs.db`—gitignored.
- **The practitioner's prompts.** `prompts/` and similar—gitignored per the practitioner's infrastructure conventions.

## Commit Message

The first commit uses a descriptive message:

```
Initial scaffold

Project type:    <type>
Stack:           <languages>
Verification:    ruff, mypy, pytest with --cov-fail-under=90
Pre-commit:      installed and verified
Modules:         <which language modules the project's CLAUDE.md imports>

Scaffolded via ho-setup-project-environment-collaborator.
```

If the practitioner prefers a shorter style:

```
Initial scaffold via ho-setup-project-environment-collaborator
```

Either is fine. Match the practitioner's existing repo style if you can see it.

## Verification Before Commit

Before the first commit, prove the verification stack runs clean:

```bash
ruff format
ruff check
mypy
pytest
```

If any of these fail, fix before committing. A first commit with broken lint or failing tests starts the project in debt.

The smoke test (`test_smoke.py`) is designed to pass; if it doesn't, the package layout is wrong or the import path is broken—diagnose and fix.

## Pre-commit Install Order

Pre-commit hooks should be installed BEFORE the first commit, so the first commit itself runs through them:

```bash
uv sync --all-groups        # installs pre-commit
pre-commit install          # registers hooks
git add -A
git commit -m "Initial scaffold"   # runs the hooks
```

If the hooks find issues at this stage (formatting, lint), the templates have a problem—report it back so the templates can be fixed for future scaffolds.

## After the First Commit

Don't push to GitHub immediately. Pushing is a separate, explicit step the practitioner takes when they're ready:

```bash
gh repo create <account>/<project> --private --source=. --remote=origin
git push -u origin main
```

The remote may not exist yet, the practitioner may want to think about visibility (public vs. private), and pushing makes the project visible in their account—each of these reasons to keep the push as a deliberate action, not a default.

If the practitioner has multiple GitHub accounts, the `gh` CLI may need to be told which account:

```bash
GH_HOST=github.com GH_USERNAME=<account-name> gh repo create ...
```

Or the practitioner has authenticated each account separately and `gh` handles routing automatically. Read their `infrastructure.md` for whatever convention they've documented.

## What's Not the First Commit

The first ho is not in the first commit. The seed conversation is not in the first commit. The README expansion is not in the first commit. Those happen later, by the practitioner, in their own commits.

The first commit's job is: establish the baseline. The verification stack runs. The structure is in place. The next commit can be substantive.
