# Agent Task Format — Dandori, ho-scoped

The full format reference for an agent task: the surgical spec an autonomous coding agent reads to execute one bounded unit of work. Loaded by `ho-kamae-5-authoring-collaborator` when the Think phase reveals the work decomposes into bounded tasks. Also usable for standalone tasks authored outside any parent ho.

Each task is one bounded unit of work — small enough for a single agent session, large enough to hand off as a standalone artifact. The agent reads one spec, produces defined output, and verifies against explicit acceptance criteria.

This format is dandori adapted for the Ho System. The standalone dandori format lives at `~/Vaults/sageframe-no-kaji-dev/dandori/skill/FORMAT.md` — leaner, with no Ho System assumptions. This version honors Ho System–specific requirements: child-task naming (`Ho-NN-AT-MM`), location (`ho-process/agent-tasks/`), frontmatter (`parent-ho`, `task`), commit format (`ho-NN: ...`), and translation moves from a ho's Think phase to executable spec.

---

## Kokoroe context

Agent tasks are executed under kokoroe — the five guidelines documented in `KOKOROE.md`. Their job is to govern how the *executing* agent behaves; the format below assumes they're loaded.

When they're not, the spec compensates — more explicit `Do Not` items, more conservative `Required Changes`, stricter `Acceptance`. The cleaner pattern is to load kokoroe once at the project level (see `KOKOROE.md` for installation) and let each spec stay tight.

---

## Naming and location

### Child tasks (descend from a ho)

**Naming:** `Ho-NN-AT-MM.md`

- `NN` is the parent ho number, zero-padded (`01`, not `1`; `00` for ho-00 if it ever has a task).
- `MM` is the task number within the ho, zero-padded.
- The capital letters and hyphens matter — pattern-matchable across the project.

Examples: `Ho-01-AT-01.md`, `Ho-01-AT-02.md`, `Ho-15-AT-03.md`.

**Location:** `ho-process/agent-tasks/` — a sibling directory to `ho-process/hos/`.

Why sibling rather than nested under hos:

- Agent tasks are referenced from per-ho documents but read independently by the agent. Sibling placement keeps the path short.
- Some agent tasks are standalone (no parent ho). Sibling placement accommodates both child and standalone use without restructuring.
- Pattern-matching across all agent tasks (find every task that touches a particular file, search for tasks that reference a specific dependency) is easier when they're all in one directory.

### Standalone tasks (no parent ho)

**Naming:** `Standalone-AT-YYYY-MM-DD-slug.md`

- Date the task was authored.
- Slug: three to six words, kebab-case, descriptive.
- Examples: `Standalone-AT-2026-05-09-bump-fastapi.md`, `Standalone-AT-2026-05-02-anthropic-export-shape.md`.

**Location:** `ho-process/agent-tasks/` — same directory as child tasks. The naming distinguishes them.

The `Standalone-AT-` prefix sorts standalone tasks together; the date sorts them chronologically; the slug carries the semantics.

---

## Format

Agent tasks have required sections (always present, even if brief) and optional sections (included when warranted). Order matters — agents read top to bottom.

### Required sections

#### Frontmatter

Child task:

```yaml
---
created: YYYY-MM-DD
type: agent-task
project: <slug>
parent-ho: <NN>           # parent ho number, zero-padded (e.g., "01")
task: <MM>                # task number within the ho, zero-padded
status: ready             # ready | in-progress | complete | blocked
---
```

Standalone task:

```yaml
---
created: YYYY-MM-DD
type: standalone-agent-task
project: <slug>
status: ready
---
```

The `parent-ho` and `task` fields link a child task to its parent ho. Standalone tasks omit them and use `type: standalone-agent-task` to be explicit about the absence.

#### Goal

One to three sentences. What this task produces. Concrete enough that "done or not done" is unambiguous.

Bad: "Improve the parser."
Good: "Implement `AnthropicParser` that reads Anthropic conversation exports and yields validated `ConversationRecord` objects, with full unit test coverage against synthetic fixtures."

#### Files

A bullet list of files the task creates, modifies, or reads. Use absolute paths from repo root. Indicate `Create:`, `Modify:`, or `Read-only:` (read but not changed).

````markdown
**Files**

- Create: `src/shuji/parsers/anthropic.py`
- Create: `tests/parsers/test_anthropic.py`
- Create: `tests/fixtures/anthropic/synthetic_5.json`
- Read-only: `src/shuji/parsers/base.py` (Parser protocol reference)
- Modify: `src/shuji/parsers/__init__.py` (add to `PARSERS` list)
````

If the file list is large, group by area. If a file is created and later modified within the same task, list it once with the more substantial action.

#### Required Changes

The substantive section. What the agent does, in enough detail that the work is unambiguous but not so much that it becomes pseudocode. Each change is its own subsection or numbered item.

For each change:

- **What** — the concrete artifact (function signature, test name, schema column).
- **Why** — one line, only if not obvious from context.
- **How** — only if the approach isn't obvious. Default to leaving how to the agent.

Example:

````markdown
**Required Changes**

1. **Define `AnthropicParser` class** in `src/shuji/parsers/anthropic.py`.
   Implements the `Parser` protocol from `src/shuji/parsers/base.py`.

   - `can_parse(path: Path) -> bool` returns True if the file is a JSON array
     of objects with the expected Anthropic export shape.
   - `parse(path: Path) -> Iterator[ConversationRecord]` yields one record
     per conversation. Raise on first malformed entry; no lenient mode.

2. **Add to PARSERS list** in `src/shuji/parsers/__init__.py`.
   Append `AnthropicParser` to the list.

3. **Synthetic fixtures** in `tests/fixtures/anthropic/`.
   ...
````

The agent produces the named artifacts. Architectural intent was settled in the parent ho's Think phase (or, for standalone tasks, in the planning conversation) and captured in the spec; the agent doesn't reinterpret it here.

#### Acceptance

The pass/fail criteria. The agent verifies these before declaring the task complete.

````markdown
**Acceptance**

- [ ] `AnthropicParser` class exists in `src/shuji/parsers/anthropic.py` and implements the Parser protocol.
- [ ] All unit tests in `tests/parsers/test_anthropic.py` pass.
- [ ] `AnthropicParser` registers in the `PARSERS` list.
- [ ] Linting clean (ruff, mypy).
- [ ] No new dependencies added beyond what `pyproject.toml` already declares.
````

Use checkboxes — machine-readable, and the practitioner can scan them.

#### Verification

The commands the agent runs to verify acceptance. Concrete, copy-pasteable.

````markdown
**Verification**

```bash
# Run unit tests
uv run pytest tests/parsers/test_anthropic.py -v

# Lint
uv run ruff check src/shuji/parsers/anthropic.py
uv run mypy src/shuji/parsers/anthropic.py

# Smoke test the registration
uv run python -c "from shuji.parsers import PARSERS; print([p.__name__ for p in PARSERS])"
```
````

Verification commands match acceptance criteria one-to-one. If a criterion can't be verified by a runnable command, name the manual check explicitly.

#### Commit

The agent's commit instruction. Brief — message format and any commit-time discipline.

Child task — message uses the `ho-NN:` prefix and references the spec by its full ID:

````markdown
**Commit**

Single commit. Message format:

```
ho-01: add AnthropicParser

Implement AnthropicParser per the Ho-01-AT-02 spec. Yields ConversationRecord
from Anthropic conversation exports. Strict error handling; no lenient mode.

Tests: 9 passing.
```
````

Standalone task — conventional commit prefix (deps, fix, docs, etc.) appropriate to the work:

````markdown
**Commit**

Single commit. Message:

```
deps: bump fastapi 0.110 -> 0.115

[Note any non-trivial fixes inline.]
Tests passing, lint and type checks clean.
```
````

The `ho-NN:` prefix on child-task commits keeps the git log scannable by ho.

### Optional sections

Include when warranted; omit when not.

#### Context

A few sentences when the task can't be understood from Goal alone — reserved for background from the parent ho's Think phase or planning conversation that the agent needs.

````markdown
**Context**

The parser plugin protocol was decided in Ho-01-AT-01. Each parser implements
`can_parse` (cheap dispatch check) and `parse` (the work). Strict error handling
was chosen over lenient mode in v1; see ho-01 Decision 2.
````

Used when the task references decisions made elsewhere that the agent can't see.

#### Problem

For ri-shaped hos or standalone maintenance tasks: what's broken, what's the symptom. Skipped for ha-shaped hos where Goal already states the work.

````markdown
**Problem**

The `summary` column was added in ho-01 but never populated. ho-05's titler
expects to read summaries from existing rows, so unpopulated rows raise.
Backfill needed.
````

#### Do Not

Explicit out-of-scope items the agent might otherwise wander into. Reserved for cases where the boundary is non-obvious.

````markdown
**Do Not**

- Do not add a migration framework. Schema changes are ALTER TABLE in the
  ho that introduces them, per ho-01 Decision 4.
- Do not modify `ConversationRecord`'s public fields. The model is shared
  with the indexer; changing it has cross-ho implications.
- Do not add lenient parsing modes. Strict was decided in ho-01 Decision 2.
````

A guard against drift. Use sparingly; every entry should be a failure mode the agent might hit.

#### Stop Condition

When to halt and surface to the practitioner instead of continuing. Used for tasks where the work might surface unexpected complexity or ambiguity.

````markdown
**Stop Condition**

If a real Anthropic export reveals shape mismatches with the schema (fields
not in the synthetic fixtures), stop and surface findings before modifying
the data model. Schema decisions belong to the practitioner, not the agent.
````

Especially useful for tasks that include real-data inspection — the inspection itself may produce findings the agent shouldn't act on alone.

---

## Translation moves: from ho's Think phase to executable spec

A ho's Think phase produces decisions. The Execute phase decomposes those decisions into agent tasks. The translation isn't mechanical — it's a craft move. Patterns:

### Move 1: Decisions → interface specifications

A decision like "data model: pydantic v2" becomes a concrete artifact in an agent task: "Define `ConversationRecord(BaseModel)` with fields `id: str`, `source: str`, `messages: list[Message]`, ..." The decision said what; the task says what artifact and where.

### Move 2: Architectural commitments → file boundaries

A commitment like "shuji owns SQLite; bridges and Library Surface read via shuji" becomes file boundaries in the task: "Add to `src/shuji/`, not `src/shodo/api/`." The architectural commitment becomes a literal directory rule.

### Move 3: Out-of-scope items → Do Not entries

The ho's "out of scope" list converts into Do Not entries when the boundary is non-obvious. "ho-01 doesn't include the indexer" doesn't need a Do Not (the file paths make it obvious). "ho-01 uses strict parsing only" might need one (a thoughtful agent could rationalize a lenient mode as helpful).

### Move 4: Deferred discoveries → Stop Conditions

Decisions that defer to execution-time real-data inspection become Stop Conditions. The Think phase said "discovery deferred"; the agent task says "stop and surface if X is encountered."

### Move 5: Verification dependencies → ordering

If task A's verification depends on task B's output, A comes after B in the task sequence. Sometimes the practitioner has to reorder mid-decomposition because a verification dependency surfaces. That's fine.

---

## Standalone agent tasks

Not every agent task is a ho child. Some tasks exist outside the Kamae chain:

- **Maintenance.** "Bump dependency X to version Y, fix any breaks." Doesn't merit a ho.
- **Surgical fixes between hos.** "Fix the typo in CLAUDE.md that's confusing the agent." Tiny, no Kamae context.
- **Pre-ho exploration.** "Inspect a real Anthropic export and report on its schema." The output informs the next ho's Think phase but isn't itself part of any ho.

Standalone tasks use the same format with the standalone naming and frontmatter (above). The `ho-kamae-5-authoring-collaborator` is **not** the entry point for standalone tasks — it's the per-ho authoring skill. Practitioners author standalone tasks directly using this format reference, or use the standalone `dandori` skill at `~/Vaults/sageframe-no-kaji-dev/dandori/`.

---

## Anti-patterns

- **Pseudocode in Required Changes.** Specs that try to write the implementation. The agent does the implementation; the task says what to produce.
- **Acceptance criteria without verification commands.** A criterion the agent can't run a command for is a criterion the agent can't verify.
- **Verification that doesn't match acceptance.** Acceptance says "tests pass," verification doesn't list how to run tests. Mismatch.
- **Do Not entries that aren't real failure modes.** Padding with hypothetical drift. Each Do Not should be a real way the agent might wander.
- **Stop Conditions on every task.** If every task has one, the practitioner is over-supervising. Reserve for tasks that warrant a halt.
- **Tasks that combine multiple goals.** "Implement X and also fix Y while you're in there." Split. One Goal per task.
- **Missing the `parent-ho` frontmatter on a child task.** The link from task to ho is the navigation. Don't drop it.
- **Slugs that don't disambiguate.** `Standalone-AT-2026-05-09-fix.md` tells you nothing. `Standalone-AT-2026-05-09-fix-titler-summary-null-handling.md` does.

---

## Format checklist

Before declaring a task ready:

- [ ] Frontmatter complete (created, type, project, status; parent-ho and task for child tasks)
- [ ] Goal: one to three sentences, unambiguous
- [ ] Files: full paths, create/modify/read-only marked
- [ ] Required Changes: each change is concrete, not pseudocode
- [ ] Acceptance: checkboxes, each one verifiable by a command
- [ ] Verification: commands match acceptance one-to-one
- [ ] Commit: message format specified, `ho-NN:` prefix for child tasks
- [ ] Optional sections (Context, Problem, Do Not, Stop Condition) present only when warranted
- [ ] Naming follows convention (`Ho-NN-AT-MM.md` for child, `Standalone-AT-YYYY-MM-DD-slug.md` for standalone)
- [ ] Slug (for standalone tasks) is descriptive enough to disambiguate
