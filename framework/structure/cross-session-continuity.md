---
id: "2.14"
title: "Cross-Session Continuity"
type: structure
stage: n/a
status: stable
tags: [ho-system, structure, continuity, memory, handoff, autonomous, state-summary]
---

# Cross-Session Continuity

## Carrying the Thread When the Human Isn't There to Carry It

---

## 1. The Problem the Framework Leaves

The Ho System's answer to continuity across sessions is *documents as memory*. A fresh agent
arrives with no context and reads its way in: the README, the system design, the ho overview,
the relevant ho, the code. The bounded-session discipline keeps each session in clean context;
the [[devlog|Reflect phase]] (framework/structure/devlog.md) banks what each ho learned; the
forward-only principle keeps the record honest; `builds-on:` frontmatter gives the reading
order. That model works — and it rests on an assumption the framework never states out loud:
**a human carries the thread.** The practitioner remembers the last session, holds the
architectural intent, drives planning mode, and is the watching presence that catches drift.
The [[operating-discipline|operating discipline]] (practitioner/operating-discipline.md) names
this directly — *"the practitioner is the continuity,"* *"the presence is the verification."*

Remove the human from between sessions and two roles the whole model rests on vanish: **holding
intent across sessions**, and **being the verification presence**. The
pālana pilot — the first fully autonomous Ho build ([[continuity-discipline|5.3]]
(examples/palana-autonomous/continuity-discipline.md)) — ran a multi-session build where the
agent's context window was wiped between sessions and the agent itself had to reconstitute the
entire build state from written artifacts alone. It evolved a system to fill exactly that gap.
This document promotes the load-bearing parts of that system into doctrine.

The same gap opens, in smaller form, on any long human-driven build: a session six weeks in
re-reads the whole chain to remember where it was, which is the context bloat the
bounded-session rule warns against. So the pattern is not autonomous-only. It is **strongest
where the human is least present**, and it degrades gracefully to a cheap universal minimum
where the human is fully present.

---

## 2. The Two Continuities — and the Two Tiers

A build has two continuities. The **implicit continuity** is the entire cold record — git, the
per-ho Reflect, the build record, the Kamae chain itself: every well-kept artifact carries the
thread as a side effect of being what it is. The **explicit continuity** is the project's
**State Memory**, the [[kamae-project-framing|Kamae 6]]
(framework/structure/kamae-project-framing.md §2.7) document — the one document whose *only
job* is continuity, a living file that always exists at a fixed path (§10). This document
specifies the explicit continuity and the disciplines that keep it honest; the implicit
continuity needs no new doctrine — it *is* the record, already governed by forward-only.

The State Memory holds two tiers:

- **The state-summary block — the universal minimum.** Pinned at the top of the State Memory, a
  four-field block refreshed at every ho close and every session end. Cheap, always useful,
  minimum context to produce, and a fixed parseable shape that downstream automation can hook.
  Present on *every* build, attended or not. §3.
- **The working-memory body — grown by event-gated accretion.** Beneath the block, the file
  grows into a dense handoff the fresh agent reads first to reconstitute all state without
  opening code: sealed decisions, a per-ho log, do-not-rediscover traps, queues, the
  practitioner's voice. *How much* body accrues is decided by what the build actually
  encounters (§4.2); *that the file exists* is not.

The block is the spine, always present; the body is what grows when the build needs it. The
tempting mistake — the one this document exists to prevent — is to make the memory *file itself*
optional or its location negotiable. A universal block needs one fixed home, not a conditional
one. That home is K6.

---

## 3. The State-Summary Block

Every ho closes, and every session ends, with a **state-summary block**: four fields, fixed
order, fixed labels. It is the answer to *"where is this build, in one glance?"*

```
**STATE-SUMMARY**
- **COMPLETED** — what was just finished this session/stretch.
- **NEXT** — the single clear pointer to what comes next.
- **ACTION ITEMS / BLOCKS** — open items needing action, and anything blocking progress. A
  blocked build must say so loudly here (`BLOCKED: …`), never bury it. Write `none` when clear.
- **PROJECT LIFECYCLE** — `kamae` | `dev` | `beta` | `production`.
```

**The four fields are verbatim and ordered.** The block leads with the literal token
`STATE-SUMMARY` and the four labels in this sequence. This is deliberate: the block is a **hook
surface**. A greppable, fixed-shape block lets ntfy alerts, dashboards, the practitioner's
memory system, or any other tool extract build state without parsing prose. Free-form status
notes are not this; the value is in the fixed form. Keep it that way even when it feels rigid —
the rigidity is what makes it machine-findable.

**Project lifecycle** is where the *thing being built* sits in its life. It is orthogonal to
the two "stage" notions in the framework — [[shu-ha-ri|shu-ha-ri]]
(framework/structure/shu-ha-ri.md) is the *practitioner's* skill stage, and the `stage:`
frontmatter field is the *Kamae* position — and it is unrelated to the `status:` frontmatter
field, which is a *document's* own state (`draft`, `complete`, `stable`). The word "status" was
deliberately kept off this field to avoid that collision:

| Lifecycle | Meaning |
|---|---|
| `kamae` | Still in the Kamae chain (seed → design → readme → overview). Pre-build; no product code yet. |
| `dev` | Building against the ho sequence; not yet exercised by real users. |
| `beta` | Feature-complete enough to be driven by real users or the practitioner; hardening. |
| `production` | Shipped, in real use. |

**Where the block appears:**

- **At every ho close** — the last thing a ho does, alongside flipping `status: complete`.
- **At every session end** — including sessions that don't close a ho. This is a rule of the
  [[operating-discipline|operating discipline]] (practitioner/operating-discipline.md), not
  just of this document: a session ends by stating where it leaves the build.
- **At the top of the State Memory** (K6, §4), always current — the first thing a fresh
  session reads.
- **As the shape of each build-record entry** (§8) — the build record is a chronological run
  of state-summary blocks with prose around them.

Alongside the block, the **ho-status roster** — K4's other companion (the roster to the build
record's log; [[kamae-project-framing|Kamae Project Framing]] (framework/structure/kamae-project-framing.md)
§2.4) — is regenerated from ho frontmatter at every block write, *if it needs updating*: a ho
close changes a ho's `state`, so the same trigger that refreshes the block refreshes the roster.
It is a derived cache like the block itself — the cold record (git, per-ho Reflect, the build
record) wins on conflict. Until a project has a generator, the agent regenerates the roster by
hand from frontmatter; the target is script-generated, so it never drifts.

The block is the universal minimum because it survives everywhere: even a build whose State
Memory is nothing more than its spine — no working-memory body, no alert channel, a fully
present human — still benefits from ending each session with *completed / next / blocks /
lifecycle*, and pays almost nothing to produce it.

---

## 4. The State Memory File (Kamae 6)

The State Memory is the [[kamae-project-framing|Kamae 6]]
(framework/structure/kamae-project-framing.md §2.7) document: the single living file the fresh
agent reads **first**, before any code, to reconstitute the whole build state. It always exists,
at a fixed path (§10). In pālana its body is the innovation — the working memory
that made an autonomous, self-directing, multi-session build possible. Everything else in the
pattern is close to standard Ho practice; this file's body is not.

### 4.1 What it holds

Observed from the pilot, in reading order:

1. **The current state-summary block** (§3) at the top — always fresh.
2. **Sealed decisions** — practitioner rulings that supersede the written chain, each dated and
   attributed. The constitutional layer: the agent may not relitigate these. Each is banked in
   a cold canonical home at the moment of sealing (§6); the K6 copy is the cache. (Mutability
   [[artifact-type-registry|`sealed`]] (framework/structure/artifact-type-registry.md §6).)
3. **A dense chronological build log** — one entry per ho / session / hands-round, stamped with
   what closed, the commit hash, test count, coverage, CI status, the practitioner's verbatim
   reactions, and a `NEXT:` pointer.
4. **Do-not-rediscover findings and traps** — the hard-won institutional memory (platform
   quirks, tooling conflicts, fixture gotchas) that would otherwise be re-learned every session
   at cost. Compaction never touches this section (§7); of everything the file holds, it is the
   hardest to reconstruct.
5. **Queues** — deferred work parked against the ho that will pick it up.
6. **Verbatim practitioner voice** — the practitioner's laws quoted exactly, so design intent
   survives the context wipe with its register intact.

### 4.2 Accretion — how the body grows

The file always exists; what varies is how much *body* grows beneath the state-summary block.
The body is **event-gated**: each section switches on when its triggering event first occurs,
not because a mode was declared up front. The build grows the file it needs.

| Level | Body section | Switches on when… |
|---|---|---|
| **0 — floor** | state-summary block only | always — every project, from day one |
| **1** | + sealed decisions | the first practitioner ruling that supersedes the written chain lands (and is banked cold at that same moment — §6) |
| **2** | + do-not-rediscover traps | the first hard-won finding a future session would otherwise re-learn |
| **3** | + queues | the first deferred item is parked against a future ho |
| **4** | + the dense per-ho log and the practitioner's verbatim voice — paired with the alert heartbeat (§9) | the human stops watching — the only genuinely *mode-gated* tier |

The two build modes remain real — as descriptions of the common endpoints, not as the gating
mechanism:

- **Active-oversight builds** — the human is in the loop, watching, carrying much of the
  thread. Such a build typically settles at levels 0–2: the human is the redundant continuity,
  and the cold record (build record + Reflect) plus the practitioner's own memory do the rest.
  The alert heartbeat (§9) is unnecessary here.
- **Autonomous / human-as-designer builds** — the human *designs* (intent, sealed decisions)
  but the agent *executes* unattended, across context wipes, with no one watching. The full
  body holds the human's two vanished jobs here (holding intent between sessions, being what
  the build reconstitutes from), paired with the alert heartbeat (§9) as the substitute for
  presence. This is the mode the pālana pilot proved.

What is not gated by anything: the file exists on every build, its state-summary block is
always fresh (§5), and it obeys the hot/cold authority rule (§6).

---

## 5. Freshness — the Dominant Failure Mode

A stale working-memory file is not neutral. It is **worse than no file**, because the agent
reconstitutes wrong state and acts on it *confidently* — the pilot's own log records incidents
where the file drifted and the agent no longer knew its own build state. Staleness, not size,
is the failure mode that kills the pattern.

Three disciplines keep it honest:

1. **Update at every real pause — not merely at ho close.** Hands-on and interactive stretches
   generate state faster than ho boundaries do; the file must be refreshed whenever work stops,
   not only when a ho completes. (At the accretion floor — a block-only file, §4.2 — freshness
   *is* the block refresh; there is nothing beneath it to go stale.)
2. **The mandatory session-end state-summary is the forcing function.** A session cannot end
   without emitting the block (§3), and emitting it refreshes the top-of-file state. The
   universal rule and the freshness rule are the same rule seen twice.
3. **Blast-radius containment through the hot/cold rule (§6).** Because the file is
   non-canonical, a reconstituting session verifies anything the build depends on against the
   cold record *by command* — `git log`, the Reflect, `gh run list` — rather than trusting the
   hot file blindly. Staleness that slips through is caught at the canonical boundary.

**If you cannot commit to keeping it fresh, do not keep the file.** A handoff you don't trust
is a liability, and half-trust is worse than none — you stop verifying exactly when you should.

---

## 6. Authority — Hot and Cold

The working-memory file duplicates state that also lives in the build record, the Reflect
phases, and the git log. That duplication is only safe under an explicit authority rule:

> **The working-memory file is HOT: mutable, non-canonical, a fast-pickup cache derived from
> and subordinate to the cold canonical record.** The cold record — the git history, the per-ho
> **Reflect**, and the **build record** (§8) — is the source of truth. When the hot file and the
> cold record disagree, the **cold record wins**, and the hot file is corrected to match.

The hot file is an *index* of state optimized for fast reconstitution, never the record itself.
This is what keeps a mutable first-read file from becoming a shadow source of truth that cuts
against forward-only.

**Sealed decisions bank cold at the moment of sealing.** The one thing K6 carries that is *not*
derived from the cold record — a practitioner ruling that supersedes the written chain — is
recorded in a cold canonical home the moment it is made: a [[kamae-addenda|Kamae addendum]]
(framework/structure/kamae-addenda.md) against a frozen document, a dated seed revision, a new
ho, or a build-record entry, whichever the ruling's target demands. The K6 copy is a **cache of
that record**, never its sole home: K6 holds zero original authority, so there is nothing the
cold-wins rule could erase. And *sealed*
means here exactly what it means everywhere in the framework
([[artifact-type-registry|artifact-type-registry]]
(framework/structure/artifact-type-registry.md) §6): the ruling's text never changes, and its
force yields only to a deliberate, recorded supersession — reopening carries gravity, never
done casually. The temperature of the container does not touch the gravity of the decision.

**Forward-only applies to the cold record, and the hot file is explicitly exempt** — precisely
*because* it is not the record. Closed hos stay closed; Reflect is sealed; the build record is
append-only. The hot file, by contrast, is *supposed* to be overwritten every pause — its
`NEXT:` pointer, its live queues, its running status change constantly. This is the
**hot/cold finding lifecycle**: a finding lives *hot* in working memory (raw, mutable, tactical)
and graduates *cold* into Reflect at ho close (cooked, sealed, permanent). The two temperatures
are a feature, not a contradiction — the framework simply had no name for the mutable tier
beneath the immutable record until now.

---

## 7. Growth and Compaction — Graduated and Preserving

A file read every session eventually costs the very context it saves (pālana's crossed 70 KB
and kept climbing). It must be compacted. But the dense do-not-rediscover memory is exactly what
made the autonomous build work — **compaction must not gut it.** The rule is graduated pruning
under a single preservation invariant.

**Graduated by recency and closure:**

| Region | Treatment |
|---|---|
| **Active / current ho** | Full detail, untouched. |
| **Recent closed hos** (current phase) | Light pruning — trim redundancy only. |
| **Closed phases** (where a project has phases) | Heavier compaction — collapse the per-ho blow-by-blow into a single phase state-summary, after **verifying — not presuming —** that each ho's detail is banked in its Reflect (cold, canonical) and its build-record entry. A thin Reflect gets its lessons promoted before its narration is collapsed. |

**Never pruned, at any recency:** sealed decisions; the do-not-rediscover findings and traps
(the hard memory); the current state-summary block. These are institutional memory — they
persist in the file or graduate to a durable home, never vanish.

**The invariant:**

> Compaction removes **narration** whose lesson is already banked in a cold canonical home
> (Reflect, build record, or a findings/traps doc). It **never deletes a lesson.** Scaffold
> comes down only once the building stands. If a durable finding is not yet in the cold record,
> it must be **promoted there before** the hot log is compacted. **Reflect itself is never
> touched by compaction.**

**When compaction runs — the cadence:**

- **Refresh** — the state-summary block is rewritten at every session end and ho close (§5).
  That is upkeep, not compaction; it never touches the body.
- **Compact at phase close** — the natural boundary. The closing phase's per-ho blow-by-blow
  collapses to a single phase state-summary, after the banked-cold check above.
- **Size tripwire at session start** — if the body has grown past roughly **25–30 KB**, the
  reconstituting session compacts before working. The number is a first calibration, set from
  the one empirical data point available (pālana's file crossed 70 KB and was visibly costing
  the very context it existed to save); a second project will move it.
- **The agent compacts, at those two triggers, under the invariant — never mid-ho**, and never
  the active region.

Where K6 is versioned (§10), compaction is not even lossy in principle: the pre-compaction
state is one `git log` away.

**Where permanent traps live long-term.** Default: a **never-pruned section of the working
file**. At scale — when the traps outgrow a section, or the project wants them visible beyond
the hot file — they **graduate to an in-repo findings/traps document**, a cold canonical home of
their own. Such a doc would be a *third* kind of continuity artifact, distinct from
[[idea-log|`ideas.md`]] (framework/structure/idea-log.md) (intentions ahead of you) and `notes/`
(dated evidence behind you): durable *do-not-rediscover* institutional knowledge, neither an
intention nor dated one-ho evidence. That artifact is proposed **provisionally** here (§11) —
one pilot has proven the *need*, not yet the *shape*.

---

## 8. The Build Record (K4 Extension)

The public counterpart to the hot file. An **append-only build-record log grafted onto the tail
of the K4 [[kamae-project-framing|ho overview]] (framework/structure/kamae-project-framing.md
§2.4)** — one entry per ho close and per replan checkpoint, each entry shaped as a
state-summary block (§3) with prose around it. Cleaner than the hot file, no hashes or traps:
the human's progress ledger.

It differs from the hot file on three axes:

| | Working-memory file (§4) | Build record (§8) |
|---|---|---|
| **Audience** | the next agent session | the practitioner + future agents |
| **Authority** | hot, mutable, non-canonical | cold, append-only, canonical |
| **Altitude** | dense, tactical, private | narrative, strategic, public |

The build record turns K4 from a pure forward plan into a plan with a running record grown onto
its tail — the forward plan above, the append-only ledger below. It is cold and forward-only:
entries are added, never rewritten. The convention is specified in
[[kamae-project-framing|kamae-project-framing §2.4]]
(framework/structure/kamae-project-framing.md); this document names its role in continuity.

---

## 9. The Alerting Heartbeat

For any build where the human is **not continuously watching**, presence-as-verification has to
be reconstructed as an asynchronous reach. The build never expects the human to poll; it
**contacts the human when it stops or needs hands.**

- **Mandatory on every stop** — session end, block, halt-on-surprise, or "ready for your hands."
- **One message: what closed, what's next** — the state-summary block (§3) is the natural
  payload.
- **The channel is project-shaped** — pālana used an ntfy push topic (recorded in a gitignored
  file, phone-delivered). The mechanism is not the point; the discipline is: *the build reaches
  the human instead of the human watching the build.*

This is the direct substitute for the operating discipline's real-time monitoring when the
practitioner is absent. It is required only when the human is not present; a fully attended
build has the human already watching and needs no heartbeat.

---

## 10. Location, Privacy, and Versioning

The location *is* the rule. A universal block needs one fixed, findable home — not a directory
to search, not a per-project negotiation, not an optional file.

- **The State Memory is the Kamae 6 document at a fixed path:
  `ho-process/kamae-6-<project>-state-memory.md`** ([[kamae-project-framing|Kamae: Project
  Framing]] (framework/structure/kamae-project-framing.md) §2.6–2.7). It always exists there;
  the next session — and any hook — knows exactly where to look without being told.
- **It is private by default.** Gitignored, at the fixed path. The body is written *raw*, in
  the practitioner's actual voice — verbatim rulings, unvarnished incidents, the real register
  of the collaboration — and never for an audience. The file's value depends on that register:
  a K6 written politely is a K6 that has stopped carrying intent. Publication is a **closeout
  election**, made deliberately when the practitioner is ready: stay private, publish as-is, or
  publish a sanitized copy. It is never a posture during the build.
- **Versioning — three postures.** Private-by-default and versioned are compatible; they just
  don't live in the same repo when the repo is public:
  1. **Private repo → track it.** Versioned, private, zero extra machinery.
  2. **Public repo → gitignore `ho-process/` and version it as a nested private repo** —
     `git init` inside the ignored directory, optionally pushed to a private remote. The fixed
     path is unchanged; the public repo carries the cold public record, the private process
     repo carries the hot history. **Recommended.** The hot history is the timeline of what
     the build *believed* at every pause — forward-only's value extended to the tier that is
     allowed to be overwritten. It makes compaction lossless in principle (§7), and it is what
     a publish-the-history closeout election publishes. This framework's own repo dogfoods
     this posture.
  3. **Unversioned.** The legitimate floor. What it loses is hot *history*, never a lesson —
     the compaction invariant (§7) guarantees nothing durable lives only hot.
- **The cold record is always in-repo and tracked.** The build record lives on the K4 overview;
  the Reflect lives in each ho document. These carry forward-only and are non-negotiable. This
  is also what keeps documentation-as-bootstrap intact under a private K6: a collaborator's
  fresh clone still reads a complete public chain; K6 is an accelerator on top of it, never
  the sole memory.
- **The alert channel's configuration** (a topic, a token) is secret-adjacent and stays
  gitignored regardless (e.g. `prompts/ntfy-topic.txt`).

**One divergence from the evidence, named.** pālana kept its working memory *outside* the repo
entirely (`~/.claude/projects/<proj>/memory/`, the agent's private memory directory). The
doctrine moves it in-repo because the fixed, project-local path is what makes the block a hook
surface and the file findable by any session, tool, or collaborator without being told — and it
keeps the file private by default, which is the property the pilot's location was actually
protecting.

---

## 11. Relationship to the Framework — and the Two Modes It Serves

Mapped against existing doctrine (from [[continuity-discipline|5.3]]
(examples/palana-autonomous/continuity-discipline.md)):

| Pattern | Relationship to prior doctrine |
|---|---|
| Per-ho **Reflect** | **Conformant** — standard Ho, unchanged. |
| **Build record** on K4 | **Extension** — K4 was a forward plan; this adds an append-only ledger to its tail (§8). |
| **State-summary block** | **New, and canon** — low-risk, generalizes past the pilot, the universal minimum (§3). |
| **State Memory (Kamae 6)** | **New chain link** — the living, reflexive sixth link; the mutable tier beneath the cold record (§4, §6; [[kamae-project-framing|kamae-project-framing]] (framework/structure/kamae-project-framing.md) §2.7). |
| **Alert heartbeat** | **Invention** — the async substitute for presence-as-verification (§9). |

**The two modes this pattern serves.** The pattern is canon across both modes, and the
accretion ladder (§4.2) means neither needs declaring — the build grows the file it needs, and
only the top tier (per-ho log, voice, heartbeat) is genuinely mode-gated:

- **Active-oversight** — the human watches and carries the thread; K6 typically settles at
  levels 0–2, the heartbeat is unnecessary, and the pattern mostly formalizes the good
  session-hygiene the practitioner already had.
- **Autonomous / human-as-designer** — the human designs and the agent executes unattended;
  level 4 and the heartbeat do the continuity and verification the absent
  human cannot. This is the mode the pālana pilot proved end-to-end.

The autonomous mode is pilot-proven; the active-oversight mode's evidence is,
so far, this framework's own dogfood. What carries the always-present file on attended builds
is coherence — a universal block needs one fixed home — plus cost asymmetry: being wrong costs
one small file per project. What a second long-running build will *calibrate* — not cast into
doubt — is the size tripwire (§7) and the trigger for graduating traps into a standalone
findings doc (§7). Those are tunings within a canon pattern.

This is *"documents as memory"* taken to its logical end: a document whose only job is to *be*
the memory, plus the disciplines that keep such a document from lying.

---

## 12. Related Framework Documents

- [[continuity-discipline|pālana Pilot — The Continuity Discipline]] (examples/palana-autonomous/continuity-discipline.md) — the evidence base (5.3) this document promotes.
- [[operating-discipline|The Operating Discipline]] (practitioner/operating-discipline.md) — the session-end state-summary rule; real-time monitoring; bounded sessions; forward-only.
- [[kamae-project-framing|Kamae: Project Framing]] (framework/structure/kamae-project-framing.md) §2.4 — the build-record convention on the K4 ho overview.
- [[ho-structure|Ho Structure]] (framework/structure/ho-structure.md) §3.5 — the forward-only principle the cold record obeys.
- [[devlog|The Devlog]] (framework/structure/devlog.md) — the Reflect phase, the cold home a hot finding graduates into.
- [[idea-log|The Idea Log]] (framework/structure/idea-log.md) — `ideas.md` (intentions ahead), the neighbour a findings/traps doc is distinct from.
- [[artifact-type-registry|Artifact Type Registry]] (framework/structure/artifact-type-registry.md) §6 — the living / frozen / sealed mutability regimes.

---

_This document is part of the Ho System framework. It specifies cross-session continuity for
builds where the human does not continuously carry the thread — the universal state-summary
block and the **State Memory** (Kamae 6) that holds it, with the disciplines (freshness, hot/cold
authority, graduated-and-preserving compaction) that keep a mutable first-read file honest.
Promoted from the pālana pilot ([[continuity-discipline|5.3]] (examples/palana-autonomous/continuity-discipline.md)); serves both build modes — active-oversight and autonomous — through event-gated accretion (§4.2, §11)._
