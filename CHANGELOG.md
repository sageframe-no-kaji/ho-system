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

## 2026-07-03 — canonical-claims reconciliation (ho-07)

Reconciles the foundation/structure layer with settled doctrine ho-02 missed
(Fable pass 2, findings B2a/B2b/D-1/D-6). Executes decided doctrine — it makes no
new architectural call.

### Merge the UPDATES catalog into 2.1; adopt the five-document chain — kind: changed

Merged the four CONFIRMED updates from `kamae-project-framing.UPDATES.md` (Kamae 5 /
five-document chain; practitioner-scope acknowledgment; System Design ↔ Ho Overview
"not beholden"; phase-first Ho Overview with release tags) into 2.1, then removed the
catalog (labeled "discard after merge"). New §2.5 "Per-Ho Documents (Kamae 5)";
Filenames renumbered §2.6. The three SUGGESTED updates were dropped per the catalog's
own legend. Root fix for B2a — the glossary's five-document citation now resolves to a
source that agrees.

### 2.1 four→five by sense; complete the D4 mutability amendment — kind: changed

Resolved every "four documents" occurrence by sense rather than a blanket rename: the
**framing phase** still produces four *framing* documents (§2 header, §§1/2.6/4/6), while
**the chain** is the five-link chain (§1 diagram K-labels, §3 count + diagram node
`INDIVIDUAL HOS` → `PER-HO DOCUMENTS`, the K1–K5 walkthrough). Also completed merge-decisions
**D4**: the residual "the README evolves; the other Kamae documents don't" claim (finding
D-1) now states the living/frozen model (only the System Design is frozen).

### Propagate the five-document chain + validation-awareness into 1.1 — kind: changed

`the-ho-system.md` (1.1): the Kamae summary (§ The Arc) now names the five-document
chain and drops the pre-seed term "concept" (finding B2a); the Verification Practices
doc-map row names the **five-layer** stack (settling the four-layer/five-item drift,
[ho] item 5) and adds the validation layer — whether the *right thing* was built
(finding D-6, propagating D19 up from 2.7).

## 2026-07-03 — post-audit correction (fork-A completion)

### Complete the fork-A term swap in design-work + template — kind: changed

Finishes the register → Basis-of-Design swap that ho-04 declared but left incomplete (Fable
pass 2, finding **B1**). Seven residual design-sense "register" uses in
`framework/structure/design-work.md` and `framework/templates/design-ho-template.md` — including
the template's `## Register propagations` heading and its closing `living-register` (the exact
pre-rename term a new project would copy) — replaced with Basis of Design / propagation. Also
drops the residual "tuner landings" category labels → "landings" (naming record #3). Completes
ho-04's declared intent; the decision (fork A, **D20**) was already made. Also sweeps the
standalone word "modality" (finding D-5, practitioner ruled) → "method" / "design tuning" in 2.11
and the INDEX row — the framework reserves "modality" for the four *validation* modalities, per
the naming record's own reasoning for rejecting "the tuning modality."

## 2026-07-03 — Fable-audit merge (ho-04, naming + glossary)

### Name the twelve unnamed concepts across their home docs — kind: renamed

The naming checkpoint (2026-07-03) named the twelve concepts from `concepts-without-names`;
the names land in their home framework docs first (Path A — land, then index). **2.8
ho-task-decomposition:** §1.1 names the two-tier principle **mind / hand** (epigraph *mens et
manus*); §3.3 names the AT-escalation property **tripwired** (escalation stays the mechanism).
**2.11 design-work:** the modality is **design tuning**; the living register → **Basis of
Design** + **propagation ledger**; tuner-landing → **landing ho** + **kagen** (加減). **2.9
artifact-type-registry:** §3.3 Tuner-landing → **Landing hos**; §4.1 Teaching notes →
**Learning walkthrough** (`type: teaching-note` superseded by `learning-walkthrough`); §6
mutability reframed as the **living | frozen | sealed** axis ("permanent record" → sealed);
§7 open-names item closed. **2.1 kamae-project-framing:** §4 adds **declared compression** /
**compressed chain** (the ri-stage move of combining chain layers, legitimate only when
declared). The "register" collision is ruled fork A: register stays the cognitive altitude
(architectural / executable); the design file drops the word. Naming record:
`private/audit/naming-record.md`; timeline pointer merge-decisions **D20**.

### Add the Glossary (1.3) — kind: added

New foundation doc `framework/glossary.md` (1.3): the framework's working vocabulary, each
term with its defining document (merge-decisions **D12**). Indexes the twelve just-named
concepts, the four validation modalities (smoke / interaction / eval / dogfood, from 2.7 §3),
and the ~40 terms already in use. Replaces the stale draft entries (register(visual) → Basis
of Design; side track/subproject + old "sidequest = record" → sidequest; teaching note →
learning walkthrough). INDEX gains the 1.3 Foundation row. Lands last of the framework docs,
per D12.

## 2026-07-02 — Fable-audit merge (ho-3.5, the validation layer)

### Add the Validation layer to 2.7 — kind: added

`framework/structure/verification-practices.md`: new §3 "The Validation Layer" — the "is it
*good*?" half of quality, paired with the existing verification stack ("is the code
*right*?") (merge-decisions **D19**). Four modalities carved by *who runs it* (agent vs
human) × *what is judged* (function → feel → output-quality → real-use): **smoke test**
(agent, function), **interaction test** (human, function + feel), **eval** (human, output
quality), **dogfood** (human, real use). Carries the agent-floor/human-verdict sub-axis, the
governing principle ("a passing smoke test is not validation — it's the floor a human
interaction test has to clear"), the no-green-bar completion signal (a ho is not `complete`
on verification alone), and the sharibako ho-04.2 worked example (agent smoke + 346 green
tests passed; a human interaction test found 2 errors + unusable UI in a failure class smoke
structurally can't reach). Adds a sixth principle ("verification is not validation");
existing §3–§7 renumber to §4–§8. Extends 2.7's founding overestimation thesis from
correctness into goodness. Supersedes the smoke/dogfood artifact framing of **D14/D15**
(bidirectional per **D7**); D14's core (sidequest = severable build arc) stands. Strips the
stale `version: "1.0"` field (D13 / IDEA-006).

### Rebuild registry §2 to eval + dogfood — kind: changed

`framework/structure/artifact-type-registry.md`: §2 retitled "Validation artifacts" and
rebuilt to the two validation modalities that produce documents (merge-decisions **D19**) —
**eval** (§2.1, a graded-eval record) and the **dogfood finding** (§2.2). Renames the old
§2.1 "smoke test" → **eval** (the entry mislabeled the human-graded output-quality pass; the
true agent-run smoke test is a *practice*, now homed in 2.7 §3 with no registry entry — as is
the interaction test). shodo's `smoke-handoff` / `smoke-results` re-home as `eval-handoff` /
`eval-results` (the `smoke-` token dropped). Drops the "one discipline, two artifacts"
framing; adds the altitude note (the validation *layer* is canon per D19; the two artifact
*types* stay canonical-provisional, single-project). Cross-referenced to 2.7 §3; §3–§7
untouched. Supersedes the D14/D15 artifact structure (bidirectional per **D7**).

## 2026-07-02 — Fable-audit merge (ho-03, standalone drafts move-in)

### Add 2.12 External-Project Contribution — kind: added

New structure doc `framework/structure/external-contribution.md` (2.12): running Ho
against an upstream codebase — keep the session discipline, surrender project authority
(merge-decisions **D11**). The Kamae chain compresses (the issue substitutes for it);
the session discipline survives intact; the verification stack, commit style, and
forward-only are renegotiated to the host. Adopts the **sidecar-directory** doctrine —
Ho artifacts never enter the contribution repo's object database — flagged "one case
study (supacode)."

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
