# Kamae Project Framing — Updates

A catalog of proposed updates to `framework/structure/kamae-project-framing.md`, organized by section. Each update names the current state, the rationale, proposed wording, and a status (CONFIRMED, FOR DECISION, or SUGGESTED).

The author of the canonical document merges these in. This catalog is a working artifact — discard after merge.

---

## Status Legend

- **CONFIRMED** — Andrew explicitly approved during the May 2026 framework conversation.
- **FOR DECISION** — Structural choice needing Andrew's call before merging.
- **SUGGESTED** — Recommendation surfaced during the conversation; low-stakes; merge or discard at will.

---

## Update 1 — Add Kamae 5: Per-Ho Documents

**Status:** CONFIRMED

**Where:** Section 2 (The Four Documents), Section 3 (The Chain), Section 8 (Related Framework Documents). Renames throughout from "four documents" to "five documents."

**Current state:** The canonical document treats individual hos as the *output* of Kamae 4 rather than as a Kamae document themselves. The chain ends at "Ho Overview" with individual hos as a downstream consequence.

**Rationale:** Per-ho documents are written at session start using the same kamae logic — getting in stance before doing the work. Each is a framing document for one bounded session. They belong in the chain, not after it. Naming them Kamae 5 also matches the skill architecture: four planning skills (seed, system-design, readme, ho-overview) plus one per-ho authoring skill (deferred).

**Proposed wording for Section 2.5 (new subsection):**

```markdown
### 2.5 Per-Ho Documents

**What it is:** The bounded scope for a single working session, written at the start of executing each ho. Per-ho documents take a position from the Ho Overview and turn it into something the practitioner and the agent can work against in one session. Each ho gets its own document.

**What it's NOT:** A re-statement of what's in the Ho Overview. A vague description of "the next thing to do." A document written after the work is done. The per-ho document is the *plan* for the session, written *before* the work begins.

**What "done" looks like:**

A per-ho document should define:

- The ho's heading and number (matching the Ho Overview)
- A 3–5 sentence narrative of what the ho accomplishes
- Explicit dependencies (what must be in place before this ho can start)
- What's in scope, in concrete enough detail that the agent can execute against it
- What "done" means — concrete acceptance criteria, including test coverage, lint pass, and verification gates
- What's out of scope (boundaries the agent should refuse to cross)
- Decisions that this ho resolves (with the criteria for resolving them)
- The verification stack the ho commits to (lint, test, type-check, optional cross-model)

**What "done" does NOT look like:**

- Implementation specifics that the agent should determine ("use this exact algorithm")
- Architectural decisions that should have been made upstream ("decide whether to use Postgres or SQLite")
- A walkthrough of every file the agent will touch (over-specifies)

**Relationship to what comes next:** The per-ho document is the document the agent reads at session start. It is the most direct manifestation of the encoded environment — it tells the agent exactly what success looks like for this session.

**Example from Kanyō:** Each ho document in the Kanyō pilot served this function, though they varied in how strictly they encoded acceptance criteria. The framework formalizes the role they were already playing.

**Example from Hōzō:** Hōzō's Three Hours session compressed Kamae 5 into the same document as the implementation specification — the practitioner's spec doubled as the per-ho document. This is a Ri-stage move; new practitioners should keep them separate.
```

**Proposed update to Section 3 (The Chain) diagram:**

The current chain diagram shows:

```
Concept → System Design → README → Ho Overview → Individual Hos
```

Should become:

```
Seed → System Design → README → Ho Overview → Per-Ho Documents
(K1)    (K2)            (K3)     (K4)          (K5)
```

With the chain diagram updated to reflect five documents and the labels Kamae 1 through Kamae 5.

---

## Update 2 — Naming: Concept → Seed

**Status:** FOR DECISION

**Where:** Throughout the document. Section 2.1 title, Section 3 chain diagram, Section 4 references, Section 7 (Kanyō Evidence).

**Current state:** The document calls Kamae 1 "Concept." The skill that produces it is `ho-seed-collaborator`. The two names refer to the same artifact.

**Rationale:** Naming consistency. Two options:

1. **Rename in the framing doc to "Seed"** to match the skills. Argues "seed" is more concrete (organic metaphor, suggests something that grows), better metaphor (matches the rest of the Japanese-themed naming), and the skill name should drive since the skill is the operational artifact.

2. **Rename the skill to `ho-concept-collaborator`** to match the framing doc. Argues the framing document is canonical and the skills should derive their names from it.

3. **Keep both names and acknowledge them as synonyms.** Argues both names have value and switching costs are low.

**Recommendation:** Option 1 — rename in the framing doc to "Seed." The seed metaphor does real work in the methodology (something planted that grows into a project), and the skills are the operational artifacts that practitioners actually invoke.

**Proposed wording for Section 2.1 title and opening:**

```markdown
### 2.1 Seed

**What it is:** The raw idea. Brainstorming output. What's exciting, what
problem it solves, who it's for, why you care. Written for yourself.
[...rest unchanged, with "concept" → "seed" throughout the section...]
```

---

## Update 3 — Practitioner Scope Acknowledgment

**Status:** CONFIRMED (in principle; specific wording open)

**Where:** Section 1 (What Kamae Is) — add a brief section. Section 8 (Related Framework Documents) — add a link.

**Current state:** The document is implicitly project-scope. It treats Kamae as the framing for a specific project. Practitioner-scope concerns (operating discipline, environment setup, practitioner profile) are not acknowledged.

**Rationale:** The methodology has two scopes. Project scope = what gets built (the Kamae chain, per project). Practitioner scope = how the practitioner works (operating discipline, environment configuration, persistent across projects). The framing document needs to acknowledge that practitioner scope exists alongside the Kamae chain rather than inside it.

**Proposed wording for end of Section 1:**

```markdown
Kamae sits at project scope. It frames *the specific thing being built*. This is distinct from practitioner scope — *how the practitioner works*, regardless of project. Practitioner-scope concerns include the operating discipline (how testing, linting, permissions, and verification are handled), the practitioner's environment configuration (IDE settings, agent instructions like CLAUDE.md), and the practitioner's profile (skill level, language preferences, tool stack).

Practitioner scope is established once per practitioner-tool combination and travels across projects. Kamae is done once per project. The two scopes meet at ho-00, where the project's specific instantiation of the operating discipline gets encoded in the repo.

For the practitioner-scope canonical document, see [[the-operating-discipline]] (`practitioner/the-operating-discipline.md`).
```

**Proposed addition to Section 8:**

```markdown
- [[the-operating-discipline|The Operating Discipline]] (practitioner/the-operating-discipline.md) — The practitioner-scope canonical document
```

---

## Update 4 — System Design ↔ Ho Overview Relationship

**Status:** CONFIRMED

**Where:** Section 2.2 (System Design) and Section 2.4 (Ho Overview). Add a note in each section about the relationship.

**Current state:** The document doesn't address what happens when the system design includes a provisional ho sequence and a deferred decisions table (which it usually does). The current chain diagram suggests a one-way refinement: system design → README → ho overview. In practice, the ho overview welcomes the system design's provisional content as starting material but is not bound to it.

**Rationale:** The system design's job is to commit to architecture. The ho overview's job is to sequence the build. They serve different purposes and may evolve at different speeds. The system design's provisional ho sequence and deferred decisions are useful starting material; they should not be treated as fixed inputs.

**Proposed addition to Section 2.2 (System Design) — add to "What 'done' looks like" section:**

```markdown
The system design may include a provisional ho sequence and a deferred decisions table at the end. These are starting material for Kamae 4 (Ho Overview). They reflect the architecture's natural build order and the empirical decisions tied to specific architectural choices. The Ho Overview welcomes them but is not beholden to them — sequence and groupings will evolve as the build is planned and executed.
```

**Proposed addition to Section 2.4 (Ho Overview) — add as a paragraph after the existing "What 'done' looks like":**

```markdown
The Ho Overview welcomes the System Design's first-pass ho sequence and deferred decisions table as starting material. It is not beholden to them. The two documents serve different purposes and evolve at different speeds; they do not need to mirror each other.

Decisions deferred from the System Design are rendered inline with the ho that resolves them, not as a master table. When the practitioner is reading or executing a given ho, the decisions that ho is supposed to resolve are visible in that ho's section. Anything that doesn't tie to a specific v1 ho — visual identity, post-v1 features — is enumerated at the end of the Ho Overview under "Other deferred decisions."
```

---

## Update 5 — Phase Structure as Primary Cognitive Unit in Kamae 4

**Status:** CONFIRMED

**Where:** Section 2.4 (Ho Overview), in the "What 'done' looks like" section.

**Current state:** Section 2.4 says the Ho Overview should define "phase groupings (orientation → foundation → construction → integration → polish)" alongside per-ho fields. This treats phases as one of several fields, equivalent to dependencies and stage estimates.

**Rationale:** Phases are the primary cognitive unit of the Ho Overview. The skill that produces Ho Overviews identifies phases first (natural clusters of work that share a theme), then identifies hos within each phase. Reversing this — listing hos and grouping into phases retroactively — flattens the architecture into a sequence and loses the structural visibility.

**Proposed wording — replace the existing bullet about phase groupings:**

```markdown
- **Phase structure as the primary organizing principle.** Phases come first; hos populate phases. Each phase has a paragraph describing what it produces and what's true at the end of the phase that wasn't true at the start. Hos within a phase are nested under the phase header. The phase structure makes the architecture visible at the planning level.
- For each ho: title, narrative (3–5 sentences in plain English), dependencies, what's in scope (light), what's out of scope (light), and any decisions the ho resolves.
- Release tags at phase boundaries (v0.1, v0.2 ... v1.0) — each completed phase produces a tagged release that marks "this phase is done."
```

---

## Update 6 — Update the Kanyō Evidence Section with shodō

**Status:** SUGGESTED

**Where:** Section 7 (The Kanyō Evidence) — add or replace.

**Current state:** Section 7 describes how Kanyō combined all four framing concerns into a single Ho-00 document, and uses this as the contrast that motivated separating concerns. This was accurate when written. By 2026 there are now multiple project examples — Hōzō (built in three hours), and shodō (currently in planning, with full Kamae chain articulated).

**Rationale:** The methodology has more evidence now. Kanyō remains the historical pilot that motivated the separation. Hōzō demonstrates Ri-stage compression (where the practitioner can collapse Kamae steps because they've internalized them). Shodō demonstrates the full chain articulated cleanly from the start, including Kamae 4 and the inline-decisions principle.

**Proposed wording for an updated Section 7:**

```markdown
## 7. Evidence Across Projects

The framework's structure has been validated across three projects of increasing complexity:

**Kanyō (the pilot, 2025).** Combined all four framing concerns into a single Ho-00 document (351 lines). Worked well enough for a single-learner pilot but conflated concerns that serve different purposes. The framework's separation of Kamae documents was extracted from what didn't work cleanly in this combined approach.

**Hōzō (Ri-stage, 2026).** The practitioner had internalized the methodology enough to compress Kamae steps. The "Three Hours" build session combined Kamae 2 (system design) and Kamae 5 (per-ho documents) into a single specification — and shipped a production application. This compression is a Ri-stage move and not appropriate for new practitioners. It demonstrates that Kamae's structure becomes invisible at mastery, not absent.

**Shodō (full Kamae, 2026).** The first project where all five Kamae documents were articulated cleanly from the start: seed, system design, README, ho overview (12 hos across 6 phases), and individual per-ho documents. Shodō also demonstrated the inline-decisions principle (Kamae 4 renders deferred decisions with their resolving ho rather than in a master table) and the phase-as-release-boundary discipline (v0.1 through v1.0 tied to phase completion).

Each project taught the methodology something the previous project couldn't. Kanyō surfaced the need for separated concerns. Hōzō demonstrated Ri-stage compression as a real outcome of the practice. Shodō validated the full chain at scale and surfaced the inline-decisions and phase-structure principles for Kamae 4.
```

---

## Update 7 — Project Arc Relationship Sharpening

**Status:** SUGGESTED

**Where:** Section 5 (Relationship to the Project Arc).

**Current state:** Section 5 references the Project Arc's universal phases (Orientation → Foundation → Construction → Integration → Polish & Launch) and says the Ho Overview "assigns each planned ho to a phase." This is accurate but understates the difference between universal arc phases and project-specific Kamae 4 phases.

**Rationale:** The Project Arc names *universal* phase patterns that apply to any project. The Ho Overview names *project-specific* phases that map to those patterns. Shodō's phases (Foundation → Ingest Pipeline → Searchable Corpus → Enrichment → Surfaces → Multi-source + Acceptance) are project-specific labels that map to the universal arc but use the project's own language. The framing document should make this explicit.

**Proposed addition to Section 5:**

```markdown
The Project Arc names universal phase patterns. The Ho Overview names project-specific phases that map to those patterns. A project might have a "Searchable Corpus" phase (specific to a search tool) that maps to the Construction phase of the universal arc. The mapping is implicit and doesn't need to be documented in the Ho Overview itself — the universal phases are scaffolding for the practitioner's thinking, not labels that need to appear in every Kamae 4 document.

When the practitioner names project-specific phases, the names should describe what's being built (in the project's own vocabulary), not what the practitioner is doing. "Foundation" is universal; "Ingest Pipeline" is project-specific. Both are valid labels at their respective levels.
```

---

## Update 8 — Release Tags at Phase Boundaries

**Status:** CONFIRMED

**Where:** Section 2.4 (Ho Overview), in the "What 'done' looks like" section. Already integrated into Update 5 above.

**Rationale:** Each phase produces a release. Release tags make progress legible and force "done" judgments. v0.1 = first phase done, v0.2 = second phase done, ... v1.0 = final phase done.

**Note:** Already covered in Update 5's proposed wording. Listed here for completeness so the merge can verify the addition lands.

---

## Update 9 — Section 6 Update

**Status:** SUGGESTED

**Where:** Section 6 (What This Document Does NOT Cover).

**Current state:** Section 6 currently lists three things the document doesn't cover (AI engagement at each stage, how to write good hos from the Ho Overview, and adapting the framing process to non-software domains).

**Rationale:** The list should add: practitioner-scope concerns. The framing document is project-scope. Practitioner scope (the operating discipline, environment setup, the practitioner's profile) lives in a parallel framing document.

**Proposed wording — add a fourth item:**

```markdown
**How the practitioner works across projects.** This document describes Kamae at project scope — the framing for a specific project. The framing for *how the practitioner works* (the operating discipline, environment setup, practitioner profile, the relationship between practitioner stage and the methodology) is practitioner-scope and lives in a parallel document. See [[the-operating-discipline]] (`practitioner/the-operating-discipline.md`) and the practitioner-scope framing document (when it exists).
```

---

## Merge Order Recommendation

If merging incrementally:

1. **Start with Updates 1, 3, and 4** — these are CONFIRMED structural additions that cleanly extend the document. They establish the five-document chain, acknowledge practitioner scope, and clarify the System Design ↔ Ho Overview relationship.

2. **Then handle Update 2 (the naming question)** — needs your decision before merging. If renaming to "Seed," do it as a single pass through the whole document.

3. **Then merge Updates 5 and 8 together** — both touch Section 2.4 (Ho Overview). Single coordinated revision is cleaner than two passes.

4. **Then Updates 6, 7, 9** — SUGGESTED items. Merge as time and energy allow; defer if not.

After all merges, increment the document's version metadata and commit with a descriptive message about the structural changes.
