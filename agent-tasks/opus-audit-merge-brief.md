---
created: 2026-07-02
type: agent-task
audit-source: private/audit/fable-2026-07-01/ (Fable, ran 2026-07-02)
audit-brief: agent-tasks/fable-audit-brief.md
prior-findings: agent-tasks/fable-audit-prior-findings.md
merge-model: Claude Opus (practitioner present, planning-mode)
status: ready
---

# AT — Fable audit merge session (Opus, planning-mode)

## What Fable produced

Fable completed the framework audit specified in `agent-tasks/fable-audit-brief.md`
and wrote 16 files (~171 KB) to `private/audit/fable-2026-07-01/`. Nothing outside
that directory was touched — `private/` is untracked, framework/practitioner/skills
are untouched. No premise-level surprise fired the escalation clause.

**Top-level** (5 files):
- `README.md` — executive summary + deliverable table
- `discovery-corpus-map.md` — corrected corpus (15 Ho projects; three additions the
  prior audit missed: `constructive-interference/kiku`, `personal-projects/resume-
  engine`, `sageframe-irori/sageframe-config`; six corrections)
- `disagreements-with-prior-audit.md` — **six substantive disagreements + five
  judgment calls made under unattended-run discipline**
- `deliverable-2-web-presence-vision.md` — Kamae-2-resolution site vision; three
  creative commitments; declines two brief candidates with reasoning
- `deliverable-3-kinhin-mcp-seed.md` — real Kamae 1 seed, forward-only supersession
  of the February seed, 13-tool surface

**deliverable-1-docification/** (11 files):
- `framework-structure-artifact-type-registry.md` — draft: full taxonomy (core,
  emergent-record, structural-extension types; three mutability regimes; five open
  decisions)
- `framework-structure-ho-task-decomposition-revised.md` — **revision of the
  existing framework doc**, elevating model field, escalation clause,
  one-commit-per-AT, and two-tier principle from the skill layer
- `framework-structure-kamae-addenda.md` — draft: kamae-N.M doctrine from the
  sharibako 2.1/2.2 specimens
- `framework-structure-design-work.md` — draft doctrine + design-ho template stub;
  recommends AGAINST `shape: design` and not-yet for a design skill
- `framework-structure-external-contribution.md` — draft: supacode-grounded
  doctrine including the sidecar-directory answer to ho-process privacy
- `other-invented-patterns.md` — verdicts on 12 patterns (compressed chains,
  monorepo, extraction-hos drafted inline; closure convention decided per the
  brief)
- `framework-glossary.md` — ~40 entries with defining sources
- `framework-changelog-proposal.md` — convention + seeded from git history
- `stale-reference-cleanup.md` — 22 items ranked by blast radius
- `framework-debt.md` — 11 items + a three-ho execution sequence
- `concepts-without-names.md` — 12 concepts described, none named — awaiting the
  practitioner

## What overturned the prior audit

Fable surfaced six substantive corrections. **Walk these with the practitioner
first** — several change the shape of downstream merge work:

1. **The Ho ↔ AT split IS already codified.** Framework 2.8 exists and is good.
   BUT its frontmatter spec (`parent:` path) contradicts what every project and
   the dandori FORMAT use. The four operational AT properties (procedural, model
   choice, escalation, one-commit-per-AT) live only in the skill layer. The revised
   doc Fable wrote is a **revision to a specific existing doc**, not new authoring.
2. **AT directory naming is already decided** — `ho-process/agent-tasks/`, per
   kamae-project-framing §2.5. shodo and kiku are drift. The open question is the
   standalone-AT filename, specified three contradictory ways.
3. **pink-reader is not a "rationale-only project"** — has a full seed + system
   design. That pattern loses its only instance in the prior audit's inventory.
4. **Closure signaling — four-plus conventions, not three**, drifting within
   shodo itself.
5. **sageframe-mcp ho statuses were inverted** in the prior audit;
   **sharibako has 5 hos / 9 ATs, not 4 / 7**, plus three unrecorded patterns
   (teaching notes, private-docs, retained pre-seed).
6. **Overview mutability isn't an unanswered gap** — sharibako's kamae-4 states
   the living-document doctrine explicitly. The framework hasn't absorbed it yet.

## The most consequential finding outside the brief's scope

The env-setup skill's bundled operating discipline is a **stale snapshot missing
forward-only entirely**. A practitioner onboarded by it today receives a discipline
without the framework's newest principle. This is a correctness bug, not a
documentation gap. Stale-ref item 1; debt item 3. Prioritize.

## Your job — this session

You are the merge session. The practitioner is here. This is planning-mode work,
not execute-everything.

### Read in this order

1. `private/audit/fable-2026-07-01/README.md`
2. `private/audit/fable-2026-07-01/disagreements-with-prior-audit.md` — six
   disagreements + five judgment calls. This is where the practitioner's input is
   most immediately needed.
3. `private/audit/fable-2026-07-01/discovery-corpus-map.md`
4. Each deliverable in the order Fable structured them
5. `agent-tasks/fable-audit-brief.md` and `agent-tasks/fable-audit-prior-findings.md`
   for context on what was asked

### First-pass output

Do not propose merges yet. Produce:

1. **A one-page executive summary** of what Fable actually delivered — file by
   file, one-line each, honest about draft quality where you can judge it.
2. **The disagreements walkthrough as a decision list** — six overturns, five
   judgment calls, plus the 12 unnamed concepts. For each: what Fable found, what
   decision the practitioner needs to make, your recommendation with reasoning.
3. **A leverage-ordered merge plan** — the top three findings to act on first,
   with justification. Highlight the env-setup stale-discipline issue as a
   correctness bug that deserves urgency.
4. **Wait for the practitioner to react** before going deeper.

### Then, after the practitioner reacts

Expected flow:

1. Walk each finding one at a time — accept-as-is / modify / reject / defer, with
   your recommendation.
2. Group accepted findings by merge shape:
   - **Move-into-framework** — the four draft structure docs (artifact-type-registry,
     kamae-addenda, design-work, external-contribution), glossary, changelog.
   - **Replace-in-framework** — ho-task-decomposition-revised replaces the current
     `framework/structure/ho-task-decomposition.md`. Careful — it's a revision
     of a live doc, not new authoring.
   - **Execute-as-work** — the 11 debt items + 22 stale-ref items. Some are
     correctness bugs (env-setup stale discipline), some are hygiene.
   - **Decide-and-name** — the 12 unnamed concepts. Each gets a name from the
     practitioner. Do not propose names.
   - **Consume-and-plan** — deliverable-2 (site vision) and deliverable-3 (Kinhin
     seed) inform future work but aren't merges into `framework/`.
3. Group accepted findings into one or more hos following
   `framework/structure/ho-structure.md`. Fable proposed a three-ho sequence
   inside `framework-debt.md` — consider it but do not defer to it uncritically.
4. Practitioner opens the ho(s). Actual framework merging happens in those ho
   sessions, not this one.

## Constraints

1. **Do not modify anything under `framework/`, `practitioner/`, or `skills/`
   this session.** `private/audit/fable-2026-07-01/` is drafts;
   `framework/` etc. is canonical. Merging is real ho-work.
2. **Do not name unnamed concepts.** Those 12 concepts in `concepts-without-names.md`
   are the practitioner's naming work. Surface them, describe them precisely, wait.
3. **Ambiguities in Fable's output get surfaced, not silently resolved.**
4. **Fable's judgment calls in `disagreements-with-prior-audit.md` — walk them
   explicitly.** The practitioner decides whether each stands.
5. **Follow the operating discipline.** Planning mode. Bounded scope. Forward-only —
   merging Fable's drafts is real framework change; it goes through hos, not through
   this session's edits.
6. **Answer questions before continuing.** The practitioner is here, not absent.
   A mid-execution question is a decision input; stop, answer, wait.

## Voice

Systems language. Direct. If Fable got something wrong, say so with the evidence.
If Fable produced a draft that shouldn't merge as-is, name the specific issue.
The practitioner wants pushback on Fable's drafts, not deference.

## Begin

Read the files. Then produce the first-pass output — executive summary,
decision list, leverage-ordered merge plan, top three to act on first. Wait for
the practitioner.
