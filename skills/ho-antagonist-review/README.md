# Ho Antagonist Review

An evidence-based review skill for AI-assisted software development, with bounded reviews of individual work units and an independent review of the completed codebase.

The antagonist examines the implementation against its requirements, tests the builder's claims, and reports what the evidence supports. The owner decides intent, scope, and accepted risk. Reviews default to two passes, with an optional third; unresolved findings remain visible when the cycle stops.

A *ho* (歩, “step”) is a bounded unit of work in the Ho System. This skill also works with a milestone, change set, or pull request, without adopting the rest of the methodology. It uses Markdown instructions and templates, with no executable service or required framework.

## Two review modes

| Mode | Scope | Result |
|---|---|---|
| Ho review | One completed unit, its requirements, changed implementation, and affected boundaries | Findings, verification evidence, and an acceptance assessment for that unit |
| Final codebase review | The completed repository, its components, critical flows, operational definitions, and public claims | An independent system assessment with an explicit record of inspection coverage and gaps |

The final review examines the current system directly. Earlier ho reports help reconcile history and accepted decisions, but their individual Pass results do not establish that the combined system works. Each final review has its own declared cycle and budget.

## Install

Obtain the complete folder from its source repository, [sageframe-no-kaji/ho-system](https://github.com/sageframe-no-kaji/ho-system/tree/main/skills/ho-antagonist-review), or extract a skill archive you were given. Keep `SKILL.md`, `references/`, and `assets/` together; `agents/` is optional. Install the folder as follows:

| Agent | User installation | Invocation |
|---|---|---|
| Claude Code | `~/.claude/skills/ho-antagonist-review/` | `/ho-antagonist-review` |
| Codex | `~/.agents/skills/ho-antagonist-review/` | `$ho-antagonist-review` |

Both locations must contain `SKILL.md` directly inside `ho-antagonist-review`, without an extra nested folder. See the host documentation for [Claude Code skills](https://code.claude.com/docs/en/skills) and [Codex skills](https://learn.chatgpt.com/docs/build-skills). Local installation does not also install the skill in a cloud account; upload the skill archive through that account's own mechanism.

Install or link the whole folder: copying only `SKILL.md` omits the review documents. For another agent, supply the complete folder and ask it to read [SKILL.md](SKILL.md). The files in `agents/` provide optional Codex display metadata; the review procedure does not depend on them. If an installed skill has not appeared in the host's current session, start a fresh session or refer to `SKILL.md` by its actual path.

## Start a review

Prepare a project-specific copy of [the review context](assets/review-context.md). The skill version is pinned before the first dispatch, by the coordinator or, when there is none, by the reviewer, so later passes use the same rules; the [handoff guide](references/handoffs.md#pin-the-governing-instructions) says how. Identify the reviewed revision, requirements, owner rulings, permitted verification, report destination, and current pass number and ceiling. Supply previous reports for a follow-up, preserving the same cycle identity and counter.

Use a fresh reviewer context when possible. Give that reviewer access to the repository and supporting evidence; if it also built the implementation, disclose that limitation. The skill does not create another agent or grant access on its own.

Example for Claude Code:

```text
/ho-antagonist-review
Review the ho described in REVIEW_CONTEXT_PATH. This is pass 1 of 2.
Write the report to REPORT_PATH. Do not modify implementation files.
```

Example for Codex:

```text
Use $ho-antagonist-review in final mode with REVIEW_CONTEXT_PATH.
Independently assess the completed repository and record inspection coverage.
This is pass 1 of 2. Write the report to REPORT_PATH.
Do not modify implementation files.
```

Replace the uppercase paths with actual locations. The reviewer reports findings with their requirement, location, reachable failure or maintenance consequence, and supporting evidence. It distinguishes established defects, incomplete verification, requirement conflicts, and nonblocking observations; zero findings is a valid outcome.

## Hand the report to the builder

A report that returns work links to a separate handoff file beside it. Give the main agent a two- or three-line prompt pointing to that file. The builder assesses each finding, repairs supported defects within authorization, and answers with what changed and the evidence for it. A follow-up reviewer receives those files under the same cycle and counter.

The [handoff guide](references/handoffs.md) carries the rules, with a [handoff template](assets/handoff.md) for the file. It supports manual handoffs and authorized agent-to-agent dispatch. Select coordinator role when asking an agent to manage the cycle, or builder role when asking it to repair findings; a review request alone selects reviewer role.

For direct exchanges between agents, use [the agent transport guide](references/agent-transport.md): Herdr connects them through `hc`, and tuios offers agent mail. Host permissions still apply, and a prepared connection is not a completed live test.

Every repair is committed before it is reviewed again. Final-review repairs always use a separate Git worktree; ho-review repairs use one when the checkout is shared.

## Bound the cycle

Pass 1 covers the declared scope. Pass 2 examines authorized repairs, unresolved findings, and their direct consequences; a third pass is available when explicitly selected or authorized. The reviewer issues Pass, Return, or Incomplete, and stops at the ceiling with unresolved items left for the owner. Owner acceptance is recorded separately from the reviewer's assessment. The [cycle rules](references/cycle.md) are the authority for all of this.

Only the owner changes the budget. The ceiling is an instruction supported by a persistent record, not a software execution guard: any automated coordinator must enforce the counter before starting another review. This package supplies instructions for coordination, repair handoffs, and agent transport. It does not install a daemon or orchestration service, alter agent grants, or connect existing sessions by itself.

## Relationship to Kamae

Antagonist review sits alongside the Kamae document chain as a recurring verification responsibility. Overview planning schedules the reviews, and per-ho authoring defines their acceptance boundary and prepares the context. It is not another numbered Kamae step; the [Ho integration guide](references/ho-integration.md) explains the connections and project records.

## Included documents

| File | Purpose |
|---|---|
| [SKILL.md](SKILL.md) | Agent entry point and mode selection |
| [Shared standards](references/standards.md) | Evidence, severity, requirements, code quality, and verification |
| [Ho review](references/ho-review.md) | Review of a bounded implementation unit |
| [Final review](references/final-review.md) | Independent assessment of the completed system |
| [Handoff guide](references/handoffs.md) | Version checks, file-and-pointer handoffs, agent coordination, and worktrees |
| [Agent transport](references/agent-transport.md) | Direct Herdr/hc and tuios exchanges, return routing, authorization, and recovery |
| [Handoff template](assets/handoff.md) | Cycle-specific inputs, rulings, endpoints, and output location |
| [Cycle rules](references/cycle.md) | Pass counting, finding dispositions, stopping, and owner decisions |
| [Report templates](references/report-template.md) | Reviewer report, builder response, and owner decision |
| [Review context](assets/review-context.md) | Project inputs and persistent cycle state |
| [Ho integration](references/ho-integration.md) | Planning, authoring, acceptance, and continuity |

The reference documents carry the operating rules; this README introduces their use. No project transcript or case-study evidence is needed to run the skill. Verification quality still depends on the available evidence and the reviewing agent, and a completed source review does not establish behavior in an environment it never examined.

## Independent distribution

The portable package consists of `SKILL.md`, `README.md`, `LICENSE`, and the complete `references/`, `assets/`, and `agents/` directories. Include every file under those paths for both publication and cycle snapshots. Development records in `notes/` and generated archives in `dist/` are outside this boundary; they are not governing instructions and need not travel to a reviewer.

Publish those package paths at the root of an independent repository, or use `dist/ho-antagonist-review.zip`, whose top-level folder is `ho-antagonist-review/`. Keep all relative paths intact. Do not copy the whole development directory as a public distribution: the local notes include project-specific transcripts and evidence.

Maintain the package in Git and regenerate distributed copies from it. Notes and archive changes do not change the governing package version. The broader Ho System and development case studies are not installation dependencies.

## Development records

The maintained analyses, evidence, and revision notes live under `notes/` beside this skill. Start with `notes/README.md` when working in the Ho System checkout; this local index is intentionally absent from portable distributions. Normal reviews load the instructions for their selected role and mode, not the development archive. Structural checks and observed use are documented separately from unperformed behavioral validation.

## Author and license

Created by Andrew Todd Marcus as part of the Ho System. Distributed under the Ho System's existing [Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International license](LICENSE). See the license for its terms and commercial-use contact.

Using the skill to review your own project is permitted, commercial or not, and so is filling in its templates and keeping them in that project's records. The license terms govern redistribution of the skill itself.
