# Antagonist review in the Ho process

Antagonist review follows each build ho and a separate final review examines the completed system. Both consume the project's requirements and actual implementation, produce evidence and findings, and preserve the practitioner's authority over intent and accepted risk. The review is a verification responsibility within the existing Kamae chain, not a seventh Kamae document or a new implementation phase.

## Kamae 4: plan the reviews

Place a bounded antagonist review after each build ho in the overview, and place an independent whole-codebase review at the completed-system acceptance point. Where a project already uses `ho-NNx` review labels, preserve that convention; otherwise identify the review alongside its parent ho without inventing a new numbering system. Orientation or research hos are reviewed against their claims and deliverables when their acceptance requires review, rather than being given fictitious code or test requirements.

State the pass ceiling the project uses and identify who can rule on a Return or incomplete evidence. The [cycle rules](cycle.md) define the default ceiling and its options, the separate final-review cycle, and what a report authorizes.

## Kamae 5: prepare the review packet

When authoring a build ho, include the review's intended acceptance boundary and its place in the close. Once the build endpoint is known, fill the bundled [review context](../assets/review-context.md) from the ho specification, system design, seed, current owner decisions, and actual revisions. Read the selected [ho brief](ho-review.md) or [final brief](final-review.md), together with the [shared standards](standards.md) and [cycle](cycle.md), rather than pasting a second maintained copy of those rules into every project.

For a review-only `ho-NNx` document, use the context packet and report contract; do not invent a new Think phase or implementation work merely to fit a build-ho shape. Preserve the project's existing frontmatter and naming conventions. If the reviewing agent lacks this skill, supply the complete portable skill folder or the relevant documents along with the packet so the review does not depend on a path on another person's machine.

## Project records

Use existing project destinations. For a project without review conventions, these are suggested paths rather than mandatory schema changes. They take effect when the owner designates them in the review context; until then the default destination outside the repository applies:

| Record | Suggested location |
|---|---|
| Ho review context and cumulative cycle state | `ho-process/reviews/ho-NNx-context.md` |
| Each immutable reviewer report | `ho-process/reviews/ho-NNx-report-N.md` |
| Final system review context and reports | `ho-process/reviews/final-context.md`, `final-report-N.md` |
| Authorized repair specification, when needed | An ordinary fix ho under `ho-process/hos/`, following the project's numbering |
| Owner ruling | The cycle ledger or a linked fix-ho decision, with its date and scope |

Keep the original opening revision and record the previous reviewed endpoint for every follow-up. A new fix-ho number is a label like any other under the [cycle rules](cycle.md); it does not reset the counter. A wording-only ruling generally needs one decision entry, not a new implementation ho; existing project record requirements still apply.

## Acceptance and continuity

Record build completion and review acceptance separately until both conditions required by the project are satisfied. The reviewer's assessment and the practitioner's acceptance stay distinct entries, as [cycle.md](cycle.md#assessment-and-workflow-state) defines.

The coordinator records the result in the existing build record and refreshes the project's State Memory and status roster according to its normal close discipline. The report-only antagonist does not acquire permission to edit those files merely because they exist. Where review is required for acceptance, per-ho authoring must not mark the ho accepted solely because its implementation finished.

## Changes after acceptance

Material behavior added after an accepted endpoint needs its own bounded change-set review when review is part of project acceptance. Place that review before integration or release unless the owner explicitly selects deferred review; preserve that decision and outstanding status. Judge the scope by affected behavior and callers, not line count, and do not turn a small change review into a fresh whole-repository audit. Earlier Pass records remain tied to their original endpoints. An explicit owner waiver of re-review is also a valid acceptance decision; record the added range, focused verification, and remaining limits without describing the later endpoint as reviewer-passed or leaving a waived review falsely pending. A small change can require checking its actual dispatch boundary while still needing no new broad test campaign.

## Cycle handoffs

Handoffs between reviewer, builder, and coordinator, instruction pinning, and repair worktrees follow the [handoff guide](handoffs.md). Nothing about them is specific to a Ho project.

## Cross-agent use

Installation and invocation in Claude Code, Codex, and other agents are described in the [README](../README.md#install).
