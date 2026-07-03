---
id: "2.9"
title: "Artifact Type Registry"
type: structure
stage: n/a
status: stable
tags: [ho-system, structure, artifacts, registry]
---

# Artifact Type Registry

## Every Document Type the Practice Produces

The Ho System is an artifact-driven methodology: agents and practitioners conform to
what the documents encode, so the documents themselves need a type system. This
registry enumerates every artifact type the practice produces, what each is for, who
authors it, what its lifecycle is, and where it lives. The Kamae chain documents the
*sequence*; this registry documents the *taxonomy*.

A type earns a registry entry when it has a distinct purpose, a distinct lifecycle,
and at least one committed instance in a real project. Types observed in only one
project are marked **provisional** — recorded so the next project that needs the
pattern finds it, not yet canonical. A provisional type carries a **tier** telling the
next practitioner what to do with it:

- **Canonical-provisional** — a real, reusable pattern seen once; *adopt on the second
  use*.
- **Light mention** — recorded so it's on the map, but not encouraged.
- **Historical variance** — recorded so the history is legible; *do not reproduce*.

---

## 1. The core types

### 1.1 Kamae documents (Kamae 1–4)

**Purpose:** the project-scale thinking chain — seed (parti), system design
(architecture), README (scope as public face), ho overview (build sequence).
**Authored by:** the practitioner, in discursive sessions (chat-primary), usually via
the four Kamae collaborator skills.
**Lifecycle:** drafted → committed. Mutability varies by document — see §6.
**Location:** `ho-process/kamae-<N>-<project>-<doctype>.md`; the README is the repo
root `README.md` with no prefix (see kamae-project-framing §2.6).
**Frontmatter (observed canonical form, sharibako):** `created`, `status`, `type`
(`seed` | `system-design` | `readme` | `ho-overview`), `project`, `stage: kamae-N`,
`kamae-chain`, `builds-on`, `next`.

### 1.2 Kamae decision addenda (kamae-N.M)

**Purpose:** a mid-build architectural decision that supersedes part of a frozen Kamae
document without mutating it. Forward-only applied to architecture.
**Authored by:** the practitioner, in a discursive session, when new evidence reopens
a committed decision.
**Lifecycle:** decided → permanent record ("not to be edited except for typographical
fixes").
**Location:** `ho-process/kamae-<N>.<M>-<project>-<slug>.md`.
**Frontmatter:** `type: decision`, `stage: kamae-N.M`, `supersedes:` (section-precise,
e.g. `kamae-2 §7 (partial — the "no runtime injection" commitment)`), `builds-on:`,
`next:` (the downstream documents the decision is reflected into).
**Instances:** sharibako kamae-2.1 (injection), kamae-2.2 (ownership).
Full doctrine: the Kamae Addenda structure document (framework 2.10).

### 1.3 Ho documents (Kamae 5)

**Purpose:** the bounded scope for one working session; the architectural register.
Carries decisions, scope boundaries, verification gates, deferred discoveries, and
post-execution reflection.
**Authored by:** the practitioner via `ho-kamae-5-authoring-collaborator`; before the
work begins (edelmore reader ho-01 records a retroactive authoring and names it as an
exception).
**Lifecycle:** draft → executed → `complete` (or `superseded`). Closure is signaled by
the `status:` field — see ho-structure §5.4 (merge-decisions D2).
**Location:** `ho-process/hos/ho-<NN>-<slug>.md`.
**Shapes:** orientation, ha, ri, shu (per ho-shape-templates).
**Frontmatter (canonical form, sharibako/shoshin/sutra):** `created`, `status`,
`type: ho-document`, `project`, `ho: "NN"` (quoted), `kamae: 5`, `shape`,
`builds-on:` (list), `agent-tasks:` (list of child AT filenames), optionally
`splits-from:` (ho splits: sutra ho-02.x, shoshin ho-07.2).

### 1.4 Agent tasks (ATs)

**Purpose:** the executable register — a surgical, command-verifiable spec an
autonomous coding agent reads to execute one bounded unit of work. No architectural
thinking inside; all decisions were extracted to the parent ho's Think phase.
**Authored by:** the kamae-5 skill's embedded dandori toolkit (child tasks) or the
practitioner directly via the standalone dandori skill (standalone tasks).
**Operational properties (the four that define the type):**

1. **Procedural.** The AT contains no undecided architecture. If specification
   requires a decision, the decision belongs upstream in the ho.
2. **Executor-portable.** Because there is no thinking in it, the AT can be dispatched
   to a cheaper model, a subagent, or any executor that reliably follows a procedural
   spec. Every AT names its executor in the `model:` frontmatter field — an unset
   model is an unmade decision. This is the operating discipline's model-choice-by-task
   principle made concrete.
3. **Escalating.** The AT is autonomous *until it finds something new*. A surprise —
   schema mismatch, wrong assumption in the spec, unanticipated dependency — halts the
   run and surfaces the finding to the practitioner. No silent architectural decisions
   inside an AT (kokoroe guideline 4, "Halt and surface"; `Stop Condition` sections
   name the anticipated surprises, the rule covers the unanticipated ones).
4. **One commit.** Each AT lands as a single commit, prefixed `ho-NN:` for child
   tasks, referencing the spec ID. The git log reads as a sequence of ATs grouped by
   ho — the split's visible signature.

**Lifecycle:** `ready` → `in-progress` → `complete` | `blocked`.
**Location:** `ho-process/agent-tasks/Ho-<NN>-AT-<MM>.md` (child; the canonical
sibling-of-`hos/` placement per kamae-project-framing §2.6 and dandori FORMAT.md).
Standalone tasks share the directory as `Standalone-AT-YYYY-MM-DD-slug.md`
(merge-decisions D3; see ho-task-decomposition §4.2).
**Frontmatter:** `created`, `type: agent-task` (child) or `type:
standalone-agent-task`, `project`, `parent-ho: "NN"`, `task: "MM"`, `model`, `status`.
The child binding matches ho-task-decomposition §4.1 (`parent-ho` + `task`); the
earlier `parent:` path form is superseded.

### 1.5 Devlogs / Reflect

**Purpose:** structured reflection, the primary evidence of learning. In current
ri/ha-stage practice the devlog has dissolved into the ho document's Reflect phase
(per ho-structure §5.3); standalone devlog files appear in shu-stage and older
projects (satori's `devlog/` directory, kanyo pilot).
**Lifecycle:** written at close of work; never revised (historical record).

---

## 2. Validation artifacts

The **validation** layer — the "is it *good*?" half of quality, paired with the "is it
*right*?" verification stack — is defined in full in [[verification-practices|Verification
Practices]] (verification-practices.md) §3. It runs in four modalities across two axes (*who
runs it*: agent vs human; *what is judged*: function → feel → output-quality → real-use):
**smoke test**, **interaction test**, **eval**, and **dogfood**. Only two of the four produce
documents and therefore earn a registry entry — **eval** (§2.1) and the **dogfood finding**
(§2.2). **Smoke and interaction are practices**: they produce commits, no document, so they
live in 2.7 §3, not here. (This answers the old "does smoke earn a registry entry?" — no; it
earns a place in the validation layer, not the artifact map.)

**Altitude note (canon layer, provisional types).** The validation *layer* is canonical
doctrine (2.7 §3, merge-decisions **D19**) — it no longer waits on a second project. The two
artifact *types* below sit at a separate altitude: both are still single-project (shodo), so
by this registry's own evidentiary rule they carry the **canonical-provisional** tier — adopt
on the second use. The layer is settled; the document formats are not yet corpus-proven.

A correction is baked into this section (merge-decisions **D14/D15**, superseded in part by
**D19**). An earlier audit read shodo's `ho-04-smoke-sidequest-*` files as a canonical
"sidequest = record" artifact type. They are not: they are **dogfood findings**, and
"sidequest" is a *different* type entirely — a severable build arc (§3.1). The `sidequest`
token in those filenames is a naming collision to drop. D19 further corrects the D14/D15
framing itself: this is not "one discipline, two artifacts" but four independent modalities,
of which only two are artifacts.

### 2.1 Eval (graded eval) *(canonical-provisional — adopt on 2nd use)*

**Purpose:** a graded pass on *output quality* — correctness, clarity, judgment — the
validation modality that needs human discernment rather than a pass/fail robot (2.7 §3). A
regression floor plus a new-capability set, with a grader/observer split and a corpus-delta
check. Two paired documents in practice: a self-contained *handoff* framing the pass, and a
*results* document recording graded outcomes.
**Frontmatter:** `type: eval-handoff`, `type: eval-results`.
**Location / instances:** shodo main arc and Subproject-A; the instances carry the historical
`smoke-handoff` / `smoke-results` tokens — new instances use `eval-handoff` / `eval-results`
(the `smoke-` token is dropped, as `smoke-sidequest-` was). Produced by the project-local
`ho-smoke-collaborator` skill — a promotion candidate, re-scoped to the validation modalities
it serves (IDEA-004); it self-describes as "a sibling to the Kamae authoring skills: it feeds
Kamae 5's Reflect phase without writing it."

> **Name correction (D19).** This entry is what earlier drafts mislabeled a "smoke test." In
> the validation layer (2.7 §3) the *smoke test* is the agent-run function check — a
> *practice*, not an artifact — while this human-graded output-quality pass is the **eval**.

### 2.2 Dogfood finding *(canonical-provisional — adopt on 2nd use)*

**Purpose:** a real-use finding — the practitioner's actual query *as a user*, the attempts,
the ranked results, and the finding that falls out (2.7 §3, modality *d*). Documents a real
hunt, not planned work; nothing to execute. Named after the *finding*.
**Lifecycle:** `recorded` — terminal on creation. Never "completed"; it is evidence, and its
findings feed later hos' Think phases.
**Location:** `ho-process/hos/`. shodo's instances are the mislabeled
`ho-<NN>-smoke-sidequest-<M>-<slug>.md` files; the `smoke-sidequest` token predates this
correction — new instances drop `sidequest`.
**Instances:** shodo (5 with frontmatter + 2 pre-convention drafts).

> The earlier "sidequest frontmatter schema" open question (two variants in shodo) is
> **withdrawn**: it analyzed the dogfood-finding schema, not the §3.1 sidequest type.

---

## 3. Structural-extension types

### 3.1 Sidequest

**Purpose:** a bounded, severable capability chain *inside* a project — a full
feature/build developed within the project that wants its own ordered arc without
becoming a separate project ("the work is a capability, not a separate project — no
separate seed, system design, or README," shodo Subproject-A phase overview). It is
*work*, an arc — not a record. shodo's ingest/harvest engine (Subproject-A-Harvester)
is the canonical example.
**Structural signature:**

- Its own folder under `ho-process/hos/` (`Subproject-A-Harvester/`)
- Its own phase-overview document: `type: ho-overview-phase`, `stage: kamae-4`,
  `subproject:` field — a mini-Kamae-4 fragment with local scope
- Local ho numbering with a letter prefix (ho-A1 … ho-A5)
- An explicit `supersedes:` clause against the specific main-kamae-4 bullet the
  sidequest overtakes, plus a documented cascade reading order
- Its own validation pass (in shodo's case, an eval) and release-tag consequences

**Bidirectional supersession (required).** The supersession link must run *both* ways
(merge-decisions **D7**): the sidequest's phase overview names the main-kamae-4 bullet
it supersedes, and the main kamae-4 carries a forward pointer back to the sidequest.
shodo's phase overview claims the main kamae-4 cross-references the sidequest; it does
not — the link exists only from the sidequest's end. Since kamae-4 is a living document
(§6), adding the forward reference is *required*, not optional; a one-way link is the
drift the encoded-environment thesis exists to prevent.
**Instances:** shodo (Subproject-A-Harvester). shoshin-no-sono's ho-A branch is a
lighter variant — letter-prefixed hos with `parent:` and a `kind: sidequest`
frontmatter field, no phase overview. With the taxonomy corrected, shoshin's
`kind: sidequest` label is now *consistent* with this type (an emergent build arc), not
the collision the prior audit flagged.

**The distinction that matters, stated once:** a **sidequest** is a planned, severable
*arc* of work — it has an overview, ordered hos, execution, and closure. A **dogfood
finding** (§2.2) is an emergent *record* — it documents a finding and is terminal on
creation. One is work; the other is evidence. The earlier framing that called the
*record* a "sidequest" was backwards; this registry fixes the names.

### 3.2 Extraction-hos *(canonical-provisional — adopt on 2nd use)*

**Purpose:** a named ho category whose deliverable is extracting a shared package when
the second consumer arrives — the monorepo counterpart of "don't abstract before the
second use."
**Instances:** edelmore/apps/reader (ho-01-extract-design, ho-02-extract-book,
ho-03-extract-narration → `packages/@edelmore/*`).

### 3.3 Landing hos

**Purpose:** a ho whose deliverable is landing visual/behavioral parameter values at
their **kagen** (加減 — the felt-right degree) by feel against real data — judgment work,
not feature work. Deliverable is a set of committed default values plus the commit that
locks them ("landing — lock the practitioner's by-feel pass as the field defaults"). The
method that produces them is **design tuning** (2.11).
**Shape:** typically ri (shoshin ho-06.5, ho-07.5); the category edge is fuzzy —
ho-07.6 is ha-shaped rendering work the design-process-note nonetheless lists among
landings, and the act survives without literal tuners, which is why the category is named
for the landing, not the instrument. The registry treats "landing" as a *deliverable
kind*, not a shape.
**Instances:** shoshin-no-sono (06.5, 07.5, ho-A-6.0), sutra (planned, folded into
ho-03/05), shodo (informal).

---

## 4. Other provisional types

### 4.1 Learning walkthrough *(canonical-provisional — adopt on 2nd use)*

**Purpose:** a plain-language, retrospective post-execution walkthrough of what a ho
actually built, addressed to the practitioner **as learner** — the learning layer
separated from the architectural record. Analogy in the source: "ho-01 built the engine
block … bench-test it under load."
**Frontmatter:** `type: learning-walkthrough`, `about-ho: "NN"`, `audience: practitioner`.
The `type: teaching-note` token in sharibako's instances is superseded — new instances use
`learning-walkthrough` ("learning," not "teaching": the document centers the practitioner
as learner, not the document as teacher).
**Location:** `ho-process/learning/ho-<NN>-what-happened.md`.
**Instances:** sharibako (ho-00, ho-01). One project — but it operationalizes the
framework's own claim that the deliverable and the learning are distinct outputs, and
it is the only artifact type that addresses the learning reader directly. A strong
docify candidate despite single-instance status; adopt on the second use.

### 4.2 Ho-artifacts *(light mention)*

Evidence files attached to a ho — raw query outputs, evaluations, first-use records —
that are neither hos nor dogfood findings. shodo's ho-3.2x files carry
`type: ho-artifact` with `parent:` / `relates-to:` / `note:` fields. Recorded as a
light mention only: the catch-all "evidence file" is a junk-drawer risk, not a pattern
to promote. shodo only.

---

## 5. Historical variance — do not reproduce

Recorded so the history is legible, but new projects should not copy these.

### 5.1 Phase-prefixed hos

satori-internal-affairs organizes hos as `P1-H0N` / `P2-H0N` with `-DONE-` filename
closure and no frontmatter. This predates the canonical conventions: phases already
live in the kamae-4 overview, and filename-encoded state duplicates what frontmatter
carries. Recorded as historical variance, not a pattern to reproduce.

---

## 6. Mutability regimes (cross-cutting)

Not every artifact has the same mutability. The corpus practices one axis with three
values — **living | frozen | sealed** — and naming them removes a recurring ambiguity in
forward-only's application: forward-only governs how *frozen* and *sealed* change
(supersede forward, never edit), while *living* is the declared exception (edited in place).

| Value | Applies to | Rule |
|---|---|---|
| **living** | README (kamae-3), ho overview (kamae-4), seed (kamae-1, see note), the Basis of Design (2.11) | Edited in place as the project evolves. Overview edits are forward-looking only: dead numbers stay dead, checkpoint outcomes get recorded, historical entries aren't rewritten. "Small frequent updates beat large rare ones" (sharibako kamae-4). |
| **frozen** | System design (kamae-2) | Not edited in place; changed only by a superseding addendum. Body preserved as-authored; a reader's-note pointer at the top names the addenda; the addenda carry the change (sharibako kamae-2 + 2.1 + 2.2). Thaw-able through the addendum mechanism, and still governs the build. |
| **sealed** | Closed hos, sidequests, addenda, dogfood findings, devlogs/Reflect, executed ATs, propagation-ledger commits | Final — never changes. Typographical fixes only; anything that changes what the document *said* belongs in a new document. A future document may *respond* (forward-only), but nothing supersedes it in force — it is already history. |

**Seed note (decided — merge-decisions D4).** The seed is a **living parti**, revised in
place with dated revision notes: sharibako's kamae-1 was revised when kamae-2.1 landed,
and kamae-project-framing §2.1 endorses deliberate seed revision. The framing doc's older
claim that the README is "the only Kamae document that changes" is amended accordingly.
The three regimes, stated once: **seed = living-by-deliberate-dated-revision; README /
overview = living-continuous; system design = frozen + addenda.**

---

## 7. Open decisions this registry surfaces

Practitioner-authored; the registry states the field, not the answer.

*None open.* The last item — **names** for the still-unnamed concepts — was resolved in the
2026-07-03 naming checkpoint; the vocabulary is in the [[glossary|Glossary]] (1.3). The
concepts this registry surfaced are now named: the **learning walkthrough** (§4.1), the
**landing ho** and **kagen** (§3.3), and the **mutability** axis (§6, living | frozen |
sealed) — and, in their own homes, the **Basis of Design** and **propagation ledger** (2.11),
**mind / hand** and **tripwired** (2.8), and **declared compression** (kamae-project-framing).

*Resolved earlier, in the 2026-07-02 Fable-audit merge:* closure signaling (§1.3, D2), the
standalone-AT filename (§1.4, D3), seed mutability (§6, D4), and the withdrawn "sidequest
frontmatter schema" question (§2.2, D14). The severable-capability arc is named **sidequest**
(§3.1).

---

_This document is part of the Ho System framework. It is the taxonomy companion to_
_[[kamae-project-framing]] (the sequence) and [[ho-task-decomposition]] (the ho ↔ AT_
_relationship). When a project invents a new artifact type that survives contact with_
_a second project, it earns an entry here — a sidequest (§3.1) and a dogfood finding_
_(§2.2) are distinct types, one work and one record._
