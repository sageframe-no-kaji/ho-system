# Languages: Python

Conventions for Python work. Two sections: universal Ho System Python conventions (apply to any practitioner using Ho) and my specifics (overlay).

This module is imported by project-level CLAUDE.md files when the project uses Python. It is not loaded globally. Projects that don't touch Python don't pay this context cost.

---

## Universal Ho System Python conventions

The framework's opinions for Python. Override deliberately, not by default. Universal disciplines (verification rhythm, type error categories, tests as specification) are in `operating-discipline.md`; this section names the Python-specific implementation.

### Lint and format: ruff

Single tool, both linting and formatting. Replaces flake8 + black + isort. The reasons:

- Speed: 10–100x faster than the older stack. Verification cycle in seconds vs. tens of seconds matters when it determines whether linting happens often or only at the end.
- Single config in `pyproject.toml` under `[tool.ruff]`. No separate `.flake8`, `.isort.cfg`, or black config.
- Single mental model: one tool the practitioner can articulate what it catches and why.

`ruff format` runs first (formatting changes the code); `ruff check` runs second (analysis only reports).

### Type checking: mypy strict

mypy in strict mode is the default. Looser typing is acceptable in early scaffolding; the project converges to strict before it ships.

The three-category rule for handling errors lives in `operating-discipline.md`. Python-specific implementation: when an entire module needs an exception (dynamically-extended classes, library shape that doesn't match its stubs), use `[[tool.mypy.overrides]]` in pyproject.toml with `disable_error_code` and a comment naming why. Documented at the config level; not silent.

### Testing: pytest with ≥90% coverage enforced

pytest is the runner. `--cov-fail-under=90` is enforced—the suite fails if line coverage drops below 90%.

The "tests as specification" principle (define behavior first, implement second) lives in `operating-discipline.md`. Python-specific implementation: pytest fixtures express setup behavior; `parametrize` for variant cases; `@pytest.mark.slow` for slow tests so they can be excluded from the fast feedback loop when needed.

Coverage reporting: `--cov-report=term-missing` (visible in console) plus `--cov-report=html` (drillable). HTML output goes to `htmlcov/`, which is gitignored.

### Project layout: src/

```
project-root/
├── src/
│   └── <package>/
│       ├── __init__.py
│       ├── main.py              # or __main__.py for python -m <package>
│       └── utils/
├── tests/
├── docs/
├── pyproject.toml
├── .pre-commit-config.yaml
├── .gitignore
└── .env.example
```

src/ layout (not flat) prevents accidental imports from the development directory and makes packaging unambiguous. Tests at top level. docs/ at top level.

### Dependency management

Modern tooling, not the legacy `requirements.txt + venv + pip` triad:

- Dependencies declared in `pyproject.toml` under `[project.dependencies]`
- Dev dependencies in `[dependency-groups.dev]` (uv) or `[project.optional-dependencies.dev]`
- Lockfile committed (uv.lock, poetry.lock, etc.)

The specific tool is a practitioner choice. The principle: dependencies declared in pyproject.toml, locked, synchronized via a single command.

### Configuration discipline

- `pyproject.toml` is the central config file. Tool configs live under `[tool.*]`.
- `.env.example` is committed; `.env` is gitignored. Secrets never in version control.
- `config.yaml` (or similar) gitignored when project-specific; `configs/config.template.yaml` committed as the structure.
- `.gitignore` includes Python defaults plus the prompt-privacy convention (`prompts/`).

### Pre-commit hooks

`.pre-commit-config.yaml` runs the verification stack on every commit attempt:

- ruff format (changes the code)
- ruff check (reports)
- mypy (reports)
- pytest (optional—only if the suite is fast enough)

For slow test suites, run pytest in CI rather than blocking every commit. The practitioner makes the call.

### Python version

Minimum: 3.11. Newer is fine. The framework targets actively supported versions; 3.11 is the production floor as of early 2026.

### Type hints and docstrings

Type hints on all public APIs (exported functions, classes, module-level variables). Docstrings on modules and public functions. Internal helpers don't need exhaustive docstrings; the public surface is documented.

---

## My specifics (Andrew's overlay)

### Package management: uv

uv is my package manager. Single tool replaces pip + venv + pip-tools.

Workflow:
- `uv venv .venv` creates the project's `.venv/`
- `uv sync` installs from pyproject.toml + uv.lock
- `uv sync --extra dev` (or `--all-groups`) includes dev deps
- `uv add <package>` adds and locks
- `uv run <command>` runs in the project env without manual activation

`.venv/` gitignored. `uv.lock` committed.

### Line length: 100

Configured in ruff. Wider than the older 79/88 standard, narrower than "no limit." Carrying forward from kanyō's flake8 config.

### Python target: 3.11

For new projects, 3.11 minimum. Move to 3.12 when there's a concrete reason. Don't chase the latest just because it shipped.

### Logging: loguru with custom event()

Structured logging via loguru. Native methods (`info`, `error`, `warning`, `debug`) plus a custom `event()` method I add dynamically. Standard `logging` rarely used.

The custom event() method requires a per-module mypy override:

```toml
[[tool.mypy.overrides]]
module = [
    "package.utils.logger",
    "package.utils.notifications",
    "package.detection.event_handler",
]
disable_error_code = ["attr-defined"]
# Custom event() method dynamically added to Logger; mypy can't see it
```

Comment in pyproject.toml names why.

### CLI tooling

click or typer for project CLIs. `m4bmaker` uses typer. argparse only for trivial one-off scripts.

### HTTP layers

FastAPI for backends. Kanyō, Glassroom, and Shodō all use it. Single process serves the API, the MCP server (when present), and any web UI mounted in the same app.

### File organization conventions

- `src/<package>/`—the package
- `src/<package>/utils/`—shared utilities (logger, config, notifications)
- `src/<package>/main.py` or `__main__.py`—entry points
- `scripts/`—operational scripts and one-offs
- `man/`—CLI man pages (when shipping a CLI; m4bmaker has these)
- `docs/`—project documentation
- `tests/`—pytest tests

### Test discovery configuration

```toml
[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = ["test_*.py"]
python_classes = ["Test*"]
python_functions = ["test_*"]
addopts = [
    "--verbose",
    "--cov=src/<package>",
    "--cov-report=term-missing",
    "--cov-report=html",
    "--cov-fail-under=90",
]
```

### Distribution patterns

Pattern depends on what the project IS:

- **Service** (Kanyō, Glassroom, Shodō, Hōzō) → Docker via `sageframe-docker-deploy`
- **Desktop app** (m4bmaker) → PyInstaller with platform spec files; signed/notarized macOS `.app`, Inno Setup Windows installer
- **Library or CLI** → pip-installable via pyproject.toml metadata; PyPI when public, GitHub Releases when not

Deployment infrastructure detail in `@modules/infrastructure.md`.

---

## Templates

Concrete starting files for new Python projects live in `~/.claude/templates/`:

- `pyproject.baseline.toml`—drop in, fill placeholders
- `pre-commit.baseline.yaml`—ruff + mypy + pytest stack
- `gitignore.baseline`—Python defaults plus conventions

The `ho-setup-personal-environment-collaborator` skill (when built) places these. Per-project ho-00 instantiates them with project-specific values.
