# Shared review standards

The reviewer determines whether the implementation meets the intended requirements and whether its evidence supports its claims. Review for defects and consequential complexity, with severity based on the effect of the problem. The owner decides disputed intent, acceptable residual risk, and changes to established architecture.

## Authority and interpretation

Apply current user instructions, binding project constraints, explicit owner decisions, the unit's requirements, and then implementation guidance. An owner amendment must identify what it supersedes; a builder's reflection or assertion does not amend a requirement. When turning conversation into acceptance criteria, preserve route, platform, timing, and exception qualifiers. Identify the source as an owner instruction, an accepted proposal, or an inference; seek a ruling only where an unresolved inference changes acceptance. Where documents conflict and the precedence is unclear, report the conflict rather than selecting whichever reading produces a defect.

Distinguish mandatory acceptance constraints from estimates, preferences, examples, and implementation recipes. A strict requirement still binds until the owner changes it, even if the reviewer considers it excessive. Report a mismatch caused by disputed wording as a decision needed, with its concrete consequence; do not silently turn an estimate into a product requirement or silently waive an explicit constraint.

A specification can itself be wrong. When the implementation follows a recipe that contradicts a required invariant, identify both the invariant failure and the conflicting recipe. State the behavior that must hold and a counterexample; avoid prescribing another unverified mechanism as the fix. Before presenting mechanisms for an owner decision, check inexpensive feasibility questions through authorized inspection or isolated probes. Label unresolved technical premises as unverified instead of asking the owner to choose on the assumption they work.

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
| Severity | Critical: severe safety, security, or irreversible data exposure/loss on a supported path. Major: material functional failure or a gate falsely certifying consequential required evidence. Moderate: bounded functional defect or demonstrated recurring maintenance burden. Minor: limited polish or nonblocking inconsistency. |
| Confidence | Confirmed by reproduction or complete static argument; probable with a named unresolved assumption; hypothesis requiring investigation. |
| Kind | Implementation defect; evidence gap; requirement conflict; maintainability; documentation. An accepted limitation keeps its original kind. |
| Disposition | Must fix; should fix; decision needed; informational. Owner acceptance or rejection is recorded separately. |

Must fix applies to an established defect that is consequential: a failed specified requirement; a security problem; lost or damaged data; unspecified behavior that a user or another component can observe; a test or gate that passes while the behavior is broken; or a regression in something that worked before. An ambiguity that will affect the user is a decision needed, and it blocks Pass until the owner rules. A missing test is blocking when that test or equivalent evidence is explicitly required, or when a material correctness claim otherwise lacks adequate support; it is not automatically blocking for every branch. Conflicting acceptance requirements can prevent acceptance through a decision-needed item without pretending that a product bug has been proved.

Severity follows consequence and reachability; a mandatory acceptance failure may be Must fix without being Major. Name the actual effect rather than raising severity merely because the requirement says “every” or “always.” Remediation priority also considers frequency, exposure, repair cost, and regression risk. An expensive fix does not lower severity, and an easy deletion does not raise it. Do not equate every rubric preference with a Must fix, or classify a closed architectural defect as harmless solely because it requires an owner decision.

An accepted limitation keeps its severity, evidence, owner, rationale, and scope of acceptance. Do not raise it again without changed exposure, invalidated assumptions, or new material evidence. When recommending reconsideration, name that change; the recommendation does not reopen the decision.

## Code quality without quotas

Investigate unused abstractions, duplicate rules and state, unnecessary indirection, inert configuration, opaque tests, weak error handling, and comments that mislead or obscure intent. A finding must explain the cost in the actual code. One implementation, one caller, several named steps, a long file, or a repeated literal is a search cue, not a verdict.

Internal mechanisms need a demonstrable purpose in meeting requirements, preserving correctness, or maintaining an established boundary; each helper does not need its own sentence in the specification. A one-implementation interface may isolate a volatile dependency or provide a useful test boundary. Repeated test setup and independently stated expected values may improve clarity and protect against a test merely agreeing with the implementation.

Prefer consolidation when it removes conflicting sources of truth; prefer separation when it makes independent concerns easier to inspect. Explain the tradeoff of file splitting, including navigation and widened access. Do not propose new abstractions merely to remove a small amount of harmless repetition.

Evaluate comments by meaning: inaccurate claims, obsolete instructions, mechanics repeated without useful orientation, and unsupported promises. Preserve explanations of non-obvious invariants, ownership, constraints, and deliberate compromises. Vocabulary, presumed AI authorship, and the presence of a process citation are not defects by themselves; apply project-specific prose rules only when they are actually adopted.

## Verification and test quality

Read the verification commands and environment restrictions before running them. Run the required checks on the exact reviewed state, capturing command, revision, relevant environment, exit status, test counts, coverage scope, and log location. Do not interpret a pipeline's final display command as the tested command's exit status, and do not claim a full suite ran when only a subset completed.

When assessing a test, identify a plausible incorrect implementation that would still pass it. For each material repair, the builder states the claimed outcome, observable witness, actual boundary exercised, and remaining limit in the existing response. A substituted callback establishes invocation, not the omitted external behavior; eventual equality establishes the result, not a claimed intermediate mechanism. Check that a stand-in models the real dependency's result domain, including additional objects or states the operation creates. Prefer the smallest permitted witness that distinguishes correct behavior from the plausible failure, and preserve it when simplifying tests.

Choose evidence proportional to the claim and risk. An automated regression, isolated reproduction, boundary test, manual observation, or static proof can each be appropriate; they establish different things. A local demonstration must preserve its input, command or procedure, revision, and output so another reviewer can repeat it. Test helpers and release scripts are not categorically exempt from regression testing, and application code does not categorically require an elaborate new harness for every small repair.

When a control dispatches work through a framework, inspect the actual dispatch and receiver. A test that checks a menu action, route name, callback registration, or command string can pass while the framework selects another handler. Use a focused dispatch witness or trace the resolution path and record its remaining execution limit; do not require a full GUI suite solely because a control was added.

For an affected asynchronous or persistent workflow, examine a second operation and an operation superseded while still in flight, as applicable. An isolated first success or failure does not establish state reuse or stale-result handling. Choose witnesses from the actual lifecycle rather than imposing every example on every project.

Before treating a failed probe as a product defect, check its premise and executable inputs. A regression test normally asserts the repaired behavior; a defect-reproduction probe may intentionally assert the old failure and therefore fail after a successful fix. Classify the actual observed state against the requirement, preserve both outputs, and identify any changed assertions. Rebuild the exact object or test module the probe links, including after restoring a mutation experiment; unchanged source hashes alone do not identify a stale compiled object.

Record what remains unwitnessed. Testing a shared decision property does not test that every caller uses it; a screenshot does not test routing; endpoint memory readings do not establish a peak; high line coverage does not establish correct outcomes. A passing rerun does not establish why a previous test failed. Inspect raw output before inferring a missing event or user action from filtered logs. Describe a boundary as untested or unavailable in the current environment rather than claiming it cannot be automated without evidence for that limitation.

## Limits on execution

Use only the environment and side effects authorized in the review context. Do not publish, deploy, sign, contact real users, launch applications, access secrets, or reach production systems unless the current task specifically authorizes those actions. This restriction concerns execution, not the review's ability to inspect relevant source and identify risks.

Do not mutate the reviewed source to make a probe pass. Use a permitted isolated scratch copy when reproduction or mutation testing is needed, preserve the failing and passing evidence, and clean up only resources owned by that probe. If verification unexpectedly writes source or changes external state, stop the affected activity and report what happened without destroying the evidence.

Before parallel verification or packaging, inspect shared inputs, outputs, build caches, fixture ownership, and cleanup. Serialize jobs when one can replace or delete another's inputs, or give them isolated directories. A launch or background-task notification is not a passing result; wait for the relevant task and its recorded exits.

The normal plan runs each required verification step once per materially changed executable state. Permit at most one diagnostic retry for an identified environment problem or a bounded flake investigation; retain the original failure. Further execution needs new evidence or an owner-approved investigation budget, not a hope that repetition will produce green.
