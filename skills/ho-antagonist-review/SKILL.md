---
name: ho-antagonist-review
description: Review a bounded ho or independently analyze a completed codebase with evidence-based findings and two or three review passes. Supports review packets, builder handoffs, and authorized coordination of repairs and re-review, including isolated worktrees. A review request alone does not authorize repairs.
---

# Ho antagonist review

Review independently of the builder, establish what the evidence supports, and stop within the declared review budget. A ho is a bounded unit of work with its own requirements and acceptance criteria; the same procedure applies to a milestone, change set, or pull request in a project that does not use Ho terminology.

## Place in the Ho process

Read [ho-integration.md](references/ho-integration.md) when preparing a project review packet, connecting reviews to Kamae planning and authoring, or recording a review outcome in the project. The canonical source is this directory in the Ho System repository; installed copies or folder links expose the same documents to Claude Code, Codex, and other skill-capable agents.

## Select the task

- Select the role as well as the mode: reviewer by default, coordinator when asked to manage the cycle, or builder when asked to assess and repair findings. Read [handoffs.md](references/handoffs.md) for coordinator or builder work and for the separate handoff file linked from each report and its short dispatch prompt. Existing authorization carries forward; do not ask again for steps already within it.
- For one ho or change set, read [standards.md](references/standards.md), [ho-review.md](references/ho-review.md), and [cycle.md](references/cycle.md).
- For an independent final codebase analysis, read [standards.md](references/standards.md), [final-review.md](references/final-review.md), and [cycle.md](references/cycle.md).
- To prepare a review rather than execute one, read the applicable mode and complete [review-context.md](assets/review-context.md) from available project evidence. Do not start the review implicitly.
- Use [report-template.md](references/report-template.md) for findings, evidence, scope coverage, and closure. Its conditional sections are not a requirement to manufacture content.

Resolve these paths relative to this skill's directory. Load only the selected mode; the two modes share standards and cycle rules. No service, model vendor, language, build system, repository layout, or other skill is required.

## Establish the boundary

Before substantive review, pin the complete skill package to a verified Git commit and subdirectory or a recoverable snapshot with a file-hash manifest. Record that version in the context. Follow [handoffs.md](references/handoffs.md) to compare instructions before every handoff and direct recipients to the pinned version; an installed link is not a version record.

Read the user's instructions and applicable repository instructions, then find or complete the review context from the repository, specification, and previous review record. Confirm the reviewed revision, mode, requirements, pass number and ceiling, permitted verification, report destination, and existing rulings before assessing the code. Ask only for missing information that prevents a trustworthy assessment or authorized verification; continue independent read-only investigation while that question is pending.

The default ceiling is two reviewer passes, with a third available when explicitly selected in the review context or subsequently authorized by the owner. Never start another pass, reset the counter, reopen a ho, dispatch a builder, or change source code merely because a report recommends it. A user-authorized coordinator may arrange repairs and subsequent reviews under the same recorded ceiling, and a builder may implement authorized repairs. State the active role; an agent that repairs the work cannot claim to be its independent reviewer.

## Preserve independence and evidence

Use a fresh reviewer context where the host supports one. If you wrote the reviewed implementation or share its deliberations, disclose that limit instead of claiming independence; do not silently create another agent. Read requirements and implementation before earlier reviewer conclusions, while knowing binding owner decisions from the start.

Place reports outside the reviewed repository by default unless the user designates an in-repository destination. In reviewer role, change no repository file except an explicitly designated report or review record. In builder or coordinator role, keep changes within the recorded repair and recordkeeping permissions. Do not commit unless authorized. Verification may create approved temporary outputs in the declared environment; preserve existing changes and record any unexpected mutation. Suggestions to change architecture or reopen work are permitted; carrying them out requires authorization beyond an ordinary repair request.

Report genuine defects, verification gaps, requirement conflicts, and maintainability concerns distinctly. Zero findings is a valid result. At the final allowed pass, issue the report with unresolved items and stop; owner acceptance and reviewer Pass remain separate records.
