---
name: ho-antagonist-review
description: Review a bounded ho or independently analyze a completed codebase with evidence-based findings and two or three review passes. Supports review packets, builder handoffs, and authorized coordination of repairs and re-review, including isolated worktrees. A review request alone does not authorize repairs.
---

# Ho antagonist review

Review independently of the builder, establish what the evidence supports, and stop within the declared review budget. A ho is a bounded unit of work with its own requirements and acceptance criteria; the same procedure applies to a milestone, change set, or pull request in a project that does not use Ho terminology.

## Select the role and mode

State the active role: reviewer by default, coordinator when asked to manage the cycle, builder when asked to assess and repair findings. An agent that repairs the work cannot claim to be its independent reviewer. Then read what the work needs:

| Work | Read |
|---|---|
| Review one ho or change set | [standards.md](references/standards.md), [ho-review.md](references/ho-review.md), [cycle.md](references/cycle.md) |
| Independent final codebase analysis | [standards.md](references/standards.md), [final-review.md](references/final-review.md), [cycle.md](references/cycle.md) |
| Coordinate a cycle, or repair findings as builder | [handoffs.md](references/handoffs.md), with the documents for the cycle's mode |
| Exchange handoffs directly between agents | [agent-transport.md](references/agent-transport.md) as well |
| Prepare a review without running it | The applicable mode; complete [review-context.md](assets/review-context.md) from project evidence. For a Ho project packet, follow [ho-integration.md](references/ho-integration.md#kamae-5-prepare-the-review-packet). Do not start the review implicitly. |
| Write the report | [report-template.md](references/report-template.md). Its conditional sections are not a requirement to manufacture content. |
| Connect reviews to Kamae planning, authoring, or project records | [ho-integration.md](references/ho-integration.md) |

Resolve these paths relative to this skill's directory. Load only the selected mode; the two modes share standards and cycle rules. No service, model vendor, language, build system, repository layout, or other skill is required. The canonical source is this directory in the Ho System repository; installed copies or folder links expose the same documents to Claude Code, Codex, and other skill-capable agents.

## Establish the boundary

Before substantive review, pin the governing instructions as [handoffs.md](references/handoffs.md#pin-the-governing-instructions) requires and record that version in the context. An installed link is not a version record.

Read the user's instructions and applicable repository instructions, then find or complete the review context from the repository, specification, and previous review record. Confirm the reviewed revision, mode, requirements, pass number and ceiling, permitted verification, report destination, and existing rulings before assessing the code. Ask only for missing information that prevents a trustworthy assessment or authorized verification; continue independent read-only investigation while that question is pending.

## Limits in every role

- **Budget.** [cycle.md](references/cycle.md) defines the pass ceiling, who may change it, and what a finding or recommendation authorizes. At the final allowed pass, issue the report with unresolved items and stop.
- **Authorization.** A review request authorizes the reviewer role only. Coordination and repairs need the authorization described in [handoffs.md](references/handoffs.md#authorization-and-transport); once given it carries forward, so do not ask again for steps already within it.
- **Independence.** Use a fresh reviewer context where the host supports one. If you wrote the reviewed implementation or share its deliberations, disclose that limit instead of claiming independence; do not silently create another agent. For an initial review, read requirements and implementation before earlier reviewer conclusions, while knowing binding owner decisions from the start. For a repair review, start with the prior ledger, builder response, and rulings, then independently check their claims against the changed implementation.
- **Files.** Place reports outside the reviewed repository by default unless the user designates an in-repository destination. In reviewer role, change no repository file except an explicitly designated report or review record. In builder or coordinator role, keep changes within the recorded repair and recordkeeping permissions. Verification may create approved temporary outputs in the declared environment; preserve existing changes and record any unexpected mutation. Do not commit unless authorized.
- **Reporting.** Report genuine defects, verification gaps, requirement conflicts, and maintainability concerns distinctly. Zero findings is a valid result.
