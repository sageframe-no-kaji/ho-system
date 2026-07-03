---
id: "1.3"
title: "Glossary"
type: foundation
stage: n/a
status: stable
tags: [ho-system, glossary, vocabulary]
---

# Glossary

The Ho System's working vocabulary. Terms are defined as the framework uses them, with
the defining document named. Japanese terms give the reading first. When the corpus uses
a term two ways, the entry says so.

---

**agent task (AT)** — The executable register: a surgical, command-verifiable spec an
autonomous coding agent reads to execute one bounded unit of work. Procedural (no
architectural decisions inside), executor-portable (names its `model:`), **tripwired**
(halts and surfaces on surprise), one commit per task. Authored via dandori. Child tasks
(`Ho-NN-AT-MM.md`) descend from a ho; standalone tasks stand alone.
_(ho-task-decomposition 2.8; artifact-type-registry 2.9 §1.4; agent-task-spec.)_

**arc** — The complete sequence of hos that takes a project from idea to running system;
"a narrative of building" with a beginning (Kamae), a characteristic phase shape, and a
planned-versus-actual evolution the numbering makes legible. _(project-arc 2.2.)_

**architectural register / executable register** — The two cognitive registers a single
architectural thought is written in: the **architectural register** (the ho document —
decisions, reasoning, reflection, read by humans) and the **executable register** (the
agent task — exact files, changes, verification, read by the executor). The word
_register_ in the framework means this cognitive altitude; the design source-of-truth file
is the **Basis of Design**, not a "register." _(ho-task-decomposition 2.8 §1.)_

**Basis of Design** — The single file holding a project's landed design decisions as
explicit numeric parameters — the source of truth the tuners land into, the spikes
validate against, and the implementation ports from. Holds the _frozen_ values at any
moment (the _living_ happens in the **propagation ledger**): the file itself is living —
propagated with named-reason commits only — while each landed value is frozen until a
propagation moves it. Generalizes past visual to any
design parameter, interaction thresholds included. Named from the AEC term of art; not a
"register" (see _architectural register_). _(design-work 2.11 §2–4.)_

**builds-on** — Frontmatter field listing the upstream documents a ho or Kamae document
depends on; the cascade reading order. Opening any document pulls the context behind it by
following the chain. _(corpus-wide convention.)_

**closure** — The state change when a ho's work and reflection are done. Canonical signal:
frontmatter `status: draft → complete` (after Reflect for ha, Results for ri, authoring for
orientation), optionally with a `commit:` field; `superseded` is the second terminal state.
Forward-only applies from closure onward. _(ho-structure 2.3 §5.4.)_

**compressed chain** — The state of a Kamae chain whose layers have been combined or
skipped by a ri-stage practitioner. Legitimate only as the result of a **declared
compression** (see next); a chain compressed silently is drift.
_(kamae-project-framing 2.1 §4.)_

**constitutive child** — An agent task that cannot be understood without its parent ho: its
Context names the parent's Think-phase decisions, and its siblings decompose the same
architectural thought. The dominant AT relationship in mature practice.
_(ho-task-decomposition 2.8 §2.1.)_

**dandori (段取り)** — Preparation; staging; the carpenter's setup, the chef's mise en
place. Operationally: the discipline and format for translating a ho's resolved Think phase
into executable agent-task specs. Exists as a standalone skill (lean, no Ho System
assumptions) and embedded in the kamae-5 skill (Ho-scoped naming, paths, commit format).
The load-bearing bridge of the operating layer. _(ho-task-decomposition 2.8 §8; DANDORI.md;
FORMAT.md.)_

**declared compression** — The ri-stage move of combining or skipping Kamae chain layers,
legitimate only when **declared in writing where the next session will read it** — a note
naming what was combined and why — as opposed to silent elision. The declaration is what
keeps the compressed chain accountable. Canonical instance: sageframe-mcp ho-00; this
framework's own ho overview runs on one. _(kamae-project-framing 2.1 §4.)_

**design tuning** — The empirical design method: isolate design work into frozen sessions,
build spikes, expose values as live tuners, land them by feel against real data at real
scale, and freeze the result into the Basis of Design. Covers any sensory/experiential
design — visual, motion, interaction feel — not just visual. Part of it legitimately
happens outside the Ho chain and propagates back through frozen artifacts and numbered hos.
_(design-work 2.11.)_

**devlog** — Structured reflection written after a ho; "the artifact that distinguishes 'I
built this' from 'AI built this while I watched.'" A learning journal in shu, a decision
record in ha, dissolved into the ho document itself in ri — in current practitioner-shape
documents its function is carried by the Reflect phase (ha) or Results (ri). _(devlog 2.6;
ho-structure 2.3 §5.3.)_

**dogfood** — A validation modality (d): learning the tool by _actually using it_ on a real
task, as a real user. Produces a **dogfood finding** — the practitioner's real query, the
attempts, the ranked results, the finding — which is a record (terminal on creation), not
work, and feeds later hos' Think phases. Human-run; a validation modality, not verification.
_(verification-practices 2.7 §3; artifact-type-registry 2.9 §2.2.)_

**eval (graded eval)** — A validation modality (c): a human-graded pass on _output quality_
— correctness, clarity, judgment — that needs discernment rather than a pass/fail robot.
Produces a graded-eval record (a `type: eval-handoff` + `type: eval-results` pair). What
earlier drafts mislabeled a "smoke test." _(verification-practices 2.7 §3;
artifact-type-registry 2.9 §2.1.)_

**forward-only** — Closed hos stay closed; closed decisions stay on the record. The response
to superseded work is a new document that names what it supersedes — never a reopening,
rewrite, or retroactive edit. Typographical fixes are the only exception. Governs the
**frozen** and **sealed** mutability regimes; **living** is the declared exception.
_(ho-structure 2.3 §3.5; operating-discipline.)_

**ha (破)** — "Break from the form." The middle shu-ha-ri stage: the practitioner scopes
their own work and makes decisions inside a Think → Execute → Reflect structure. Also the ho
shape for building sessions where architectural thinking precedes specifying. _(shu-ha-ri
2.4; ho-shape-templates.)_

**ho (歩)** — "Step." The fundamental unit of work: bounded, deliverable, traceable,
reflective, sequenced (the five invariants). A session of work that produces something real
and leaves a record. _(ho-structure 2.3 §1.)_

**ho document** — The Kamae 5 artifact: the bounded scope for a single working session, at
the architectural register. Lives at `ho-process/hos/ho-NN-slug.md`, carries shape,
decisions, scope boundaries, verification gates, and Reflect. _(kamae-5 skill;
ho-shape-templates.)_

**ho overview** — The Kamae 4 document: the build's directional plan — phases first, hos
within phases, decisions inline with the ho that resolves them, replan checkpoints, release
tags at phase boundaries. A living document; the map, not the territory.
_(kamae-project-framing 2.1 §2.4; kamae-4 skill.)_

**ho-00** — The project's first session: an orientation-shaped ho covering pre-conditions,
concept primers, project ho-shape conventions, and handoff to the first building ho.
_(ho-shape-templates; sharibako/sageframe-mcp ho-00 instances.)_

**ho 0.5** — The conventional decimal slot for an inserted ho between the overview and the
first planned major: "a step that wasn't in the original plan and logically precedes the
next major." _(ho-structure 2.3 §3.3.)_

**ho-process/** — The project directory holding the Ho System's artifacts: the Kamae
documents, `hos/`, `agent-tasks/`, and any project-specific layers (`learning/`, `notes/`,
`ideas.md`). The README lives at repo root, not here. _(kamae-project-framing 2.1 §2.6.)_

**interaction test** — A validation modality (b): a human, hands on a real terminal,
checking function _plus feel_ — the UI/UX a smoke test structurally can't reach (the delta
between clean chosen input and a real human's messy TTY). Produces commits, no document — a
practice, not an artifact type. _(verification-practices 2.7 §3.)_

**Kamae (構え)** — The ready stance: everything that happens before (and framing around) the
build. Produces the five-document chain — seed (K1), system design (K2), README (K3), ho
overview (K4), per-ho documents (K5) — each step increasing commitment: opinions → decisions
→ scope → sequence → session. _(kamae-project-framing 2.1.)_

**Kamae addendum (kamae-N.M)** — A decimal-suffixed decision document that supersedes a
named part of a frozen Kamae document without editing it; carries the reopening
justification, the declined alternatives, and the propagation record. Forward-only applied
to architecture. _(kamae-addenda 2.10; sharibako kamae-2.1/2.2.)_

**kagen (加減)** — The felt-right _target_ of a design parameter: its right degree, found by
feel — "does this have the right kagen yet?" What a **landing ho** lands values at, the goal
state of a **design tuning** by-feel pass. _(design-work 2.11 §2; artifact-type-registry 2.9
§3.3.)_

**kokoroe (心得)** — The internalized discipline of care the executing agent operates under:
context-first, spec as authorization, verify by command, halt and surface, honor the
boundary. Installed at project level via CLAUDE.md so specs can stay tight. _(KOKOROE.md.)_

**landing ho** — A ho whose deliverable is landing visual/behavioral parameter values at
their **kagen** by feel against real data, locked by a commit — judgment work, not feature
work. Named for the _act_ (landing), not the instrument (tuner): the act survives without
literal sliders. _(artifact-type-registry 2.9 §3.3; design-work 2.11 §2.)_

**learning walkthrough** — A plain-language, retrospective post-execution walkthrough of
what a ho actually built, addressed to the practitioner **as learner** — the learning layer
separated from the architectural record. `type: learning-walkthrough` (supersedes the
earlier `teaching-note` token: "learning," not "teaching," centers the practitioner as
learner rather than the document as teacher). _(artifact-type-registry 2.9 §4.1.)_

**mind / hand** — The framework's central operating principle: one architectural thought
held at two registers — **mind** (the thinking: the Kamae chain at project scale, the ho's
Think phase at session scale) and **hand** (the doing: the Kamae-5/per-ho layer at project
scale, the agent task at session scale). The thinking is extracted into a durable artifact
so the hand can be delegated — cheaper model, later session, agent — without re-litigating
the mind. Latin epigraph: _mens et manus_. Diagnostic: _a hand problem_ is execution
fighting you with the design sound; _a mind problem_ is the design itself being wrong.
_(ho-task-decomposition 2.8 §1.1.)_

**mutability (living / frozen / sealed)** — The axis naming how an artifact changes over its
life. **living** — revised in place (README, overview, seed by dated revision, the Basis of
Design). **frozen** — not edited in place, changed only by a superseding addendum (system
design); thaw-able through the addendum mechanism, still governs the build. **sealed** —
final, never changes (closed hos, sidequests, addenda, dogfood findings, devlogs/Reflect,
executed ATs, propagation-ledger commits); a future document may _respond_ but nothing
supersedes it in force. Forward-only governs frozen and sealed; living is the declared exception.
_(artifact-type-registry 2.9 §6.)_

**notes/** — The `ho-process/notes/` location where pre-ho findings live (dated finding
documents) before any ho consumes them via `builds-on:`. A location, not a first-class
artifact type. Its complement is `ideas.md`: **notes/ is evidence behind you; ideas.md is
intentions ahead of you.** _(kamae-project-framing 2.1 §2.6.)_

**orientation ho** — The ho shape for ho-00, learning-only sessions, and replan checkpoints:
framing without execution — pre-conditions, concept primers, project shape, handoff. No
Think/Execute/Reflect; "the whole document IS the framing." _(ho-shape-templates.)_

**parti** — Architecture's term for the core organizational idea every design decision is
evaluated against. The seed is the parti of the project — the evaluative reference all
downstream decisions answer to. _(ho-seed-template; kamae-project-framing 2.1 §2.1.)_

**phase** — Used at three scales, context-disambiguated: (1) _arc phases_ — the universal
progression (orientation → foundation → construction → integration → operations); (2)
_overview phases_ — project-specific clusters of hos sharing a theme, the primary cognitive
unit of Kamae 4, each ending in a release tag; (3) _session phases_ — Think, Execute, Reflect
inside a ha ho. _(project-arc 2.2; kamae-4 skill; ha template.)_

**precedential thinking** — The discipline of understanding what exists in a problem space
before proposing to build in it; the research posture that feeds the seed.
_(ho-seed-template.)_

**propagation ledger** — The running record of propagation commits that update the **Basis
of Design**, each naming the reason the value moved. Where the _living_ happens for design
work; git holds it; sealed per commit. _(design-work 2.11 §2–4.)_

**Reflect** — The post-execution phase of a ha ho (and by extension the reflective close of
any session): did the design hold, which decisions were revised on what evidence, what
changes for the next ho. The durable record of discoveries that crossed back from execution.
_(ha template; ho-task-decomposition 2.8 §6.)_

**register** — See **architectural register / executable register** (the cognitive
altitude). The design source-of-truth file is the **Basis of Design**, not a register.

**release tag** — The tagged commit at a phase boundary (v0.1 … v1.0): each completed phase
produces a release, forcing a "done" judgment and a rollback point. _(kamae-4 skill.)_

**replan checkpoint** — A named pause in the overview where the practitioner stops, evaluates
against real evidence, and continues, inserts, or replans. Three kinds: phase-boundary,
reality-check, decision-trigger. Orientation-shaped when it produces a document. _(kamae-4
skill; sharibako kamae-4's three checkpoints.)_

**ri (離)** — "Transcend the form." The stage where structure is internalized; and the ho
shape for well-defined work: Problem → Solution → Changes → Results. _(shu-ha-ri 2.4;
ho-shape-templates.)_

**seed** — The Kamae 1 document: problem, landscape, vision, audience, identity, constraints,
scope boundaries, architectural opinion, self-assessment. The parti; a living document
revised deliberately with dated notes. _(ho-seed-template; kamae-project-framing 2.1 §2.1.)_

**shape** — The structural form of a per-ho document: orientation, ha, ri, or shu. Chosen by
ordered questions at authoring time; determines the document's sections. A shape is not a
stage label — it describes the document, not the practitioner. _(ho-shape-templates.)_

**shu (守)** — "Follow the form." The learner stage with author-prescribed structure; as a ho
shape, rare in architectural-authority practice — used when handing the framework to a
learner. _(shu-ha-ri 2.4; ho-shape-templates.)_

**shu-ha-ri (守破離)** — The three-stage progression model by which the methodology's
structure adapts to practitioner development: follow the form, break from it deliberately,
transcend it. Domain-local — a practitioner can be ri in Python and shu in Docker
simultaneously. _(shu-ha-ri 2.4.)_

**sidecar** — A record kept in a sibling directory outside a fork's working tree, separately
owned, so the practitioner's ho-process never enters the contribution's object database. The
pattern is the **sidecar directory**; the artifact is a **sidecar record**.
_(external-contribution 2.12.)_

**sidequest** — A bounded, severable capability _arc_ inside a project: its own folder, its
own phase-overview (`type: ho-overview-phase`), local letter-prefixed numbering (ho-A1…), an
explicit `supersedes:` against the main overview (bidirectional), its own validation pass
(in shodo's case, an eval). It is _work_, an arc — the counterpart to the dogfood finding's record. (The earlier "sidequest =
emergent record" definition was a mislabeling, now corrected — those files are dogfood
findings.) _(artifact-type-registry 2.9 §3.1.)_

**smoke test** — A validation modality (a): the **agent** walks through the real _function_
(not a code/unit test) and confirms it works. Agent-run and therefore a _floor, never a
verdict_ — "a passing smoke test is not validation; it's the floor a human interaction test
has to clear." Produces commits, no document — a practice, not an artifact type.
_(verification-practices 2.7 §3.)_

**spike (design)** — A hand-built artifact validating the frozen Basis of Design against the
real algorithmic constraints before production code; the reference target the implementation
ports rather than copies. A/B spikes test a second design variant as a toggleable
alternative. _(design-work 2.11 §2.)_

**splits-from** — Frontmatter field on a ho created by splitting an oversized ho (ho-02 →
ho-02.1/.2/.3), pointing at the split parent. _(sutra ho-02.x; shoshin ho-07.2.)_

**standalone agent task** — An AT outside any ho: maintenance, surgical fixes between hos,
pre-ho exploration. Fully self-contained; `type: standalone-agent-task`; no parent fields.
Filename `Standalone-AT-YYYY-MM-DD-slug.md`. _(ho-task-decomposition 2.8 §2.3, §4.2;
FORMAT.md.)_

**supersedes** — Frontmatter field (and body discipline) naming exactly what an artifact
overtakes — section-precise for Kamae addenda, decision-precise for hos. Supersession links
are **bidirectional**: the new document names what it supersedes, the older carries a pointer
back. The supersession is part of the record, never hidden in an edit. _(ho-structure 2.3
§3.5; kamae-addenda 2.10 §3; artifact-type-registry 2.9 §3.1.)_

**Think / Execute / Reflect** — The fixed three-phase structure of a ha ho: decisions
documented before building; the build (usually an index of agent tasks); honest
post-execution evaluation. "Phase 1 must produce a documented decision before Phase 2
begins." _(ha template; ho-task-decomposition 2.8 §5.)_

**tiered understanding** — Calibrated comprehension in three declared tiers: Tier 1 Black Box
(use without investigating), Tier 2 Functional (configure, troubleshoot, explain — the
default target), Tier 3 Deep (could redesign). Declaring tiers honestly is a core skill;
tiers are declared in shu, self-assigned in ha, internalized in ri. _(tiered-understanding
2.5.)_

**tripwired** — The property that makes an agent task safe to delegate: autonomous within its
bounds, but the moment it hits something unanticipated it halts and surfaces rather than
adapting. Bounded autonomy with a halt-and-surface tripwire. _Escalation_ is the mechanism;
_tripwired_ is the property — "ATs are safe to delegate because they are tripwired."
_(ho-task-decomposition 2.8 §3.3.)_

**tuner** — A live control exposing a visual or behavioral parameter — "never hard-code a
visual value before you've seen it move"; a named tuner is a named design decision. Tuners
serve the by-feel pass of a **landing ho**, which lands their values at their **kagen** and
locks them as committed defaults. _(design-work 2.11 §2.)_

**verification / validation** — The two halves of quality. **Verification** asks _did we
build it right?_ (matches spec) — tests, lint, self-review, cross-agent review, human
architectural review. **Validation** asks _did we build the right thing, and is it good?_ —
the four modalities (smoke test, interaction test, eval, dogfood). A passing verification
stack is not validation. _(verification-practices 2.7 §2–3.)_

---

_This document is part of the Ho System framework. When a concept in active use has no entry
here, either it needs a practitioner-authored name first (the naming checkpoint) or this
glossary has drifted — both are fixable._
