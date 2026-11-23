# Bounded review cycle and owner decisions

Each cycle has one identity, an immutable starting point, a current endpoint, and a declared ceiling of two or three reviewer passes. The default is two; a third is available when selected in the context or explicitly authorized by the owner. The reviewer never treats a new chat, a fix-pass label, a renamed report, or a different reviewer as a reset of that ceiling.

## Roles

| Role | Responsibility |
|---|---|
| Builder | Implements authorized work and supplies evidence, without declaring its own work independently reviewed. |
| Antagonist | Reads the work, verifies claims, reports findings and acceptance state; makes no implementation changes. |
| Coordinator | Reconciles findings, arranges authorized fixes, maintains the ledger and budget, and supplies the next review context. |
| Owner | Rules on intent, accepted risk, scope, architecture, and any exception to the review budget. |

The owner is the person the work is for. The agent that starts and runs a cycle is its coordinator, and the owner may also coordinate. Only the owner changes the pass ceiling, up or down. An agent's statement that the owner approved a change does not count without the owner's recorded instruction.

These are responsibilities, not a requirement to spawn four agents or use different vendors. The builder receives the cycle rules as well as the report; the reviewer remains independent of repairs. Nothing in this file authorizes delegation, extra chats, external actions, or implementation: [handoffs.md](handoffs.md#authorization-and-transport) covers how the user grants those.

## Count passes and execution honestly

A reviewer invocation that performs substantive review and issues an assessment consumes one pass, including an incomplete assessment. A preflight is blocked when the reviewer stops without an assessment because a required input is missing; it does not consume a pass. Once the reviewer reports any finding or assessment, the pass is consumed. Record the blocker and retry the preflight at most once after the blocker is reported resolved; a second block on the same cause returns the cycle to the owner. Verification commands and bounded probes within a pass are not additional passes. An explicitly authorized completion of a previously excluded check on unchanged reviewed content may be a dated evidence addendum, not another substantive pass. Preserve the earlier result and limits, record the added execution and its effect on assessment, and never use an addendum to disguise a new endpoint or another broad review.

Verification runs and retries follow [standards.md](standards.md#limits-on-execution). Changing labels from review to verification or dispatching a second reviewer to repeat the analysis does not evade the pass budget.

## The normal sequence

1. Prepare the context and pin both the reviewed state and the governing skill version under [handoffs.md](handoffs.md). Record current owner decisions, accepted limitations, verification obligations, and the pass ceiling.
2. Run pass 1 over the full ho scope or the full final-review scope. Consolidate findings before handing them over.
3. Reconcile each finding against the code and evidence. A technically false finding is rejected with a counterexample or explanation; a real but accepted limitation receives an owner ruling; an ambiguous requirement receives a decision.
4. If repairs are authorized, the builder makes them and returns a short finding-to-change map, evidence, and residual limits. Judge the causal fix and the relevant invariant, not merely whether the original example now passes.
5. Run pass 2 over the repairs, unresolved items, and their direct consequences. Retain the original context without rereading every unrelated file for a new set of preferences. When pass 1 was Incomplete because declared scope went unexamined, pass 2 first covers that scope under the pass-1 rules, then the repairs. Scope still unexamined at the ceiling is listed as not reviewed, and the cycle cannot Pass.
6. If a third pass is available and needed, use it for remaining findings and direct regressions. After that report, stop. Recommend owner action when work remains; do not launch a fourth pass.

A Pass ends the normal cycle immediately, even if a pass remains; an unused pass is not work to be performed. An unresolved owner decision pauses dependent work; authorized independent repairs may continue. Incomplete calls for the missing evidence or decision, not an unchanged rerun; review resumes only after that input is resolved and a pass remains. Each report ends with the handoff that [handoffs.md](handoffs.md) defines.

A finding, a Return, an Incomplete, or a recommendation authorizes nothing by itself: no repair, reopened ho, scope or architecture change, integration, or further pass. Each needs the owner's instruction or an authorization the owner has already recorded, and an ordinary repair request does not cover reopening work or changing architecture.

The owner can explicitly authorize further work after the stop. Record the instruction, the affected cycle, the new limit or stopping condition, and whether its scope is this cycle, this project, or a broader explicitly stated policy. Do not promote a cycle-specific exception to a standing rule or interpret “until done” as permission to repeat an unchanged blocked attempt. Keep the cumulative count, stop each reviewer after one assessment, and do not imply that an owner must accept unreviewed corrections merely to preserve the default ceiling. A separately scoped follow-up carries relevant unresolved findings forward; it does not erase them. A fresh comprehensive final review is a planned independent cycle, not a loophole for extending an exhausted ho cycle.

## Assessment and workflow state

Record the reviewer's assessment separately from workflow and owner action:

| Reviewer assessment | Meaning |
|---|---|
| Pass | Required scope and evidence are complete, with no unresolved Must-fix or acceptance-blocking decision. Nonblocking limitations may remain. |
| Return | At least one established Must-fix remains. Also name any incomplete evidence or decisions that independently prevent acceptance. |
| Incomplete | No established Must-fix is being reported, but missing access, unfinished required verification, or an unresolved requirement prevents a justified Pass. |

Workflow states are ready, reviewing, awaiting fixes, awaiting decision, stopped, or closed. Return identifies work or a ruling needed. Incomplete is not a code-defect verdict.

Owner acceptance is a separate entry: accepted as reviewed, accepted with named limitations, accepted after a specified correction without independent re-review, or not accepted. Preserve the reviewer's original report. An owner can close work at the ceiling or accept a later correction with evidence, but the record must not say the reviewer passed an endpoint it never examined.

## Close by ruling without another full review

When the owner resolves only a preference, requirement wording, or accepted risk, reconcile that decision in the ledger against the existing report. Confirm that code, tests, build configuration, dependencies, and other executable inputs are unchanged. Reuse the existing verification evidence with its exact revision and explain why it remains applicable; do not rerun the full suite solely to obtain a differently worded verdict.

Documentation changes still need focused verification when they change a factual claim, command, operational procedure, or public promise. Some documentation is executable, so classify by behavior rather than extension.

## New evidence and reopened decisions

Do not suppress a consequential defect discovered late. Record its severity, origin, and relationship to the earlier review, then apply the same budget and owner-decision rules. Conversely, an unrelated preference found in a later pass belongs in a nonblocking observation or future audit, not a newly enlarged repair contract.

When the same finding survives two repair attempts, stop prescribing another local patch until the builder has mapped the affected invariant in the existing response. Name the state or resource being owned, every relevant writer and consumer, and the reachable transitions: successful action, no-op/early return, replacement, failure, retry/repeated request, and shutdown where applicable. Explain why each route preserves the invariant or remains unresolved; use focused witnesses for the disputed routes rather than adding a new full-repository audit. The cause may be the code, an impossible requirement, an incorrect recipe, or an inadequate witness. This analysis uses the existing repair authority and does not authorize an extra reviewer pass. Ask the owner only for actual decisions; use already-authorized technical work and read-only checks to make the alternatives concrete.

## Minimal record

Keep one cycle ledger with pass number, endpoint, governing instruction version for that pass, report, assessment, required verification, finding dispositions, owner rulings, and remaining budget or explicit exception. Preserve previous version entries when instructions change; a later snapshot must not be attributed to an earlier pass. Update a finding under its stable ID; use aliases when earlier reports reused IDs. A brief repair response and an owner decision entry usually suffice; do not require a new hierarchy of specifications and status files for a one-line clarification unless the project itself requires it.
