---
id: "5.3"
title: "pālana Pilot — The Continuity Discipline"
type: example
stage: n/a
status: active
version: "1.0"
tags: [ho-system, example, palana, pilot, autonomous, continuity, memory, handoff, proposal]
---

# pālana Pilot — The Continuity Discipline

**A practice report from the first fully autonomous Ho System build — how it maintained cross-session continuity, written as a repeatable process, with proposed framework additions.**

> This is evidence from practice, not ratified doctrine. It documents what the pālana pilot *did*, names the practices as a process another autonomous build could copy, and proposes what the framework could adopt. The proposals are recommendations pending validation, in the spirit of [Contributing](../../CONTRIBUTING.md) ("practice reports … a full project arc is gold") and the evidence-first posture of [Foundations and Evidence](../../framework/ho-foundations-evidence.md). Nothing here supersedes a framework structure document.

---

## What pālana is, and why continuity became the hard problem

pālana is a native macOS file manager for the homelab operator, and — more relevant here — the first Ho System project built **fully autonomously**: the agent authors the entire Kamae chain (seed, system design, README, ho overview, per-ho docs, dandori specs) in the practitioner's voice and then executes it, as a test of *whether the methodology's discernment can be carried by the agent itself*. The practitioner steps in only for UI/UX "hands" sessions, where he drives the running app and gives feel feedback.

A fully autonomous, multi-session build faces a problem the standard Ho System never confronts head-on: **no human carries the thread between sessions.** In a human-driven Ho build the practitioner *is* the continuity — they remember the last session, hold the architectural intent, drive planning mode, and are the watching presence that catches drift. In an autonomous build the agent's context window is erased between sessions, and the agent itself must reconstitute the entire build state from written artifacts alone.

The framework's answer to continuity is real but assumes a human: per-ho documents as the next session's entry point, the Reflect phase as "the primary evidence of learning," the forward-only principle, and `builds-on:` frontmatter for reading order — all resting on *"documents as memory"* and on the practitioner being present to watch. Remove the human and two of those load-bearing roles vanish: **holding intent across sessions**, and **being the verification presence**. pālana evolved a four-layer system to fill exactly that gap.

---

## The four layers

| Layer | Artifact | Location | Reader | Visibility | Cadence |
|---|---|---|---|---|---|
| **1. Working memory / handoff** | `palana-autonomous-run.md` | `~/.claude/projects/<proj>/memory/` (private, not in repo) | the next agent session | private | **every real pause** — appended/updated continuously |
| **2. Public build record** | `Build record.` section at the tail of `kamae-4-*-ho-overview.md` | project repo, tracked | practitioner + future agents | public | one entry per ho-close and per checkpoint |
| **3. Per-ho Reflect** | `## Phase 3 — Reflect` in each `ho-NN-*.md` | project repo, tracked | anyone reading that ho | public | once, at close (flips `status: draft → complete`) |
| **4. Human alert** | ntfy push notification | topic in `prompts/ntfy-topic.txt` (gitignored) | the practitioner's phone | private | every time work stops — mandatory |

These are not redundant. They differ deliberately along three axes: **audience** (agent vs. human), **durability** (living scratch vs. immutable record), and **granularity** (per-pause vs. per-ho vs. per-stop).

---

### Layer 1 — The working-memory handoff file (the load-bearing innovation)

`palana-autonomous-run.md` is a single Ho System *memory file* (frontmatter `type: project`) that has been grown into the primary cross-session handoff document. It is the first thing a fresh agent reads to reconstitute the entire build state — without opening a line of code. At the time of writing it is ~72 KB and growing. Everything else in this report is close to standard Ho practice; **this file is not**, and it is the real contribution.

**What it holds** (observed structure, in order):

1. **Sealed decisions** — practitioner rulings that supersede the written chain (e.g. *"Swift/SwiftUI, native macOS only — supersedes the seed's Tauri+Rust+Svelte opinion"*), each dated and attributed. This is the constitutional layer: the agent may not relitigate it.
2. **Stop conditions and hook exceptions** — the operating envelope: when to interrupt the human, which repo the environment hooks allowlist, the hard limits (never mutate live hosts, never sign commits with AI tags).
3. **A dense chronological build log** — one bolded paragraph per ho / per session / per hands-round, each stamped with what closed, the commit hash, test count, coverage %, CI status, the practitioner's verbatim reactions, and a **`NEXT:`** pointer.
4. **Hard-won execution findings** — a "do not rediscover" section of platform traps (SwiftUI quirks, SwiftLint conflicts, fixture gotchas, the `charactersIgnoringModifiers`-applies-Shift trap). Institutional memory that would otherwise be re-learned every session at cost.
5. **Queues** — deferred work parked against the ho that will pick it up ("queued to ho-9.8", "9.5 Think now owes …").
6. **Verbatim practitioner voice** — his laws quoted exactly (*"ssh or fucking bust"*, *"I LOVE that it will choose the right tool"*), so design intent survives the context wipe with its register intact.

**Why it works as a handoff:** a new session reads this file and knows — without reading code — what is done, what is next, what is sealed, which traps to avoid, and what the practitioner actually wants. It is the practitioner's cross-session memory, externalized into a file the agent reloads.

**The discipline that keeps it honest** — learned the hard way, recorded in the file itself:

- *"MEMORY WAS DANGEROUSLY STALE through this whole hands session — update at every real pause."*
- *"this memory file IS the handoff; keep it current at every stop."*

Staleness is the dominant failure mode. The file is only as good as its freshness, and the build log contains real incidents where it drifted and the practitioner caught the agent not knowing its own state (*"You are supposed to be driving!"*). The freshness rule — *update at every real pause, not merely at ho boundaries* — is the single most important operating rule this layer carries, because hands sessions generate state faster than ho closes do.

---

### Layer 2 — The public build record (kamae-4)

The bottom of the ho overview carries a **`Build record.`** section: an append-only, practitioner-facing chronological log — one entry per ho close, one per checkpoint fired and answered. Cleaner prose than Layer 1, no hashes or traps. It is the *"so the practitioner's one read stays current"* artifact, and the orientation ho names the rule explicitly: *"After each ho closes, the ho overview gets its small update — progress, checkpoint outcomes, insertions."*

It is distinct from Layer 1 by audience and altitude. Layer 1 is the agent's working memory: dense, tactical, private, mutable. Layer 2 is the human's progress ledger: narrative, strategic, public, committed, append-only. The framework's replan **checkpoints** are recorded here with the question, the evidence-based answer, and the practitioner's response — turning K4 from a pure forward plan into a plan with a running record grown onto its tail.

---

### Layer 3 — The per-ho Reflect phase

Every building ho is **ha-shaped: Think → Execute → Reflect** (codified in pālana's orientation ho). Reflect is written *before* the ho's status flips `draft → complete` — closing a ho *is* filling its Reflect. It is the durable, per-unit retrospective: what the design got right, where the review caught a slip, the one platform trap worth keeping, what was deferred and to which ho. It reads as finished history — present tense, immutable once closed (forward-only).

This is the most standard layer — Reflect is a recognizable Ho phase, matching the framework's definition (*"did the design hold, which decisions were revised on what evidence, what changes for the next ho"*). pālana uses it with unusual rigor (all 19 hos to date carry it) and with one deliberate simplification: the framework files ha-stage reflection in a *separate `devlog/` file*; pālana embeds Reflect *inside the ho document* — the ri-stage convention — for every ho. One artifact per ho, not two.

---

### Layer 4 — The ntfy alert channel

A push topic (recorded in `prompts/ntfy-topic.txt`, gitignored) that pings the practitioner's phone. **Mandatory on every stop** — session end, block, halt-on-surprise, or "the app is ready for your hands." The rule, from the memory file: *"ping ntfy EVERY time work stops … One message: what closed, what's next. Not optional."*

This is the human-in-the-loop heartbeat that makes unattended running safe. The framework assumes *"the practitioner watches the agent work … the presence is the verification."* When the practitioner is *not* watching, that presence has to be reconstructed as an asynchronous reach: the build never expects the human to poll; it contacts him when it needs hands or has stopped.

---

## The write cadence — the repeatable loop

Per ho, the continuity artifacts are written in this order:

1. **Open** — author the ho document via Kamae 5 (Think-phase decisions), `status: draft`. Note the open in Layer 1.
2. **Execute** — build (often delegated to a cheaper model), review at the top tier, commit. Log the commit hash, test count, coverage, and CI status into Layer 1 *as you go*.
3. **Hands (UI hos only)** — ntfy the practitioner (Layer 4); run live feedback rounds; capture his verbatim verdicts and any new queue items into Layer 1 after *every* round (this is where staleness bites hardest).
4. **Close** — fill Layer 3 (Reflect); flip `status: complete`; write the Layer 2 build-record entry; update Layer 1's status paragraph with a fresh `NEXT:` pointer.
5. **Stop** — ntfy (Layer 4): what closed, what's next. Always.

**Golden rules extracted from the run:**

- **Verify by command, never by assertion.** "CI is green" means the output of `gh run list`, not a sentence. The memory file records a multi-day incident where CI was red and *"nobody checked `gh run list`"* — the correction became a standing rule.
- **Update memory at every real pause, not just at ho close.** Hands sessions produce state faster than ho boundaries.
- **Sealed decisions are constitutional** — attributed, dated, never relitigated.
- **Keep the practitioner's voice verbatim** — intent survives the context wipe only if the words do.
- **The memory file drives.** The agent reconstitutes state from it and must *know* the state, not ask the human to re-supply it.

---

## How this compares to the Ho System as it stands

The framework specifies cross-session continuity through **per-ho documents**, the **Reflect phase / devlogs**, the **forward-only principle** (closed hos stay closed; supersession is explicit and bidirectional), and the **`builds-on:` frontmatter**. Its stated substrate is *"documents as memory."* Mapping pālana's four layers onto that baseline:

| pālana layer | Framework equivalent | Verdict |
|---|---|---|
| **L3 — Per-ho Reflect** | **Reflect phase** (fully specified) | **Conformant.** Standard Ho, used rigorously. Lone divergence: pālana embeds Reflect in the ho doc rather than a separate `devlog/` file. |
| **L2 — Build record (K4 tail)** | **Ho Overview (K4)** | **Extension.** K4 is specified as a *forward plan* ("the map, not the territory"). pālana appended a running append-only log to its tail. The framework has no accumulating build-record concept. |
| **L1 — Working-memory handoff** | *(none)* | **Invention.** The framework relies on *bounded sessions + fresh context + documents* and names no memory file as a continuity device. No framework analog. |
| **L4 — ntfy alert** | *(none)* | **Invention.** The framework specifies *stop conditions* and the *tripwired* halt-and-surface property, but no alerting channel — because it assumes continuous human presence. |

### Where pālana and the framework diverge — and why

**1. The framework assumes a human is the continuity. pālana cannot.**
The framework's model rests on two assumptions an autonomous build breaks: *"the practitioner watches the agent work … the presence is the verification,"* and *"the default posture is ask everything … staying in your own work."* pālana is the deliberate inverse — a test of whether the agent can carry the discernment itself. With the human absent between and within sessions, the two roles he normally plays had to be externalized: **Layer 1** externalizes *holding intent across sessions* (memory as the agent's held state), and **Layer 4** externalizes *being the verification presence* (the build reaches for the human instead of the human watching). Neither exists in the framework because the framework never had the gap.

**2. "Documents as memory" scales to humans; it strains for a context-wiped agent doing 70+ hos.**
The bootstrap thesis is right that a fresh agent reads the docs — but re-reading the *entire* README + system design + ho overview + relevant ho + code every session is the very context bloat the framework's own bounded-session rule warns against. Layer 1 resolves the tension: one dense file that pre-digests state, so the fresh agent reconstitutes in a single read instead of re-deriving it from the whole chain. It is *"documents as memory"* taken to its logical end — a document whose only job is to *be* the memory.

**3. Forward-only holds — and pālana adds a hot/cold split the framework does not name.**
The framework treats a finding as belonging in Reflect: permanent, immutable, forward-only. pālana runs findings through **two temperatures** — raw and mutable in Layer 1 (traps, live queues, `NEXT:` pointers that change every pause), then cooked and frozen into Layer 3 Reflect at close. Forward-only immutability applies cleanly to L2 and L3 (append-only, closed-stays-closed). L1 is deliberately *not* forward-only — a living scratchpad that gets overwritten. That is a category the framework has no name for: a **mutable working-memory tier beneath the immutable record**.

---

## Proposed framework additions

If the Ho System wants to support autonomous or long-running builds as a first-class mode, pālana has already prototyped the missing pieces. Offered as proposals, grounded in this pilot, pending validation:

1. **Name a working-memory / handoff artifact.** A designated, always-current file — mutable, private, agent-facing — distinct from the immutable Reflect record. Specify its sections (sealed decisions; live status with `NEXT:`; hard-won findings; queues; practitioner voice) and, critically, codify the **freshness discipline** ("update at every real pause"), since this pilot's own log proves staleness is the dominant failure mode.
2. **Bless a build-record convention for K4.** Sanction the append-only log at the tail of the ho overview as the human-facing progress ledger — distinct from, and beneath, the forward plan above it.
3. **Specify an alerting / heartbeat.** For any build where the human is not continuously watching, a mandatory "ping on every stop: what closed, what's next" channel, as the asynchronous substitute for presence-as-verification.
4. **Name the hot/cold finding lifecycle.** Make the two temperatures explicit: findings live *hot* in working memory, then graduate *cold* into Reflect at close.

---

## The honest bottom line

pālana did not *violate* the Ho System. Every immutable-record discipline — forward-only, Reflect, the Kamae chain, verify-by-command, sealed-decisions-as-constitution — is intact, and if anything followed harder than a human-driven build would. What pālana did was **fill the hole the framework leaves when you remove the human**, and the hole turned out to need exactly two new artifacts (a mutable working-memory handoff and an asynchronous alert) plus one small extension (the build-record log on K4). Those three are the repeatable process above, and they are precisely the parts the current framework does not yet describe.

---

*Practice report authored 2026-07-09 from the pālana build record (in progress). pālana is the third Ho System pilot and the first autonomous one; the running project lives under `sageframe-no-kaji/palana`, with its Kamae chain and public build record tracked in that repo's `ho-process/`.*
