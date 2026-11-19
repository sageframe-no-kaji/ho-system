# Shared review standards

The reviewer determines whether the implementation meets the intended requirements and whether its evidence supports its claims. Review for defects and consequential complexity, with severity based on the effect of the problem. The owner decides disputed intent, acceptable residual risk, and changes to established architecture.

## Authority and interpretation

Apply current user instructions, binding project constraints, explicit owner decisions, the unit's requirements, and then implementation guidance. An owner amendment must identify what it supersedes; a builder's reflection or assertion does not amend a requirement. Where documents conflict and the precedence is unclear, report the conflict rather than selecting whichever reading produces a defect.

Distinguish mandatory acceptance constraints from estimates, preferences, examples, and implementation recipes. A strict requirement still binds until the owner changes it, even if the reviewer considers it excessive. Report a mismatch caused by disputed wording as a decision needed, with its concrete consequence; do not silently turn an estimate into a product requirement or silently waive an explicit constraint.

A specification can itself be wrong. When the implementation follows a recipe that contradicts a required invariant, identify both the invariant failure and the conflicting recipe. State the behavior that must hold and a counterexample; avoid prescribing another unverified mechanism as the fix.

## Evidence for a finding

Each actionable finding includes:

1. A stable ID, reviewed revision, and precise file location; use the smallest relevant line range and name the symbol when useful.
2. The requirement, invariant, architectural purpose, or concrete maintenance cost involved.
3. A reachable input, state, or sequence; expected and actual behavior, or a complete static argument showing the divergence.
4. Evidence provenance: reproduced by this reviewer; statically demonstrated; prior reproducible evidence independently checked; builder-reported only; or unresolved hypothesis.
5. The affected behavior and likely consequence, separated from the effort or size of a repair.
6. A minimal corrective outcome or decision needed, in one or two sentences. Say whether behavior would change.

A file-and-line reference locates a claim but does not establish it. Follow the caller, ownership, scheduling, and data boundaries needed to establish reachability. Check the strongest plausible explanation for why the code is correct before filing the finding, and withdraw or narrow it when that explanation holds.

Do not present a possible failure as reproduced. Static reasoning can establish a defect without running a dangerous or impractical experiment, but name assumptions and the unexercised path. Repeated claims in builder summaries, specifications, and reports count as one claim until checked against primary evidence.

## Severity, confidence, and disposition

These are separate fields, used in both modes:

| Field | Values and meaning |
|---|---|
| Severity | Critical: severe safety, security, or irreversible data exposure/loss on a supported path. Major: material functional failure, failure of an explicit mandatory outcome, or a gate falsely certifying required evidence. Moderate: bounded functional defect or demonstrated recurring maintenance burden. Minor: limited polish or nonblocking inconsistency. |
| Confidence | Confirmed by reproduction or complete static argument; probable with a named unresolved assumption; hypothesis requiring investigation. |
| Kind | Implementation defect; evidence gap; requirement conflict; maintainability; documentation; accepted limitation. |
| Disposition | Must fix; should fix; decision needed; informational. Owner acceptance or rejection is recorded separately. |

Must fix applies to established consequential defects and unambiguous mandatory acceptance failures. A missing test is blocking when that test or equivalent evidence is explicitly required, or when a material correctness claim otherwise lacks adequate support; it is not automatically blocking for every branch. Conflicting acceptance requirements can prevent acceptance through a decision-needed item without pretending that a product bug has been proved.

Severity follows consequence and reachability; remediation priority also considers frequency, exposure, repair cost, and regression risk. An expensive fix does not lower severity, and an easy deletion does not raise it. Do not equate every rubric preference with a Must fix, or classify a closed architectural defect as harmless solely because it requires an owner decision.

An accepted limitation keeps its severity, evidence, owner, rationale, and scope of acceptance. Do not raise it again without changed exposure, invalidated assumptions, or new material evidence. When recommending reconsideration, name that change; the recommendation does not reopen the decision.

## Code quality without quotas

Investigate unused abstractions, duplicate rules and state, unnecessary indirection, inert configuration, opaque tests, weak error handling, and comments that mislead or obscure intent. A finding must explain the cost in the actual code. One implementation, one caller, several named steps, a long file, or a repeated literal is a search cue, not a verdict.

Internal mechanisms need a demonstrable purpose in meeting requirements, preserving correctness, or maintaining an established boundary; each helper does not need its own sentence in the specification. A one-implementation interface may isolate a volatile dependency or provide a useful test boundary. Repeated test setup and independently stated expected values may improve clarity and protect against a test merely agreeing with the implementation.

Prefer consolidation when it removes conflicting sources of truth; prefer separation when it makes independent concerns easier to inspect. Explain the tradeoff of file splitting, including navigation and widened access. Do not propose new abstractions merely to remove a small amount of harmless repetition.

Evaluate comments by meaning: inaccurate claims, obsolete instructions, mechanics repeated without useful orientation, and unsupported promises. Preserve explanations of non-obvious invariants, ownership, constraints, and deliberate compromises. Vocabulary, presumed AI authorship, and the presence of a process citation are not defects by themselves; apply project-specific prose rules only when they are actually adopted.

## Verification and test quality

Read the verification commands and environment restrictions before running them. Run the required checks on the exact reviewed state, capturing command, revision, relevant environment, exit status, test counts, coverage scope, and log location. Do not interpret a pipeline's final display command as the tested command's exit status, and do not claim a full suite ran when only a subset completed.

When assessing a test, identify a plausible incorrect implementation that would still pass it. Prefer an observable outcome or an invariant over incidental timing, a value copied from the implementation, or a stand-in that bypasses the relevant component. Preserve the only meaningful witness for required behavior when simplifying tests.

Choose evidence proportional to the claim and risk. An automated regression, isolated reproduction, boundary test, manual observation, or static proof can each be appropriate; they establish different things. A local demonstration must preserve its input, command or procedure, revision, and output so another reviewer can repeat it. Test helpers and release scripts are not categorically exempt from regression testing, and application code does not categorically require an elaborate new harness for every small repair.

Record what remains unwitnessed. Testing a shared decision property does not test that every caller uses it; a screenshot does not test routing; endpoint memory readings do not establish a peak; high line coverage does not establish correct outcomes. A passing rerun does not establish why a previous test failed.

## Limits on execution

Use only the environment and side effects authorized in the review context. Do not publish, deploy, sign, contact real users, launch applications, access secrets, or reach production systems unless the current task specifically authorizes those actions. This restriction concerns execution, not the review's ability to inspect relevant source and identify risks.

Do not mutate the reviewed source to make a probe pass. Use a permitted isolated scratch copy when reproduction or mutation testing is needed, preserve the failing and passing evidence, and clean up only resources owned by that probe. If verification unexpectedly writes source or changes external state, stop the affected activity and report what happened without destroying the evidence.

The normal plan runs each required verification step once per materially changed executable state. Permit at most one diagnostic retry for an identified environment problem or a bounded flake investigation; retain the original failure. Further execution needs new evidence or an owner-approved investigation budget, not a hope that repetition will produce green.
