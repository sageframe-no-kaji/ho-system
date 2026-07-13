---
id: "0.2"
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

## 2026-07-11 — ho-status companion (ho-12)

### The K4 ho-status roster + required title/description frontmatter — kind: added

A new derived K4 companion — the **ho-status** roster, the counterpart to the build record's log
(kamae-project-framing 2.1 §2.4). One source (ho frontmatter `title`/`description`/`status` over
the overview's phase order) renders three ways: a `ho-status.json` feed, an in-repo `ho-status.md`
glance (one row per ho, Doc×Done from a single `state` enum), and a cross-project HTML dashboard.
The artifacts live in a repo-root **`metadata/` zone** — shared with the keisaku cache — whose
tracking is inherited from the repo's ho-work posture (gitignored when `ho-process/` is private).
`title` + `description` become **required practitioner-track ho frontmatter** (ho-structure §5.1,
the K5 authoring skill, design template 3.9); the learner-track templates keep the same pair as
H1 title + subtitle. Refresh discipline folded into cross-session-continuity 2.14 §3 (regenerate
on every state-summary write; cold record wins). Glossary headword added. Generator + dashboard
deferred to ho-13; fleet rollout to ho-14. Driver: the practitioner needs a glance-level progress
roster (hand-built one for sutra); keisaku is coordinated as a co-located cache via handoff, not
merged.

## 2026-07-10 — Kamae 2/3 glossary headwords

### Glossary gains system design (Kamae 2) and README (Kamae 3) — kind: changed

The two Kamae chain links that had no glossary headword, extracted from
kamae-project-framing 2.1 §2.2 / §2.3 in the glossary's compressed register. Driver: the
ho-actually star renders the Kamae chain as an ordered sequence (star-actually engine
ho-02), and its ingest generates nodes from glossary headwords — K2 and K3 had no entry,
so no node. The chain now resolves at all six links.

## 2026-07-10 — sage-zfs portability practice report

### Add the encoded-environment portability report (5.4) — kind: added

New Layer-5 example: `examples/sage-zfs-codex/encoded-environment-portability.md`, indexed as
**5.4**. A practice report from the first Ho System build run on a non-Claude agent — sage-zfs,
handed to Codex (gpt-5.6-sol) with a single repo-root `AGENTS.md` as the entire adaptation:
corpus pointers, skills read as procedure documents, the distilled operating rules, and the
project's hard limits. The run produced a conformant Kamae chain (scaffold, 26-ho K4, K6 with
verbatim block), a correctly executed sealed-decision banking on first contact, and a declared
compression for the pre-K4 scaffold. Includes the cross-model evaluation findings, practitioner
testimony on reading the new model, and two proposals: bless the AGENTS.md pattern as a
scaffolder-emitted template, and name the cross-vendor axis in model choice. Evidence, not
doctrine — one run, chain phase only. `examples/README.md` updated.

## 2026-07-09 — cross-session continuity doctrine (ho-10)

### Skills and templates scaffold and enforce K6 and the state-summary block (ho-11) — kind: changed

The cross-session-continuity doctrine (2.14) is now carried by scaffold and template, not by the
agent remembering to read it. The project scaffolder
(`ho-setup-project-environment-collaborator`) creates `ho-process/kamae-6-<project>-state-memory.md`
at init with a seeded state-summary block, and asks one repo-visibility-keyed posture question
(private repo → track K6; public repo → gitignore `ho-process/` and offer the nested-private-repo
option; floor → unversioned), wiring `.gitignore` accordingly. The ho-overview collaborator
(`ho-kamae-4-overview-collaborator`) now ends every overview with an empty, append-only
`## Build record` section (kamae-project-framing §2.4). The per-ho authoring collaborator
(`ho-kamae-5-authoring-collaborator`) makes each per-ho document state its close discipline —
fill Reflect/Results, flip `status: complete`, write the state-summary block to K6, append the
build-record entry to K4. The three ho templates (`ha-`, `ri-`, `shu-ho-template.md`) each gain a
closing state-summary section showing the block once (verbatim labels, fixed order): with Reflect
in ha, with Results in ri, as the final prescribed step in shu. Driven by **ho-11**, off the 2.14
doctrine.

### Add the autonomous build kickoff template (3.10) — kind: added

New Layer-3 template: `framework/templates/autonomous-kickoff-template.md`, indexed as **3.10**,
`status: draft`. The kickoff prompt for the autonomous / human-as-designer mode — the agent
authors the whole Kamae chain in the practitioner's voice and executes it, with the 2.14
continuity wiring (full-body K6, freshness, hot/cold verification, heartbeat, sealed-decisions
banking) encoded from the first message, plus the ratification gate and tripwires the pālana run
(5.3) proved necessary. Draft honestly: implied by doctrine, validated by no build yet — it
graduates on the first project that runs from it.

### Extend the Kamae chain to six links — State Memory (Kamae 6) — kind: changed

The Kamae chain gains a sixth link: the **State Memory** (`kamae-project-framing.md` §2.7), a
per-project *living* cross-session memory at a fixed path
(`ho-process/kamae-6-<project>-state-memory.md`), always present, read first by every returning
session to get back into stance. All six links are now framed on the **same footing** in two
roles — K1–K4 **preparation** (the up-front framing ladder), K5–K6 **action-time** (K5 the
pre-action document per session, K6 the record of action) — with commitment disambiguated by
scale: the project-scale ladder is K1–K4, K5 commits at session scale, K6 records. Kamae's own
definition widens accordingly: getting *and staying* in stance. The chain intro, §1 definition,
§2 header, §2.6 filename table/pattern, §3 count and closing note, and the `Kamae` glossary
entry update accordingly; the artifact-type registry (2.9 §6) registers `type: state-memory`,
names the **hot** posture (living *and* non-canonical), and sharpens `sealed` to
gravity-of-reopening. Grounded in the pālana pilot (5.3); resolved from the practitioner
rulings recorded in ho-10 (D10-8, refined by the pre-commit cross-model review in D10-9) — the
state-summary's home *is* the rule, so it gets a fixed, first-class location rather than a
negotiable one.

### Add the cross-session continuity doctrine (2.14) — kind: added

New Layer-2 structure doc: `framework/structure/cross-session-continuity.md`, indexed as
**2.14**. Specifies the **explicit continuity** — the State Memory (K6), against the *implicit*
continuity the whole cold record already carries: the **universal state-summary block**
(COMPLETED / NEXT / ACTION ITEMS or BLOCKS / PROJECT LIFECYCLE, at every ho close and session
end — a fixed parseable hook surface) pinned at the top, and the **working-memory body**
beneath it, grown by **event-gated accretion** (sections switch on as the build first needs
them; only the per-ho log + voice + heartbeat tier is mode-gated, for unattended builds — the
mode the pālana pilot proved). Disciplines that keep the mutable file honest: freshness (update
at every real pause), hot/cold authority (K6 is non-canonical and holds no original authority —
sealed decisions bank cold at the moment of sealing, the K6 copy a cache; the cold record —
git, Reflect, build record — wins), and graduated-and-preserving compaction (never delete a
lesson; compact at phase close and on a ~25–30 KB session-start tripwire). The file is
**private by default**, raw-register, published only by closeout election; versioning follows
the repo (tracked in a private repo; a nested private `ho-process/` repo under a public one —
recommended; unversioned as the floor). Lands alongside: a session-end state-summary rule in
the operating discipline (8.1, both the repo copy and the installed `~/.claude` copy); the K4
**build-record convention** (kamae-project-framing §2.4); and seven glossary terms (1.3).
Executes **IDEA-011** via **ho-10**; reviewed pre-commit by a cross-model pass (D10-9).

## 2026-07-09 — pālana continuity practice report

### Add the pālana pilot continuity discipline (5.3) — kind: added

New Layer-5 example: `examples/palana-autonomous/continuity-discipline.md`, indexed as
**5.3**. A practice report from the first *autonomous* Ho build — how it maintained
cross-session continuity with no human carrying the thread, as a repeatable four-layer
process (working-memory handoff file, public build-record log on K4, per-ho Reflect, an
ntfy heartbeat). Documents where the pilot conforms to, extends, and invents beyond the
framework's document-as-memory model, and proposes four additions for autonomous /
long-running builds (a named working-memory artifact, a K4 build-record convention, an
alerting heartbeat, the hot/cold finding lifecycle). Evidence, not doctrine — nothing
here supersedes a structure document. `examples/README.md` updated.

## 2026-07-03 — INDEX layers and hygiene (ho-09)

### Reorganize the INDEX layer model — kind: changed

Executes the layers half of IDEA-007 (the D18 remainder ho-02 deferred). Layer **7
(Project Artifacts)** is retired — empty since ho-05's archival pass; the number stays
dead, no new 7.x IDs (forward-only). Layer **0** becomes the explicit repo-meta layer
(0.1 INDEX, 0.2 CHANGELOG, 0.3 CONTRIBUTING). Two layers added: **8 Practitioner**
(operating discipline 8.1, rationale 8.2, environment architecture 8.3, module structure
8.4) and **9 Skills** (the catalog `skills/ho-skill-overview.md` as 9.1; individual
skills are cataloged there, not indexed row-by-row). The three committed 2026-07 audit
briefs are indexed as historical rows 6.3–6.5. Frontmatter `id` blocks added to all
newly indexed documents (and the missing `id: "6.2"`); `fable-audit-prior-findings.md`
normalized from the off-taxonomy `agent-task-context` to `agent-task`. The type taxonomy
gains `practitioner` (→ 8) and `skill` (→ 9). The larger IA question — whether the layer
model should become a navigation experience — stays deferred (IDEA-007 carries it).

### Retire the schema-reference ID cache — kind: removed

`.github/skills/ho-tool-index-maintenance/references/schema-reference.md` drifted
comprehensively within weeks of writing (missing 1.3, 2.9–2.13, 3.9; still listed the
archived 7.2/7.3; next-available table wrong in four layers). Deleted; INDEX.md is the
sole ID authority and the skill derives current/next IDs from it directly. Both SKILL.md
copies (`skills/` and `.github/skills/`) updated to the new layer model and taxonomy.

## 2026-07-03 — verification tool-framing (ho-08)

### Reframe 2.7 linting as a local tool choice — kind: changed

`framework/structure/verification-practices.md` (2.7) Layer 1b canonized `black / isort /
flake8 / mypy` as *the* Python lint pipeline, while the operating discipline (and current
practice) run **ruff + mypy** and frame the linter as a local choice. Adopted the operating
discipline's own resolution: the linter is **`ruff`, or `flake8 + black + isort`** — either
works, chosen per project, not prescribed by the framework; `mypy` runs alongside whichever.
Updated the Layer 1b definition, both workflow diagrams, the shu-habit line, and the devlog
example to the leaner `ruff + mypy` default (the historical Kanyō evidence and the sample
devlog entry in 2.6 stay as-is). Closes the tool-framing half of stale-ref [ho] item 5
(IDEA-010); ho-07 had already settled the layer count.

## 2026-07-03 — idea-log convention (ho-06)

### Add 2.13 The Idea Log — kind: added

New structure doc `framework/structure/idea-log.md` (2.13): canonizes the idea-log convention
the framework repo already dogfoods (merge-decisions **D16**) — a findable, forward-only
per-project backlog (`ho-process/ideas.md`) with stable `IDEA-NNN` ids, four dispositions
(independent → linked → resolved / dropped, never deleted), and two rituals (capture-on-surface,
review-at-each-ho-boundary). `kamae-project-framing` §2.6 gains the project-specific ho-process
layers (`notes/`, `learning/`, `ideas.md`), which closes coherence finding **D-7** (the glossary's
§2.6 citations now resolve to a section that names them). INDEX gains the 2.13 row; the idea-log
review hook is added to the kamae-4 and kamae-5 collaborator skills (skills/ layer).

## 2026-07-03 — mechanical stale-ref cleanup (ho-05)

Pointer- and status-level hygiene under the doctrine ho-02/03/3.5/04/07 landed. No
canonical document changes meaning here — only stale references, statuses, and metadata.

### Execute the repo-wide `version:` sweep — kind: changed

Stripped the `version: "1.0"` frontmatter field from the 14 remaining framework
documents D13's per-doc-version drop had not yet reached — the sweep the ho-02 D13 entry
deferred to this ho (IDEA-006). `CHANGELOG.md` is now the sole version story, with no
per-document `version:` field left anywhere in `framework/`. The
`ho-tool-index-maintenance` skill (both the `.github/skills/` and `skills/` copies) drops
`version:` from its maintained-fields guidance and frontmatter-schema block, so a future
index pass does not re-add it. `framework/templates/Seed Template Checklist.md` keeps its
field pending archival later in this ho.

### Reconcile INDEX layers 6 and 7 — kind: changed

`INDEX.md`: added `agent-task-2026-05-25-integrate-ho-task-decomposition.md` as **6.2** —
it was written but never indexed. Removed **7.2** (Shodō Seed) and **7.3** (Seed Template
Checklist); both were archived to `practitioner/archive/` in this ho, so they leave the map
as the other archived documents already have. **7.1** (project-checklist.md) is left in
place but points at a gitignored file — a pre-existing public-INDEX → private-doc leak,
flagged for the practitioner. New `skills/` and `practitioner/` layers, CONTRIBUTING/
CHANGELOG placement, the near-empty layer 7, and the unindexed audit-brief working docs all
stay deferred to the INDEX taxonomy pass (IDEA-007).

### Canonicalize the sealed-set membership; fold Fable pass-2 coherence fixes — kind: changed

Fable pass-2 **D-2**: the `sealed` mutability set had three different memberships (naming
record, registry §6, glossary). Made registry §6 the canonical enumeration and added
**sidequests** to its sealed row (naming-record #8 — a closed sidequest is closed hos plus a
spent phase-overview); the glossary `sealed` entry now mirrors it (regaining the
devlogs/Reflect it had dropped). **D-3**: a reconciling clause in the glossary
Basis-of-Design entry — the file is living via named-reason propagation, each landed value
frozen between propagations. **D-4**: "its own smoke pass" → "its own validation pass (in
shodo's case, an eval)" in registry §3.1 and the glossary sidequest entry (post-**D19**
vocabulary — smoke is the agent-run floor, but shodo ran an eval). **N5**: the merged-set
docs 2.8 / 2.9 / 2.10 / 2.11 / 2.12 / 1.3 flip `status: draft → stable` to match 2.7 and the
rest of the landed structure layer (2.1 left `draft` — flagged for the practitioner). No
doctrine changes — the doctrine was already coherent; these align its compression into the
glossary and registry.

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

### Validation-awareness in 1.2; settle the layer count framework-wide — kind: changed

`ho-foundations-evidence.md` (1.2 §1.3): the overconfidence answer now pairs the
verification stack with the **validation** layer (whether the *right thing* was built),
extending D19's overconfidence-into-goodness thesis into the foundations (finding D-6).
The "four-layer" label is settled to **five-layer** ([ho] item 5) at every site — 1.2,
1.1, `verification-practices.md` (§5 ri-workflow), and `ri-ho-template.md` — matching
2.7 §2's canonical five layers (the count half was known drift; the
validation-blindness half was new since D19).

### ho-00 identity: orientation ho, not the Ho Overview — kind: changed

`ho-structure.md` §3.3: "Zero (00): The Ho Overview … Ho 00 is the plan itself" is
superseded by the **orientation-ho** meaning (finding B2b) — ho-00 is the project's
first working session; the sequence plan lives in the Kamae 4 Ho Overview. The
conflation is also fixed at the 0.5 description and the filename example
(`ho-00-overview.md` → `ho-00-orientation.md`). Matches all current practice and the
glossary's ho-00 entry (merge-decisions D7, bidirectional).

### Mark agent-task-spec's tracking convention historical — kind: changed

`agent-task-spec.md` (3.8): the `tasks/` directory and `NNN - Agent Task` filenames in
the Tracking section predate 2.8's canonical `ho-process/agent-tasks/` + `Ho-NN-AT-MM.md`
/ `Standalone-AT-…` conventions ([ho] item 4). Added a historical-convention note; the
filesystem-as-tracker principle and the forward pointer to 2.8 stand. Also the last
"four-layer" → "five-layer" site (:264), completing the layer-count settling.

### Repoint glossary citations after the §2.5→§2.6 renumber — kind: changed

The two glossary entries citing `2.1 §2.5` for the `ho-process/` layout (`ho-process/`,
`notes/`) now cite **§2.6** — the Filenames section moved when §2.5 became "Per-Ho
Documents." Keeps the pointer aimed at the section that describes the directory layout.
(The deeper D-7 content mismatch — `ideas.md`/`learning/`/`notes/` not yet in that
section — remains ho-05/ho-06's, per the ho.)

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
