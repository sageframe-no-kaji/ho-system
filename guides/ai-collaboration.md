---
id: "4.3"
title: "Working with AI as Implementation Partner"
type: guide
stage: n/a
status: draft
tags: [ho-system, ai-collaboration, implementation]
---

# Working with AI as Implementation Partner

The Ho System uses AI to build, not to decide. The agent writes code fast and well; the judgment — what to build, whether it's right, when the agent is wrong — stays with you. This guide is the posture that keeps that line clear inside a working session. It's the on-ramp; the full account is [[operating-discipline|The Operating Discipline]] (practitioner/operating-discipline.md), and you should read that once. Here are the five habits that carry most of it.

## Plan before you build

Every code-writing session starts in planning mode. The agent reads the per-ho document — the scope for this session — and states back its understanding: what it's going to do, where the spec is ambiguous, what approach it proposes. You don't approve until that plan matches your intent.

This is where your architectural authority actually lives. Once the agent is writing code, you're reacting to what exists. In planning mode you're shaping what gets produced before any of it exists — the difference between authoring and editing. Planning mode is cheap and catches the three expensive things: the agent misread the spec, the spec was ambiguous, or the agent is quietly proposing more (or less) than you scoped. Catch those here and they cost a sentence. Catch them in the code and they cost a rewrite.

## Watch the work

Read the terminal as the agent works — file writes, command output, test results. Not glancing. Reading. An architectural choice the agent made twenty minutes ago is baked into four files by the time you notice; the same choice caught the moment it's proposed is a one-line correction.

Watching is the part that's easy to skip when sessions run long, and it's where most preventable mistakes get caught. You're not typing — the agent does the work — but you're *in* the work, ready to intervene. The presence is the verification. What you're willing to merge changes when you've actually watched it get built.

## Answer questions before acting

When the agent asks something mid-session — "isn't this slow?", "should this be async?", "is this the right table?" — that's a decision input, not a preamble. Stop and answer it before anything else happens. A prior "go ahead" doesn't cover a question the agent hadn't raised yet.

The same rule binds the agent toward you: if the session surfaces an architectural question the documents didn't decide, the agent's job is to *stop and surface it*, not to quietly make the call. Decisions that shape the system belong in a thinking conversation and then in the documents — not buried in an implementation the agent chose on its own.

## Keep sessions bounded

A session has a beginning (planning mode, one ho), a scope (what the ho specified), and an end (the deliverable is done and committed, or the context is degrading). When it's done, commit and stop. Don't roll the next ho in.

Long sessions degrade. The agent's context fills with file contents, command output, and old reasoning; past a point, earlier decisions get forgotten and you find yourself repeating things the agent knew an hour ago. The fix is a fresh session, in clean context, against the next ho. This is also why hos are sized to fit one session — if a ho needs two, it's too big and should split.

## Verification is the spine

Nothing closes until it's verified. Code → lint → type-check → test → commit, and no commit happens unless all four are green. Tests are specifications of behavior, not a chore you do after; uncovered lines are unspecified behavior. This matters more with an AI partner, not less: the agent that wrote the code also wrote the tests, so the tests can share the code's blind spots. That's the case for a second look — a more capable model reviewing significant changes, and always your own read — because verification shouldn't all come from one source.

Verification asks *did we build it right?* There's a second question — *did we build the right thing, and is it any good?* — that tests can't answer. Both live in [[verification-practices|Verification Practices]] (framework/structure/verification-practices.md).

## What you own, what the agent does

The split is the whole point of the practice.

**You own** the architecture, the scope, the decisions, and the verdict. What gets built, what "done" means, whether the result is correct, and when to overrule the agent — those are yours and don't delegate. You also own the calibration: naming honestly which parts of the system you understand and which you're treating as a black box (see [[tiered-understanding|Tiered Understanding]] (framework/structure/tiered-understanding.md)).

**The agent does** the implementation: writing code against a plan you approved, writing tests, running the stack, executing bounded [[agent-task-spec|agent tasks]] (framework/templates/agent-task-spec.md) where the work is well-specified enough to hand off. It moves fast within the bounds you set and stops when it hits a decision that isn't yours to have pre-made for it.

Hold that line and the record shows it: work you authored and verified, built with AI — not built by AI while you watched. That distinction is what the [[devlog|devlog]] (framework/structure/devlog.md) exists to make visible, and it's the difference the whole methodology is built to protect.

## Where to go next

- [[getting-started|Getting Started with the Ho System]] (guides/getting-started.md) — the first arc, concretely.
- [[operating-discipline|The Operating Discipline]] (practitioner/operating-discipline.md) — the full account of everything above.
- [[shu-ha-ri|Shu-Ha-Ri Progression]] (framework/structure/shu-ha-ri.md) — how the collaboration shifts as you move from shu to ri.
