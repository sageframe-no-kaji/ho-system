---
created: 2026-05-09
type: standalone-agent-task
project: shuji
model: claude-haiku-4-5
status: ready
---

**Goal**

Bump FastAPI from 0.110 to 0.115 in this project, fix any breaks, and verify the test suite passes.

**Files**

- Modify: `pyproject.toml` (version pin)
- Modify: anywhere in `src/` that breaks against the new version
- Modify: `uv.lock` (regenerated)

**Required Changes**

1. Update FastAPI version pin in `pyproject.toml` from `0.110` to `0.115`.
2. Run `uv sync` to regenerate the lockfile.
3. Run the test suite. For each failure: identify whether it's a FastAPI breaking change (consult release notes 0.111–0.115), make the minimum-surface fix to restore behavior.
4. Re-run the full verification stack.

**Acceptance**

- [ ] FastAPI pinned at 0.115 in `pyproject.toml`
- [ ] `uv.lock` reflects the new resolution
- [ ] Full test suite passes
- [ ] Linting clean (ruff)
- [ ] Type checking clean (mypy)
- [ ] No new dependencies added beyond FastAPI's transitive requirements

**Verification**

```bash
uv sync
uv run pytest
uv run ruff check .
uv run mypy src/

# Verify version pin
grep 'fastapi.*0\.115' pyproject.toml

# Manual check: review the diff to confirm only fastapi (and its transitive deps) changed.
git diff pyproject.toml
```

**Stop Condition**

If a breaking change requires API surface changes (request/response shapes, route signatures), stop and surface findings. API changes are practitioner decisions, not agent decisions.

**Commit**

Single commit. Message:

```
deps: bump fastapi 0.110 -> 0.115

[Note any non-trivial fixes inline.]
Tests passing, lint and type checks clean.
```
