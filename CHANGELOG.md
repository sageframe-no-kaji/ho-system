---
title: "Changelog"
type: foundation
status: living
tags: [ho-system, changelog, versioning]
---

# Changelog

The framework's version story: all notable **structural** changes to the Ho System —
new documents, full replacements, doctrine changes, renames, and removals. Ordered
newest-first. Prose polish, typo fixes, and broken-link repairs are out of scope.

Each entry names what changed, carries a `kind:` tag, and cites the driving decision
where one exists (`merge-decisions D1`, a ho, etc.).

`kind:` — `added` · `replaced` · `changed` · `renamed` · `removed` · `deprecated`

---

## 2026-07-02 — Fable-audit merge (ho-03, standalone drafts move-in)

### Add design-ho template — kind: added

New template `framework/templates/design-ho-template.md` (INDEX 3.9): the in-chain
wrapper for the visual-design modality — tuner landings (`shape: ri`) and design-session
batches that want a ho document (`shape: ha`). Companion to 2.11; the skeleton is
complete, section prose fills in per project (merge-decisions **D10**).

### Add 2.11 Design Work — kind: added

New structure doc `framework/structure/design-work.md` (2.11): the visual-design
modality — design that legitimately happens partly outside the Ho chain and propagates
back through frozen artifacts and numbered hos (merge-decisions **D10**). Records the
eight-step method, the living-register artifact, and the chain-attachment points. No
`shape: design` (design decomposes into existing shapes). §6 states the
`ho-kamae-design-collaborator` skill as an actively-tracked roadmap item — the doctrine
doc plus the practitioner-scope `~/.claude/modules/design-work.md` module are the interim
reference until it is built (the module ships with this ho, outside the framework repo).

### Add 2.10 Kamae Addenda — kind: added

New structure doc `framework/structure/kamae-addenda.md` (2.10): the forward-only
principle applied to the Kamae chain — a mid-build architectural decision (kamae-N.M)
supersedes a *named part* of a frozen Kamae document while the original stays frozen as
the record (merge-decisions **D9**). §3 (the three mandatory supersession links) is tied
to the framework-wide bidirectional-supersession rule in ho-structure §3.5
(merge-decisions **D7**). Moved in from the Fable-audit draft; the registry's §1.2
forward pointer drops "forthcoming."

## 2026-07-02 — Fable-audit merge (ho-02, canonical-layer reconciliation)

### Index the 2.9 registry — kind: changed

`INDEX.md`: add the 2.9 Artifact Type Registry row to the Structure layer so this ho's new
document is on the map. Partial execution of **D18** — the new `skills/` and `practitioner/`
INDEX layers and the larger "is the seven-layer model still right" information-architecture
question are deferred to a dedicated INDEX design pass (tracked in the ho-process idea log).

### Name the three Kamae mutability regimes — kind: changed

`framework/structure/kamae-project-framing.md`: amend the claim that the README is "the
only Kamae document that changes" (merge-decisions **D4**). Name three regimes — seed =
living-by-deliberate-dated-revision, README/overview = living-continuous, system design =
frozen + addenda — matching practice (the seed is a living parti) and the registry's §6
mutability table.

### Define ho closure vocabulary; generalize bidirectional supersession — kind: changed

`framework/structure/ho-structure.md`: new §5.4 "Closure signal" defines the two terminal
`status:` states — `complete` and `superseded` — and when a ho flips to each (merge-decisions
**D2**); §5.1's field table points to it. The prior `closed` / `done` / `-DONE-` conventions
are superseded. §3.5 (Forward-Only) gains the framework-wide rule that supersession links are
bidirectional (**D7**), generalizing what previously lived only in the registry's sidequest
section.

### Add 2.9 Artifact Type Registry — kind: added

New structure doc `framework/structure/artifact-type-registry.md` (2.9): the taxonomy of
every artifact type the practice produces, companion to the Kamae *sequence*
(merge-decisions **D8**). Folds in the closure vocabulary (D2), standalone-AT filename
(D3), and seed-mutability regimes (D4). Carries the **D14/D15 correction**: "sidequest" is
a severable build *arc* (§3.1, renamed from "side track"), not an emergent record; the
old "sidequest = record" type is deleted and §2 rebuilt as a provisional smoke/dogfood
block (smoke test + dogfood finding). D5 provisional tiers applied as per-entry labels;
D7 bidirectional supersession made a requirement (§3.1).

### Replace 2.8 Ho-Task Decomposition — kind: replaced

`framework/structure/ho-task-decomposition.md` fully replaced (merge-decisions **D1**).
Preserves the v1 spine; elevates the skill-layer doctrine into the framework: §1.1 the
two-tier separation of thinking from coding, §3 the four operational properties
(procedural / model field / escalation / one-commit), §7 cardinality, §8 the dandori
bridge, three worked examples. Reconciles child frontmatter to the practiced form
(`parent-ho` + `task`, superseding the unused `parent:` path). §4.2 states the D3
standalone-AT filename decision (`Standalone-AT-YYYY-MM-DD-slug.md`) rather than an open
question. Couples with 2.9 (mutually referential — landed together).

### Adopt CHANGELOG as the version story; drop per-doc `version:` fields — kind: changed

This file becomes the framework's single version story (merge-decisions **D13**).
Per-document `version:` frontmatter fields are removed as half-maintained and redundant
with this changelog. Documents touched from this point drop the field; the repo-wide
sweep of untouched documents is deferred to the mechanical-cleanup ho (tracked as
IDEA-006). No `1.0 → 2.0`-style version bumps are groomed going forward — a structural
change is recorded here instead.
