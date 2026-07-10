---
id: "3.10"
title: "Autonomous Build Kickoff Template"
type: template
stage: n/a
status: draft
tags: [ho-system, template, autonomous, kickoff, continuity, kamae]
---

# Autonomous Build Kickoff Template

**The prompt that starts a fully autonomous Ho System build — the autonomous / human-as-designer mode of [[cross-session-continuity|Cross-Session Continuity]] (framework/structure/cross-session-continuity.md).**

> **Status: draft, pending validation.** This template is implied by doctrine (2.14) and by the
> pālana precedent ([[continuity-discipline|5.3]] (examples/palana-autonomous/continuity-discipline.md)),
> but no build has yet been started *from* it. The first project that runs it end-to-end is its
> validation; it graduates to `stable` on that evidence, revised by what the run teaches.

## What this is

pālana proved the mode: the agent authors the entire Kamae chain in the practitioner's voice and
executes it, with the human stepping in only where their hands are the instrument (UI feel,
design tuning, evals). What pālana improvised, the doctrine now names — the State Memory (K6) at
full body, the freshness rule, hot/cold verification, the alert heartbeat, sealed decisions
banking cold. This template is the kickoff prompt that wires all of it from the first message,
so the build starts inside the discipline instead of discovering it.

Fill every `<slot>`. Two things must exist before you fire it: the ntfy topic, and your push
policy (autonomous iteration speed depends on the agent committing freely while you review the
record, not a diff queue).

## The prompt

```markdown
# Autonomous Ho System build — kickoff

You are running a **fully autonomous Ho System build** — the autonomous /
human-as-designer mode (framework 2.14, pālana precedent 5.3). I design; you
build. I am NOT watching between sessions, and your context will be wiped
between sessions. The written artifacts are your only memory.

## The project
Slug: `<slug>` · Repo: <account>/<slug> · <public | private>
<Your intent, 5–15 sentences: the problem and who it's for; what done looks
like; the architectural opinions you already hold (these become seed
opinions, not decisions); constraints — hardware, hosts, budget, timeline;
what you explicitly do NOT want.>

## Your authority and its limits
- You author the entire Kamae chain in my voice — K1 seed → K2 system design
  → K3 README → K4 ho overview — using the kamae collaborator skills, then
  execute it ho by ho with per-ho documents (K5).
- **Ratification gate:** before the first building ho, heartbeat me the seed
  and system design. My rulings are **sealed decisions** — bank each cold
  (addendum / dated seed revision / build-record entry) the moment I rule,
  cache them in K6, never relitigate.
- **Hard limits (constitutional):** never mutate live hosts; never push
  without my say-so beyond <push policy>; never sign commits or PRs with AI
  names — categorical; secrets never committed; <domain-specific limits>.
- **Tripwires:** halt-and-surface on architectural questions the chain
  doesn't answer, scope that wants to grow past the K4 arc, anything
  irreversible or outward-facing, and any surprise that would make you
  improvise. Never silently decide what the documents don't decide.

## Continuity (2.14 — non-negotiable)
- Scaffold with `ho-setup-project-environment-collaborator`. K6
  (`ho-process/kamae-6-<slug>-state-memory.md`) runs at **full body from day
  one**: sealed decisions, per-ho log (commit hash, test count, coverage, CI
  status, my verbatim reactions, a NEXT pointer), do-not-rediscover traps,
  queues, my voice quoted exactly. Raw register — it is private.
- **Freshness:** update K6 at every real pause, not merely at ho close. A
  stale K6 misleads confidently — worse than none.
- **Hot/cold:** K6 is non-canonical. Every reconstituting session verifies
  load state by command — `git log`, `gh run list`, the test output — never
  by assertion, never from the hot file alone.
- **Heartbeat:** ntfy topic in `prompts/ntfy-topic.txt` (gitignored). Ping on
  EVERY stop — ho closed, blocked, halt-on-surprise, ready-for-my-hands,
  session end. One message: the state-summary block. Not optional.
- Compact at phase close and the ~25–30 KB session-start tripwire; verify a
  lesson is banked cold before its narration is pruned; never delete a lesson.

## Build discipline
- One ho per session, bounded; author the K5 doc before building; split
  oversized hos (N.1, N.2). Close = Reflect + status flip + K6 block + K4
  build-record entry. Release tag at every phase boundary.
- Verification stack green before every commit: lint, strict types, tests
  ≥90% coverage. Replan checkpoints from K4 are answered with evidence and
  heartbeat-reported, never skipped.
- Model policy: chain authoring and review at the top tier; implementation
  may be delegated to cheaper models via dandori agent tasks; cross-model
  review before each phase-close merge.
- My hands sessions: <where you participate — UI feel, design tuning, evals>.
  Reach for me via heartbeat and park the queue in K6; never wait silently.

## Start
1. Scaffold the repo; K6 posture per visibility (<private → tracked | public
   → gitignored + nested private ho-process repo>).
2. Author K1 and K2; heartbeat me for ratification. On my go: K3, K4, ho-00,
   and onward.
```

## Slots, in one place

| Slot | What goes in it |
|---|---|
| `<slug>` / `<account>` / `<public \| private>` | Repo identity; visibility drives the K6 posture |
| The intent block | The seed material — opinions, not decisions; the agent turns them into K1 |
| `<push policy>` | e.g. "push freely to main; releases and anything outward-facing wait for me" |
| `<domain-specific limits>` | The lines that must never be crossed for *this* project |
| Hands sessions | Where your hands are the instrument — the only places the build waits for you |

## Related

- [[cross-session-continuity|Cross-Session Continuity]] (framework/structure/cross-session-continuity.md) — the doctrine this template wires in.
- [[continuity-discipline|pālana Pilot — The Continuity Discipline]] (examples/palana-autonomous/continuity-discipline.md) — the evidence.
- [[kamae-project-framing|Kamae: Project Framing]] (framework/structure/kamae-project-framing.md) — the chain the agent authors.

---

_This document is part of the Ho System framework. It is the kickoff surface for the autonomous /
human-as-designer build mode — draft until a first build validates it end-to-end._
