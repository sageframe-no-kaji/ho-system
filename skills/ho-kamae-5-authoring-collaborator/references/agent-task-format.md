# Agent Task Format

A reference for the agent task format — the surgical spec the agent reads to execute one bounded unit of work. Loaded by `ho-kamae-5-authoring-collaborator` when the Think phase reveals the work decomposes into bounded tasks. Also usable standalone when a practitioner authors a task outside any parent ho (maintenance, exploration, surgical fixes between hos).

The decision about *whether* to decompose into tasks lives in `ho-kamae-5-authoring-collaborator`'s SKILL.md. This file describes what the format is, how the agent operates against it, and how the translation from intent to executable spec works.

Agent tasks are surgical specs — the agent reads one and produces a defined output, with explicit acceptance criteria and verification steps. Children of hos by default (sibling directory, fixed naming convention); standalone tasks use the same format with a small naming and frontmatter adjustment.

---

## Behavioral context

Agent tasks are executed by an agent operating under a small set of behavioral guidelines. The task format assumes these are in play — typically loaded via the project's `CLAUDE.md` or equivalent. The format works to the extent the agent honors them.

### 1. Think before coding

State assumptions explicitly. If multiple interpretations of the spec exist, surface them rather than picking silently. If a simpler approach exists, say so. If something is unclear, stop and ask before implementing. Don't hide confusion.

### 2. Simplicity first

Minimum code that meets the spec. No features beyond what was asked. No abstractions for single-use code. No "flexibility" or "configurability" that wasn't requested. No error handling for impossible scenarios. If 200 lines could be 50, rewrite. Senior-engineer test: "Would they say this is overcomplicated?"

### 3. Surgical changes

Touch only what the spec names. Don't "improve" adjacent code, comments, or formatting. Don't refactor things that aren't broken. Match existing style even if you'd do it differently. If unrelated dead code surfaces, mention it — don't delete it. Every changed line should trace to the spec.

When changes create orphans: remove imports, variables, functions that the spec's changes made unused. Don't remove pre-existing dead code unless the spec asks.

### 4. Goal-driven execution

The task's Acceptance section defines success. Loop until each criterion verifies. Transform vague tasks into verifiable goals — "add validation" becomes "write tests for invalid inputs, then make them pass." For multi-step changes, state a brief plan with verification per step.

Strong success criteria let the agent loop independently. Weak criteria ("make it work") require constant clarification — that's a failure mode of the spec, not the agent.

### Source

These guidelines are derived from Andrej Karpathy's behavioral framing for LLM coding work, packaged by Forrest Chang at <https://github.com/forrestchang/andrej-karpathy-skills/blob/main/CLAUDE.md>. They bias toward caution over speed; for trivial tasks, judgment overrides.

The task format below assumes these are loaded. When they are not, the task spec compensates — more explicit `Do Not` items, more conservative `Required Changes`, stricter `Acceptance` — but the cleaner pattern is to load the guidelines once at the project level and let the spec stay tight.

---

## Naming and location

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

---

## Format

Agent tasks have required sections (always present, even if brief) and optional sections (included when warranted). Order matters — agents read top-to-bottom.

### Required sections

#### Frontmatter

```yaml
---
created: YYYY-MM-DD
type: agent-task
project: <slug>
parent-ho: <NN>           # omit for standalone tasks
task: <MM>                # omit for standalone tasks
status: ready             # ready | in-progress | complete | blocked
---
```

#### Goal

One to three sentences. What this task produces. Concrete enough that "done or not done" is unambiguous.

Bad: "Improve the parser."
Good: "Implement `AnthropicParser` that reads Anthropic conversation exports and yields validated `ConversationRecord` objects, with full unit test coverage against synthetic fixtures."

#### Files

A bullet list of files the task creates or modifies. Use absolute paths from repo root. Indicate create vs modify.

```markdown
**Files**

- Create: `src/shuji/parsers/anthropic.py`
- Create: `tests/parsers/test_anthropic.py`
- Create: `tests/fixtures/anthropic/synthetic_5.json`
- Modify: `src/shuji/parsers/__init__.py` (add to `PARSERS` list)
```

If the file list is large, group by area. If a file is created and later modified within the same task, list it once with the more substantial action.

#### Required Changes

The substantive section. What the agent does, in enough detail that the work is unambiguous but not so much that it becomes pseudocode. Each change is its own subsection or numbered item.

For each change:
- **What** — the concrete artifact (function signature, test name, schema column).
- **Why** — one line, only if not obvious from context.
- **How** — only if the approach isn't obvious. Default to leaving how to the agent.

Example:

```markdown
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
```

The agent reads this and knows exactly what artifacts to produce. The agent does not need to interpret architectural intent — that work happened in the ho's Think phase.

#### Acceptance

The pass/fail criteria. The agent verifies these before declaring the task complete.

```markdown
**Acceptance**

- [ ] `AnthropicParser` class exists in `src/shuji/parsers/anthropic.py` and implements the Parser protocol.
- [ ] All unit tests in `tests/parsers/test_anthropic.py` pass.
- [ ] `AnthropicParser` registers in the `PARSERS` list.
- [ ] Linting clean (ruff, mypy).
- [ ] No new dependencies added beyond what `pyproject.toml` already declares.
```

Use checkboxes — they're machine-readable and the practitioner can scan them.

#### Verification

The commands the agent runs to verify acceptance. Concrete, copy-pasteable.

```markdown
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
```

The verification commands match the acceptance criteria one-to-one. If a criterion can't be verified by a runnable command, name the manual check explicitly.

#### Commit

The agent's commit instruction. Brief — message format and any commit-time discipline.

```markdown
**Commit**

Single commit. Message format:

```
ho-01: add AnthropicParser

Implement AnthropicParser per Ho-01-AT-02 spec. Yields ConversationRecord
from Anthropic conversation exports. Strict error handling; no lenient mode.

Tests: 9 passing.
```
```

### Optional sections

Include when warranted; omit when not.

#### Context

A few sentences if the task can't be understood from Goal alone. Reserved for cases where the parent ho's Think phase produced background the agent needs.

```markdown
**Context**

The parser plugin protocol was decided in Ho-01-AT-01. Each parser implements
`can_parse` (cheap dispatch check) and `parse` (the work). Strict error handling
was chosen over lenient mode in v1 — see ho-01 Decision 2.
```

Used when the task references decisions made elsewhere that the agent can't see.

#### Problem

For ri-shaped hos or maintenance tasks: what's broken, what's the symptom. Skipped for ha-shaped hos where the Goal section already states the work.

```markdown
**Problem**

The `summary` column was added in ho-01 but never populated. ho-05's titler
expects to read summaries from existing rows, so unpopulated rows raise.
Backfill needed.
```

#### Do Not

Explicit out-of-scope items the agent might otherwise wander into. Reserved for cases where the boundary is non-obvious.

```markdown
**Do Not**

- Do not add a migration framework. Schema changes are ALTER TABLE in the ho
  that introduces them, per ho-01 Decision 4.
- Do not modify `ConversationRecord`'s public fields. The model is shared with
  the indexer; changing it has cross-ho implications.
- Do not add lenient parsing modes. Strict was decided in ho-01 Decision 2.
```

The Do Not section is a guard against drift. Use sparingly — every entry should be a real failure mode the agent might hit.

#### Stop Condition

When to halt and surface to the practitioner instead of continuing. Used for tasks where the work might surface unexpected complexity or ambiguity.

```markdown
**Stop Condition**

If a real Anthropic export reveals shape mismatches with the schema (fields
not in the synthetic fixtures), stop and surface findings before modifying
the data model. Schema decisions belong to the practitioner, not the agent.
```

Especially useful for tasks that include real-data inspection — the inspection itself may produce findings the agent shouldn't act on alone.

---

## Translation moves: from ho intent to executable spec

The Think phase makes decisions. The Execute phase decomposes those decisions into agent tasks. The translation isn't mechanical — it's a craft move. Some patterns:

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
- **Pre-ho exploration.** "Inspect a real Anthropic export and report on its schema." The output informs ho-01's Think phase but isn't itself part of ho-01.

Standalone tasks use the same format. They live in the same `ho-process/agent-tasks/` directory. Naming convention adjusts:

- Use a descriptive prefix instead of `Ho-NN-AT-MM`. Examples: `Standalone-AT-2026-01-15-bump-fastapi.md`, `Exploration-AT-anthropic-export-inspect.md`.
- Frontmatter omits `parent-ho` and `task`; adds `type: standalone-agent-task` to be explicit.

The `ho-kamae-5-authoring-collaborator` skill is **not** the entry point for standalone tasks. Practitioners author standalone tasks directly using this format reference. If a practitioner asks for help authoring a standalone task, point them at this file and offer to draft.

---

## Note on possible skill extraction

This reference is intentionally portable. It contains three things that travel together: the behavioral guidelines (Think before coding, Simplicity first, Surgical changes, Goal-driven execution), the format conventions, and the translation moves from intent to executable spec. None of them are specific to the Ho System — they describe how an agent operates against bounded specs in any project.

If the standalone-task use case grows enough to merit it, this reference can extract into its own skill (`agentic-prompt` or similar) that handles task authoring outside the Kamae chain. Until that reuse pressure shows up, it lives here as a reference loaded conditionally by `ho-kamae-5-authoring-collaborator`. Either way, the file is structured so extraction is a copy operation, not a rewrite.

---

## Anti-patterns

- **Pseudocode in Required Changes.** Specs that try to write the implementation. The agent does the implementation; the task says what to produce.
- **Acceptance criteria without verification commands.** A criterion the agent can't run a command for is a criterion the agent can't verify.
- **Verification that doesn't match acceptance.** Acceptance says "tests pass," verification doesn't list how to run tests. Mismatch.
- **Do Not entries that aren't real failure modes.** Padding with hypothetical drift. Each Do Not should be a real way the agent might wander.
- **Stop Conditions on every task.** If every task has a Stop Condition, the practitioner is over-supervising. Reserve for tasks that genuinely warrant a halt.
- **Tasks that combine multiple goals.** "Implement X and also fix Y while you're in there." Split. One Goal per task.
- **Missing the parent-ho frontmatter on a child task.** The link from task to ho is the navigation. Don't drop it.

---

## Format checklist

Before declaring a task ready:

- [ ] Frontmatter complete (parent-ho if child, status, dates)
- [ ] Goal: one to three sentences, unambiguous
- [ ] Files: full paths, create/modify marked
- [ ] Required Changes: each change is concrete, not pseudocode
- [ ] Acceptance: checkboxes, each one verifiable
- [ ] Verification: commands match acceptance one-to-one
- [ ] Commit: message format specified
- [ ] Optional sections (Context, Problem, Do Not, Stop Condition) present only when warranted
