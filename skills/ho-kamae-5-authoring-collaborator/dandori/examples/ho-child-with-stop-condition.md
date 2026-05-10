---
created: 2026-05-09
type: agent-task
project: shuji
parent-ho: 04
task: 01
status: ready
---

**Goal**

Inspect the real Anthropic conversation export at `~/exports/anthropic-2026-04.json` and report on its structural shape — top-level layout, per-conversation fields, message-level fields, edge cases, anything unexpected. Produce a written summary the practitioner uses during ho-04's Think phase to decide whether the existing parser schema needs revision.

**Problem**

The existing `AnthropicParser` (built in ho-01) was implemented against synthetic fixtures derived from documentation, not against real export data. The synthetic fixtures are five conversations; the real export is several thousand. ho-04 starts by needing to know whether the real export contains shapes the fixtures don't represent — attachments, tool calls, system messages, branching, edited messages — but no inventory exists yet. This task produces that inventory.

**Files**

- Read-only: `~/exports/anthropic-2026-04.json` (the export to inspect)
- Read-only: `src/shuji/parsers/anthropic.py` (the current parser, to compare against)
- Read-only: `tests/fixtures/anthropic/synthetic_5.json` (the synthetic fixture, for comparison)
- Create: `ho-process/agent-tasks/findings/Ho-04-AT-01-anthropic-export-shape.md` (the report)

**Required Changes**

This task produces a report, not code. The report covers:

1. **Top-level structure.** Is the file a JSON array, an object, something else? What are the top-level keys (if an object)?

2. **Per-conversation fields.** What fields appear on each conversation? Which are always present, which are sometimes present, which were missing from the synthetic fixture?

3. **Per-message fields.** What fields appear on each message? Specifically: are there message types beyond `human` and `assistant`? Are there attachments? Tool calls? Edited or branched messages?

4. **Distributional notes.** Rough counts: total conversations, total messages, range of message-counts-per-conversation, range of timestamp values.

5. **Schema gaps.** Anything in the real data that the current `AnthropicParser` would mishandle, miss, or fail on. Be specific — name the field and the expected failure mode.

6. **Recommendations.** For each schema gap, the agent's suggestion for what to do — but framed as a recommendation for the practitioner's ho-04 Think phase, not as something to act on.

The report goes in `ho-process/agent-tasks/findings/Ho-04-AT-01-anthropic-export-shape.md`. Use markdown. Include short JSON snippets where they clarify a structural point, but redact any conversation content (replace text with `[redacted]`).

**Acceptance**

- [ ] Report file exists at `ho-process/agent-tasks/findings/Ho-04-AT-01-anthropic-export-shape.md`.
- [ ] All six sections from Required Changes are present and non-empty.
- [ ] Conversation content is redacted in any JSON snippets.
- [ ] No code changes were made to `src/shuji/parsers/anthropic.py` or anything else outside the findings directory.

**Verification**

```bash
# Confirm the report exists and has the expected sections
test -f ho-process/agent-tasks/findings/Ho-04-AT-01-anthropic-export-shape.md
grep -c "^## " ho-process/agent-tasks/findings/Ho-04-AT-01-anthropic-export-shape.md  # should print 6 or more

# Confirm no code was modified
git status src/ tests/   # should show no changes

# Sanity-check redaction (no obvious unredacted text)
grep -E '"text":\s*"[^[]' ho-process/agent-tasks/findings/Ho-04-AT-01-anthropic-export-shape.md   # should print nothing
```

**Do Not**

- Do not modify the parser. Schema decisions belong to the ho-04 Think phase, not the agent.
- Do not modify the synthetic fixture to match the real export. The fixture's purpose is to test what the parser claims to handle; changing the fixture without changing the parser hides the gap.
- Do not include unredacted conversation text in the report. Redact aggressively; if in doubt, redact.

**Stop Condition**

If the export contains structural shapes that suggest the `ConversationRecord` model itself needs revision (not just the parser), stop after documenting the finding. Surface it explicitly in the report's "Schema gaps" section and flag it for practitioner review during ho-04 Think. Model changes have cross-cutting implications and are not for the agent to propose acting on.

**Commit**

Single commit. Message:

```
ho-04: anthropic export shape inspection

Inspect ~/exports/anthropic-2026-04.json against the current AnthropicParser
schema, per the Ho-04-AT-01 spec. Report at
ho-process/agent-tasks/findings/Ho-04-AT-01-anthropic-export-shape.md.
No code changes; recommendations only — input to ho-04 Think phase.
```
