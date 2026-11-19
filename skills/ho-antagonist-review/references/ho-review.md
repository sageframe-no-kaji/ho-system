# Ho level antagonist review

A ho review checks one bounded unit against its requirements, the system constraints it must preserve, and the behavior of its changes. Its scope includes the surrounding code needed to follow those changes through the system. It does not become a new audit of every unrelated file on each return.

Use the [shared standards](standards.md), [cycle rules](cycle.md), and completed [review context](../assets/review-context.md). The intended output is a report with evidence-backed findings, the acceptance state, and an explicit next step. A clean review is complete without inventing something to improve.

## Pass 1: review the complete unit

Read binding constraints and the unit's requirements before the diff. Establish the opening and closing revisions, distinguish pre-existing changes, and inspect moved or renamed code without mistaking movement for new implementation. Trace each material change to a required outcome or a necessary implementation purpose, and check that decisions recorded as retained were not silently changed.

Inspect the following where the unit touches them:

| Area | Questions |
|---|---|
| Required behavior | Does the implementation produce the specified result for ordinary, boundary, failure, and cancellation paths? Are requirements mutually consistent? |
| State and lifetime | Who owns each resource and decision? Can a delayed callback, concurrent action, cancellation, teardown, or retry outlive the state that makes it valid? |
| Interfaces | Do callers and consumers agree about ordering, formats, units, errors, persistence, and ownership? Does a local change break an existing user of the component? |
| Tests and evidence | What incorrect behavior could still pass? Does the witness reach the claimed boundary? Are expected values independent where they need to be? |
| Complexity | Does introduced state, abstraction, indirection, duplication, or defensive handling carry a concrete cost without a compensating purpose? |
| Scope | Are new dependencies, behavior, configuration, or component responsibilities authorized? Are editorial or supporting-file changes genuinely consequential or a record correction? |
| User-facing claims | Do changed documentation, examples, messages, and operational instructions match the implementation and the verified evidence? |

Inspect related mechanisms together so the first report captures the failure class rather than a single example. For concurrency, follow the state transitions around suspension and delivery; for parsers, vary input partitioning and boundary interactions; for data changes, inspect failure and recovery paths. These are examples of analysis suited to the system, not a mandatory technology checklist.

For affected asynchronous or persistent workflows, examine a second operation and an operation superseded while still in flight, as applicable. Successive saves, eviction of unsent changes, and an old response arriving after navigation are examples; choose witnesses from the actual lifecycle rather than imposing every example on every project.

Run the declared verification and investigate material failures within the budget. Submit one consolidated report, merging manifestations of a shared cause and separating independent causes. Classify incomplete verification honestly even when the static review found no defect.

## Pass 2: review repairs and consequences

Read the prior finding ledger and owner rulings, then inspect the repair range from the previously reviewed endpoint. For every unresolved item, check whether the repair fixes the causal mechanism, satisfies the intended requirement, and preserves the relevant behavior around it. Test variations within the same failure class rather than replaying only the exact example that prompted the fix.

Inspect direct callers, affected invariants, and new behavior introduced by those repairs. Keep the original opening-to-current range available as context, but do not restart a full-file sweep for unrelated style concerns. A claimed move or behavior-preserving refactor still warrants comparison against the pre-repair code.

New findings must identify their origin: introduced by the repair, part of an unresolved original cause, newly reachable because of the repair, or a missed pre-existing issue. An incidental pre-existing concern is recorded without automatically extending the repair scope. A newly discovered consequential defect or mandatory acceptance failure cannot be hidden to preserve a pass count; record it and apply the stopping rule instead of starting another cycle.

## Optional pass 3: decide the remaining items

Use this pass only within a declared three-pass budget or explicit owner authorization to extend two passes to three. Review remaining findings, their repairs, and direct regressions. Do not spend the last pass generating new polish work or recopying unchanged answers from the first report.

After its report, stop regardless of result. If a requirement conflict, recurring failure, or incomplete fix remains, state the unresolved consequence and recommend the next owner decision. A recommendation to reopen a ho or revise architecture does not authorize that work.

## Architecture and scope disputes

Respect established boundaries while checking whether they still satisfy the system's obligations. When the root cause lies in a closed decision, describe the failure and recommend reconsideration with the minimum affected boundary; do not invent a replacement architecture. Keep its severity tied to the consequence rather than demoting it because it requires judgment.

Do not make harmless formatting, a supporting comment correction, or an additional useful test a product blocker solely because a literal file table omitted it. If the file table is an explicit mandatory boundary, record the discrepancy and seek its disposition; report functional acceptance separately from that scope decision. Conversely, a reflection that admits unauthorized behavior does not retroactively authorize it.

## Acceptance

Use the assessment and workflow states defined in [cycle.md](cycle.md). A reviewer Pass means the declared unit was reviewed, required evidence is adequate, no unresolved Must-fix remains, and no unresolved decision prevents assessing acceptance. It does not mean zero limitations, complete test coverage, or proof that the entire repository is correct.
