---
created: 2026-05-09
type: agent-task
project: shuji
parent-ho: 01
task: 02
model: claude-sonnet-4-6
status: ready
---

**Goal**

Implement `AnthropicParser` — a parser plugin that reads Anthropic conversation exports and yields validated `ConversationRecord` objects. Register it in the `PARSERS` list. Cover with unit tests against synthetic fixtures.

**Context**

The parser plugin protocol was decided in Ho-01-AT-01 (see `ho-process/hos/ho-01-schema-and-parser.md` Decision 1). Each parser implements `can_parse` (cheap dispatch check) and `parse` (the work). Strict error handling was chosen over lenient mode in v1; see ho-01 Decision 2.

The synthetic fixtures should mirror the structure of a real Anthropic export but with redacted content. A separate exploration task (`Standalone-AT-2026-05-02-anthropic-export-shape.md`) already documented the export's shape.

**Files**

- Create: `src/shuji/parsers/anthropic.py`
- Create: `tests/parsers/test_anthropic.py`
- Create: `tests/fixtures/anthropic/synthetic_5.json` (synthetic 5-conversation fixture)
- Create: `tests/fixtures/anthropic/synthetic_malformed.json` (intentionally broken fixture for error-path tests)
- Modify: `src/shuji/parsers/__init__.py` (add `AnthropicParser` to the `PARSERS` list)

**Required Changes**

1. **Define `AnthropicParser` class** in `src/shuji/parsers/anthropic.py`. Implements the `Parser` protocol from `src/shuji/parsers/base.py`.
   - `can_parse(path: Path) -> bool` returns True if the file is a JSON array of objects with the expected Anthropic export shape (top-level array, each item has `uuid`, `name`, `chat_messages` keys).
   - `parse(path: Path) -> Iterator[ConversationRecord]` yields one `ConversationRecord` per conversation. Raise `MalformedExportError` (already defined in `base.py`) on the first malformed entry. No lenient mode.

2. **Register in `PARSERS`** in `src/shuji/parsers/__init__.py`. Append `AnthropicParser` to the existing list.

3. **Synthetic fixtures** in `tests/fixtures/anthropic/`.
   - `synthetic_5.json`: five well-formed conversations. Vary length, message count, and timestamp range.
   - `synthetic_malformed.json`: one well-formed conversation followed by one missing the `chat_messages` key.

4. **Unit tests** in `tests/parsers/test_anthropic.py`. Cover:
   - `can_parse` returns True for the synthetic fixtures, False for unrelated JSON.
   - `parse` yields five records for `synthetic_5.json`, each a valid `ConversationRecord`.
   - `parse` raises `MalformedExportError` on `synthetic_malformed.json` after yielding the first record.
   - `AnthropicParser` is in the `PARSERS` list when imported from `shuji.parsers`.

**Acceptance**

- [ ] `AnthropicParser` class exists in `src/shuji/parsers/anthropic.py` and implements the Parser protocol.
- [ ] All unit tests in `tests/parsers/test_anthropic.py` pass.
- [ ] `AnthropicParser` is in the `PARSERS` list when imported from `shuji.parsers`.
- [ ] Linting clean (ruff).
- [ ] Type checking clean (mypy strict).
- [ ] No new dependencies added beyond what `pyproject.toml` already declares.

**Verification**

```bash
# Run unit tests
uv run pytest tests/parsers/test_anthropic.py -v

# Lint and type check
uv run ruff check src/shuji/parsers/anthropic.py tests/parsers/test_anthropic.py
uv run mypy src/shuji/parsers/anthropic.py

# Smoke test the registration
uv run python -c "from shuji.parsers import PARSERS; assert any(p.__name__ == 'AnthropicParser' for p in PARSERS)"

# Manual check: review the diff to confirm no new top-level dependencies were added.
git diff pyproject.toml
```

**Do Not**

- Do not add lenient parsing modes. Strict was decided in ho-01 Decision 2.
- Do not modify `ConversationRecord`'s public fields. The model is shared with the indexer; changing it has cross-ho implications.
- Do not add a generic "export inspection" CLI. That belongs to a separate task if needed.

**Commit**

Single commit. Message format:

```
ho-01: add AnthropicParser

Implement AnthropicParser per the Ho-01-AT-02 spec. Yields ConversationRecord
from Anthropic conversation exports. Strict error handling — raises
MalformedExportError on the first malformed entry.

Tests: 6 passing.
```
