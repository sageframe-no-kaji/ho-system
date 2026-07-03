---
id: "2.11"
title: "Design Work"
type: structure
stage: n/a
status: draft
tags: [ho-system, design, visual, modality]
---

# Design Work

## Design Tuning — the Design Modality

Some work in a Ho System project is not architecture and not implementation: it is
*design* — deciding what the thing looks like, sounds like, feels like, under
constraints an algorithm or a real corpus will impose. The practice has developed a
distinct method for this work — **design tuning** — invented in shoshin-no-sono and
adopted-with-adaptation in sutra. It covers any sensory/experiential design (visual,
motion, interaction feel), not just visual. Its defining property: **part of the work
legitimately happens outside the Ho chain**, in isolated design conversations, and
propagates back into the chain through frozen artifacts and numbered hos.

The framework sanctions this explicitly. Design conversations are the one place where
the "thinking happens in chat, conclusions land in documents" rule produces artifacts
that are neither Kamae documents nor hos — session pages, the Basis of Design, spikes —
and the modality below is how those artifacts stay accountable to the chain instead of
drifting beside it.

---

## 1. Why design work breaks the ordinary flow

For generated or data-driven visuals, the usual design flow — mock it up, approve it,
build it — fails, because the look is produced by an algorithm across configurations
nobody has seen. A mockup lies; the algorithm will do something the mockup didn't.
The modality replaces the mockup phase with steps that keep design and implementation
in continuous contact. (design-process-note, "The core problem.")

The same logic covers non-procedural cases more loosely: any design whose target is
"reads right against real data at real scale" rather than "matches the approved
image" benefits from the same structure.

## 2. The eight steps

Recorded from the primary source; the specific decisions are the project's, the
method transfers.

1. **Isolate the questions.** List every visual decision the system needs, as
   discrete questions in a deliberate order — earlier questions constrain later ones
   (alphabet → grammar → signature; typography after the things it labels are
   frozen). Later sessions name earlier winners by reference; they do not restyle
   them.
2. **One question per session, in isolation.** Each question gets its own design
   conversation — no code context, no schema, no architecture. The prompt is written
   in advance and given exactly as written: one question, hard constraints, explicit
   axes to vary. Return format: labeled variants (A–D) on one page with one-line
   captions, then a recommendation. Anything belonging to a later question goes in
   one line under "parked." Isolation prevents scope-creep; the pre-written prompt
   means the session judges answers instead of defining questions.
3. **Freeze each decision in the Basis of Design.** The winning variant is extracted
   into the **Basis of Design** file (`design/basis-of-design.html`; historical
   instances: `visual-register.html`) with **frozen parameters — numbers, not
   descriptions**. The frozen parameters are the implementation spec. A frozen decision
   can be *propagated* (updated when a later coherence check reveals a problem) but only
   with a commit that names the reason — never silently. That running trail of
   named-reason propagation commits is the **propagation ledger**.
4. **Coherence check before code.** Once the core alphabet is frozen, assemble all of
   it in one hand-built scene. Elements that read right in isolation interact as a
   system; the check is where the interactions surface (shoshin: solo-peak contour
   weights 0.85/1.7 read too heavy in a multi-peak field → lightened to 0.5/0.95,
   explicitly).
5. **Design spike in the actual technical environment.** One spike validating the
   frozen Basis of Design against the real algorithmic constraints, with stand-in data. The
   spike is the reference artifact — "the target is what the spike produced."
   Implementation ports the spike; it doesn't copy it.
6. **Implement with every visual parameter exposed as a tuner.** Never hard-code a
   visual value before you've seen it move. Live slider controls serve the by-feel
   pass and force parameters to be named and isolated in the code — a named tuner is
   a named design decision.
7. **By-feel landing pass against real data.** With the algorithm on real data and
   parameters exposed, the practitioner moves sliders until the result reads right —
   judgment work, not specification work, done at the actual corpus and actual scale.
   The target is the parameter's **kagen** (加減) — its felt-right degree. Landed values
   are logged in the ho document and committed as defaults ("landing — lock the
   practitioner's by-feel pass as the field defaults"). A ho whose deliverable is
   landing values at their kagen, locked by a commit, is a **landing ho** (artifact-type
   registry §3.3).
8. **New visual modes are A/B spikes, not replacements.** A second design variant ships
   as a toggleable alternative and has to earn its place. (shoshin: hachure shipped as
   `?render=hachure`, then promoted to an independent simultaneous layer once the A/B
   showed the registers were complementary.)

**Governing principle** (from session 1, recurring throughout): *character comes from
process, not decoration.* It has architectural teeth — it rules out effects that
can't be generated algorithmically and rules in effects that are natural byproducts
of the data and algorithm. "Does this look good?" becomes "does this look like what
the algorithm is actually doing?"

## 3. How the modality attaches to the Ho chain

Three attachment points keep out-of-chain design work accountable:

1. **A kamae-4 decision-trigger checkpoint** placed where design questions block
   downstream build (shoshin: the only non-phase-boundary checkpoint in the project,
   after ho-04). The overview names which hos are blocked until which design
   questions freeze.
2. **The design sessions themselves may BE a ho slot executed outside the chain.**
   shoshin's ho-04 has no ho document; kamae-4 defines it as pure Claude Design work
   whose artifacts live in `design/claude-design/` (session pages, exports,
   coherence-check, spike). Downstream hos cite the design files directly in
   `builds-on:`. This is sanctioned: the record is the register and the session
   artifacts, not a ceremonial ho document restating them.
3. **Propagation returns via numbered hos** — implementation hos consume the frozen
   Basis of Design; decimal-inserted landing hos (06.5, 07.5) land the by-feel values
   inside the chain where forward-only and commit-traceability apply.

Adaptation is expected (sutra ho-02.1): output medium changes with the project (HTML,
not SVG — "Sutra is HTML"), tuner scope narrows to what's genuinely by-feel
(interaction thresholds, not every visual parameter), and session grouping follows
the project's layer structure (sutra: ten sessions in alphabet / grammar / signature
layers). The eight steps are the spine, not a ritual.

## 4. Artifacts the modality produces

| Artifact | Role | Lives at | Mutability |
|---|---|---|---|
| Session prompts | Pre-written one-question briefs | `design/claude-design/` | sealed |
| Session pages | Rendered variant sheets (A–D + recommendation) | `design/claude-design/` | sealed |
| **Basis of Design** | Frozen decisions as numbers; the implementation spec | `design/basis-of-design.html` | living — propagated with named-reason commits only |
| **Propagation ledger** | The named-reason commit trail updating the Basis of Design | git history | sealed (per commit) |
| Coherence check | Hand-built all-frozen-elements scene | `design/claude-design/` | sealed |
| Design spike | Basis of Design validated against real constraints; the reference target | `design/claude-design/` | sealed |
| Tuner panel | Live parameter controls in the implementation | app code | living until landing |
| Landing commits | By-feel values locked as defaults | git history + ho doc | sealed |

The **Basis of Design** is a genuinely new artifact category — the source of truth for
landed design decisions, the seam between out-of-chain sessions and in-chain
implementation. It holds the *frozen* values at any moment; the *living* happens in the
**propagation ledger**, the named-reason commit trail that updates it. It plays the same
role for design decisions that the kamae-2 system design plays for architectural ones,
with the propagation rule standing in for the addendum mechanism.

## 5. Should `shape: design` exist? — Recommendation: no

The evidence says design work already decomposes into existing shapes plus
out-of-chain sessions: the design sessions are outside the chain (no ho document at
all); tuner landings are ri-shaped (problem/solution/changes/results fits "move
sliders, land values, lock commit"); implementation-from-register hos are ordinary
ha. A fifth shape would blur the shape system's clean question ("what structure does
this session's document need?") to encode a *content domain*, which shapes don't
otherwise do. What's needed instead is this doctrine document plus the design-ho
template (§7) for the landing/session-wrapper cases. If a third project's design
work refuses to fit, revisit.

## 6. The `ho-kamae-design-collaborator` skill — actively tracked, not yet built

A design-work skill is on the roadmap — an active intention, not a "revisit someday."
A skill would add value at steps 1–2 (interrogating the question list, generating the
isolated session prompts in the project's constraint vocabulary) and step 3
(maintaining the Basis of Design), and it may be a *looser* skill than the four Kamae
collaborators rather than a strict authoring skill. It is deliberately not built yet:
two projects in, with the second adapting freely, building now risks freezing the
pattern before ri has had its say — its shape will be legible once a third project
runs the modality.

Until it exists, **this doctrine document plus the `~/.claude/modules/design-work.md`
module are the interim reference — a clear, usable stand-in for the skill, not a
placeholder.** The modality runs by hand from them: the pre-written session prompts and
the register discipline are cheap to execute directly. The skill, when built, will
automate that hand-work; it will not replace the doctrine.

## 7. The design-ho template

For the in-chain wrapper cases — tuner landings, and design-session batches that DO
want a ho document — use the **design-ho template**
(`framework/templates/design-ho-template.md`, INDEX 3.9). It carries ho-document
frontmatter with `shape: ri` (landings are ri; session-batch wrappers may be ha) and
sections for the questions in scope, the hard constraints (palette, reproducibility,
medium, out-of-scope), the parameters frozen at session end (numbers, not
descriptions), parked items, the landing record (tuner landings only), and any
register propagations. The skeleton is complete; section prose fills in per project.

## 8. The `~/.claude/modules/design-work.md` module

A small `design-work.md` module — imported per-project like the language modules —
carries the session-behavior rules an agent needs *while implementing*: the eight steps
in compressed form, the propagation rule (never silent), the
never-hard-code-a-value-before-it-moves rule, and the A/B-not-replacement rule. These
are steps 6–8, which happen in the IDE; the doctrine above is reference, not always-on
context, so it stays out of the module. The module is practitioner-scope and lives
outside any single project's repo (`~/.claude/modules/`).

---

_This document is part of the Ho System framework. Primary source:
`shoshin-no-sono/design/design-process-note.md`; adaptation evidence: sutra ho-02.1.
The specific decisions are those projects'; the method is general._
