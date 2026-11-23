# Review report template

Use the applicable sections below, keeping a follow-up report focused on what changed. References to stable prior evidence are preferable to copying an entire previous report. Do not omit missing evidence in order to make the assessment appear complete.

```markdown
# Review of PROJECT OR UNIT

Assessment: Pass / Return / Incomplete
Reason: the decisive finding or acceptance limitation in plain language.

Cycle: ID; mode; cumulative pass NUMBER; normal ceiling or owner exception; remaining budget/stopping condition
Reviewed state: exact revision or immutable snapshot
Governing instructions: pinned commit/subdirectory or snapshot/manifest
Original scope: opening-to-current range for a ho; inventory for a final review
This pass: full review / repair range and affected boundaries
Requirements and owner decisions: exact documents and relevant revisions
Independence: fresh reviewer / shared builder context, with limitation

## Findings

### F-ID — Concrete defect or decision

- Kind; severity; confidence; disposition
- Location: revision, file, line, symbol
- Requirement or concrete maintenance cost:
- Trigger and reachability:
- Expected and actual behavior:
- Evidence: reproduced / static / prior checked / builder claim / hypothesis
- Consequence and scope:
- Corrective outcome or owner decision:
- Behavior change: none / described change
- Follow-up origin: repair regression / unresolved cause / newly reachable / missed earlier

Use “none” if there are no findings. Keep incidental polish out of the blocking list.

## Verification

| Revision and environment | Command or procedure | Result and exit | Counts or measured scope | Evidence path |
|---|---|---|---|---|

Record failed attempts, justified retries, reused evidence, and what each did not establish.

## Scope and acceptance coverage

| Requirement, component, or flow | Inspected implementation | Evidence | Remaining gap or exclusion |
|---|---|---|---|

For final review, include the repository inventory and inspection depth by area.
For follow-up, reference the initial scope map and record affected areas and changes.

## Finding ledger

| ID | Previous state | Current evidence | Disposition | Owner ruling or next action |
|---|---|---|---|---|

## Closure

Reviewer assessment and unresolved acceptance questions:
Workflow state and remaining review budget:
Owner decisions already recorded, if any:
Recommended next action and recipient:
Handoff status: prepared / dispatched by authorized coordinator / awaiting owner

## Handoff

Link to the separate pass-N-handoff.md beside this report, written as
handoffs.md defines. Do not embed its contents or the dispatch prompt here.
```

## Builder response

For each finding being addressed, supply its ID, whether accepted or disputed, the causal change, and affected files/revision. Record the evidence in a compact row or equivalent prose:

| Finding / claimed outcome | Observable witness and result | Actual boundary exercised; substituted or bypassed component | Remaining limit or owner decision |
|---|---|---|---|

When the same finding survives two repair attempts, include the compact invariant/transition map required by [cycle.md](cycle.md#new-evidence-and-reopened-decisions), with any untested routes identified. Distinguish production failures from probe/build failures and label probes that assert the old defect rather than the corrected outcome.

The witness must distinguish the claimed behavior from a plausible incorrect implementation; otherwise narrow the claim or record the gap. Do not repeat suite logs or manufacture a new test for a claim already established by appropriate evidence. A claim that a test would fail on old code must say whether that was actually executed or is a static argument. Include the repair directory, original and repaired content identities, and integration state. Finish with the next authorized handoff as [handoffs.md](handoffs.md) defines.

## Owner decision entry

Record the date, finding ID, decision, rationale, changed requirement or accepted risk, affected scope, and the evidence being relied on. If the decision closes work without re-review, say so explicitly and retain the reviewer's assessment. If it authorizes a new pass or an exception to the cap, record it before execution with the fields [cycle.md](cycle.md#the-normal-sequence) requires.
