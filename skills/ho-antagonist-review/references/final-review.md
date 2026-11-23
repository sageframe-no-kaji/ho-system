# Independent final codebase analysis

The final review examines the completed system as a whole, including interactions and accumulated decisions that individual ho reviews could not establish. Earlier Pass reports are evidence about earlier units, not proof of current system correctness. The reviewer starts from the system's intended behavior and the actual repository at a pinned revision, then uses earlier reports to reconcile unresolved risks and avoid losing accepted decisions.

Use the [shared standards](standards.md), [cycle rules](cycle.md), and completed [review context](../assets/review-context.md). A final review is a separate cycle with its own budget under those rules. It may recommend reopening a ho or architectural decision, and never starts a ho review itself.

## Establish what comprehensive means

Inventory the whole repository before choosing inspection depth. Identify first-party production code, tests, scripts, dependency manifests and locks, configuration, schema or migration files, release and deployment definitions, user and operator documentation, and generated or vendored material. Include history or installed/released artifacts when the review's requirements concern them; do not claim a history or release audit from checking the current source tree alone.

Assign each area an explicit status: read in full, traced by execution path, sampled with rationale, excluded with rationale, or not examined. For a manageable codebase, read all relevant first-party source and operational definitions. For a larger system, cover every component and material boundary with declared depth; do not conceal unexamined areas behind the word comprehensive, and report an incomplete assessment if the required scope cannot be covered within the authorized budget.

Read the current requirements, design, and binding owner decisions, then form an independent map of implementation and critical flows. Before completing the report, consult earlier ho reports and accepted limitations to check repairs, cumulative risk, and missing evidence. Preserve the distinction between current verification and historical observations.

## Assess the system

| Area | Required analysis where applicable |
|---|---|
| Product completeness | Trace each mandatory system outcome to its implementation and evidence. Find missing paths, contradictory promises, unfinished scaffolding, and advertised features that do not exist. |
| Integration | Follow end-to-end flows across components, including ordering, shared state, units, data formats, error propagation, and user-visible completion. A passing unit test is insufficient evidence for the connecting boundary. |
| Resource ownership | Trace creation, use, cancellation, release, retry, and shutdown of processes, tasks, handles, storage, transactions, and subscriptions. Check boundedness under slow consumers, large inputs, and partial failure. |
| Data integrity | Inspect persistence semantics, atomicity, migrations, rollback, backup and recovery assumptions, validation, and compatibility with existing data. Use synthetic or isolated data for authorized probes. |
| Security and privacy | Identify trust boundaries, permissions, authentication, authorization, untrusted inputs, output encoding, secret handling, logging, data retention, and network behavior. Distinguish actual credentials or exposure from labels, benign identifiers, and documented local addresses. |
| Failure and recovery | Examine unavailable dependencies, timeouts, interrupted work, retries, partial completion, stale results, and restart behavior. Check that errors are neither concealed nor reported as successful completion. |
| Performance and limits | Evaluate required latency, memory, throughput, and scale against reachable workloads. Separate endpoint measurements from peaks, benchmarks from estimates, and source-level bounds from caller assumptions. |
| Test and verification integrity | Review whether tests exercise the system's promises, whether coverage excludes the risky boundaries, whether gates fail when evidence is missing, and whether fixtures conceal failures. Record unexplained flakes. |
| Accumulated complexity | Find conflicting sources of truth, overlapping capabilities, obsolete state, unused dependencies, temporary scaffolding, and inconsistent ownership across hos. Explain the maintenance consequence before proposing consolidation. |
| Build and operations | Inspect clean-checkout reproducibility, supported environments, dependency assumptions, configuration defaults, artifact composition, installation, upgrade, rollback, and operational diagnostics. Reading a release script does not prove the release artifact works. |
| Documentation and public claims | Check installation commands, examples, privacy statements, supported inputs, limitations, release conventions, and operator instructions against current behavior. A documentation-only change can alter a consequential promise. |
| Architecture | Test whether the combined design satisfies its invariants. Recommend reopening a decision when concrete evidence shows it does not; name affected boundaries and tradeoffs without performing a redesign. |

Mark an area inapplicable when the system has no such responsibility, with a short reason. Do not invent authentication, a deployment platform, a database, or a dependency policy merely because the checklist names them. Project constraints define the system under examination.

## Prove critical flows

Choose representative end-to-end flows from actual product requirements, with ordinary, boundary, and failure variants suited to their risk. Inspect and, when authorized, execute the whole path through the relevant components rather than stopping at mocked interfaces. Attach the observed result and the boundaries not exercised to each claimed outcome.

For critical asynchronous or persistent flows, apply the [successive-operation rule](standards.md#verification-and-test-quality): for example a second save after a successful first save, cancellation followed by a new request, or an old completion arriving after navigation.

A missing operational witness remains an evidence gap even if all source-level tests pass. Where execution is prohibited or unavailable, use static evidence to establish what it can and identify the exact remaining acceptance question. Do not request an external action merely to make the report look complete when the current task authorizes only analysis.

## Findings and recommendations

Use the same finding model as ho reviews. Group findings by underlying cause and affected system outcome; distinguish implementation fixes, requirement corrections, evidence work, and proposals to reopen decisions. Existing owner acceptance remains in the ledger, but the review can recommend reconsideration if system-wide interaction or changed exposure supplies new evidence.

Prioritize the smallest useful corrective outcome and explain behavior changes. Do not provide a speculative replacement design or a list of preferred frameworks. A final review can expose a defect in a component that passed its ho review without declaring that every earlier review was worthless.

## Follow-up and closure

Pass 1 is the independent full review. Later passes follow the [normal sequence](cycle.md#the-normal-sequence), checking the approved fixes against the affected system flows. Keep the scope map visible so the narrowed follow-up does not erase the record of what was or was not covered initially.

Report system acceptance separately from release or deployment authorization. A source review can pass within its declared scope while an unrequested deployment remains undone; a final release-acceptance review cannot pass if its required installed-artifact evidence is absent. Identify that distinction in the review context before starting.
