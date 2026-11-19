# Antagonist review in the Ho process

Antagonist review follows each build ho and a separate final review examines the completed system. Both consume the project's requirements and actual implementation, produce evidence and findings, and preserve the practitioner's authority over intent and accepted risk. The review is a verification responsibility within the existing Kamae chain, not a seventh Kamae document or a new implementation phase.

## Kamae 4: plan the reviews

Place a bounded antagonist review after each build ho in the overview, and place an independent whole-codebase review at the completed-system acceptance point. Where a project already uses `ho-NNx` review labels, preserve that convention; otherwise identify the review alongside its parent ho without inventing a new numbering system. Orientation or research hos are reviewed against their claims and deliverables when their acceptance requires review, rather than being given fictitious code or test requirements.

State the project default of two reviewer passes, or three if the practitioner selects it, and identify who can rule on a Return or incomplete evidence. A final review has its own declared budget and cannot be substituted for an exhausted ho review. Reviews produce reports; they do not authorize publication, deployment, or automatic reopening of closed work.

## Kamae 5: prepare the review packet

When authoring a build ho, include the review's intended acceptance boundary and its place in the close. Once the build endpoint is known, fill the bundled [review context](../assets/review-context.md) from the ho specification, system design, seed, current owner decisions, and actual revisions. Read the selected [ho brief](ho-review.md) or [final brief](final-review.md), together with the [shared standards](standards.md) and [cycle](cycle.md), rather than pasting a second maintained copy of those rules into every project.

For a review-only `ho-NNx` document, use the context packet and report contract; do not invent a new Think phase or implementation work merely to fit a build-ho shape. Preserve the project's existing frontmatter and naming conventions. If the reviewing agent lacks this skill, supply the complete portable skill folder or the relevant documents along with the packet so the review does not depend on a path on another person's machine.

## Project records

Use existing project destinations. For a project without review conventions, these are suggested paths rather than mandatory schema changes:

| Record | Suggested location |
|---|---|
| Ho review context and cumulative cycle state | `ho-process/reviews/ho-NNx-context.md` |
| Each immutable reviewer report | `ho-process/reviews/ho-NNx-report-N.md` |
| Final system review context and reports | `ho-process/reviews/final-context.md`, `final-report-N.md` |
| Authorized repair specification, when needed | An ordinary fix ho under `ho-process/hos/`, following the project's numbering |
| Owner ruling | The cycle ledger or a linked fix-ho decision, with its date and scope |

Keep the original opening revision and record the previous reviewed endpoint for every follow-up. A new fix-ho number, report filename, session, or model does not reset the cycle counter. A wording-only ruling generally needs one decision entry, not a new implementation ho; existing project record requirements still apply.

## Acceptance and continuity

Record build completion and review acceptance separately until both conditions required by the project are satisfied. Preserve the reviewer assessment as issued; if the practitioner accepts a later correction without another review, say so rather than replacing Return with a claimed reviewer Pass. At the pass ceiling, unresolved work remains visible and the reviewer stops for the practitioner.

The coordinator records the result in the existing build record and refreshes the project's State Memory and status roster according to its normal close discipline. The report-only antagonist does not acquire permission to edit those files merely because they exist. Where review is required for acceptance, per-ho authoring must not mark the ho accepted solely because its implementation finished.

## Cycle handoffs

Give the builder the review rules and report together. Use the [handoff guide](handoffs.md) to write a cycle-specific file beside the report and deliver only its short pointer, manually or through authorized agent coordination. Pin the skill version at cycle creation and compare it before each handoff, retaining the same counter and owner rulings. A worktree may isolate repairs; record its actual starting content, repair endpoint, and integration permissions so the next review examines the repaired state.

## Cross-agent use

The skill uses ordinary Markdown, relative references, and a `SKILL.md` with name and description. In Claude Code, invoke `/ho-antagonist-review`; in Codex, invoke `$ho-antagonist-review`. Give the reviewer the project context, mode, pass number, and report destination, and preserve a fresh reviewer context when the host supports one.

Install or link the whole directory into each agent's supported skill location; copying only the entry point omits the review documents. Local installations do not install the skill in a cloud account; use the portable ZIP through that account's skill-upload mechanism when needed. Optional `agents/openai.yaml` supplies Codex display metadata and is not part of the review logic.
