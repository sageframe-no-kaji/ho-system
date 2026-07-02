---
created: 2026-07-01
type: agent-task
audit-target: Ho System framework (this repo) + practitioner's Ho System practice
audit-model: Fable (Anthropic frontier model)
prior-audit: agent-tasks/fable-audit-prior-findings.md
status: ready
---

# AT — Ho System framework audit and web-presence vision (Fable)

## Preamble

You are auditing the Ho System — a methodology for human-AI collaborative software
development authored by Andrew Todd Marcus (Tyro). The framework has been in serious use
across 10+ projects over 6+ months and is mature enough to be publicly presented. This
audit produces three deliverables plus a preliminary discovery pass. Read the full brief,
plus `agent-tasks/fable-audit-prior-findings.md`, before beginning any deliverable.

You are running in Claude Code inside the framework repo at
`/Users/atmarcus/Vaults/sageframe-no-kaji-dev/ho-system/`. You have direct file access
to the entire vault. Your output writes back into this repo — no pasteboards, no
attachments.

## What the Ho System is (one paragraph)

The Ho System is a practice, not a productivity methodology. It structures AI-assisted
software development around a five-document authoring chain (Kamae: seed → system design
→ README → ho overview → per-ho documents), an operating discipline (verification stack,
planning mode, bounded sessions, forward-only progress), and Claude Code skills that
operationalize the chain. The load-bearing operating pattern is a two-tier separation of
thinking from coding: at project scale the Kamae chain separates thinking artifacts
(kamae 1–4) from doing artifacts (per-ho documents), and at session scale the Ho ↔ AT
split separates architectural thinking (ho document) from executable delegation (agent
task, dispatchable to lighter models or subagents because there is no thinking in it).
The theory: agents conform to well-encoded environments, so the practitioner's job is to
build environments that carry the discipline. Artifacts do the teaching.

## Preliminary — discovery pass

`fable-audit-prior-findings.md` (§1) enumerates the corpus a prior audit discovered.
Verify and extend:

1. Re-enumerate every project under
   `/Users/atmarcus/Vaults/sageframe-no-kaji-dev/` and
   `/Users/atmarcus/Vaults/sageframe-irori/` that shows Ho evidence
   (`ho-process/`, `kamae-N-*` files, CLAUDE.md importing the framework, `hos/` with
   numbered ho files, dandori/agent-tasks directory).
2. If you find projects the prior audit missed, add them. If you find that a project the
   prior audit listed shows evidence disagreeing with the summary, correct it.
3. Report the corrected/extended corpus as a short table at the top of your output before
   the deliverables.

## Evidence to read (live files)

### Read strategy — trust the prior audit for corpus breadth

You have the prior audit's evidence in `fable-audit-prior-findings.md`. It sampled ten
Ho projects and characterized each. Your job is judgment on the framework — not
exhaustive re-verification of the corpus. Read as follows:

- **Framework itself, design-process-note, sageframe-mcp, supacode PR hos, and
  pinkteaming.net reference** — read fully. These are the load-bearing primary sources.
- **One representative Ho project — sharibako** — read fully (kamae 1/2/2.1/2.2/3/4,
  all 4 hos, all 7 ATs, CLAUDE.md). Cleanest instance of the practice; ideal reference
  for the ho ↔ AT split and kamae-N.M pattern.
- **Every other Ho project** — spot-check only. Read the kamae-2 system design and one
  representative ho per project. Trust the prior audit's characterization of the rest
  unless you find specific reason to doubt it. If you doubt, read more of that
  specific project and log the disagreement.
- **shodo** deserves slightly deeper spot-check because it has the most invented
  patterns (side track, sidequests, subproject nesting). Read the Subproject-A phase
  overview and one sidequest in full so your side-track-vs-sidequest doctrine is
  grounded in the primary source, not the prior audit's summary.

If you decide the prior audit was wrong about a pattern, that finding goes in
`disagreements-with-prior-audit.md` with the specific file/line evidence that changed
your mind.

### Order

1. **Framework root** — `README.md`, `INDEX.md`, `CLAUDE.md`, `Shodo-seed.md`
2. **Operating discipline** — `/Users/atmarcus/.claude/modules/operating-discipline.md`
   and `operating-discipline-rationale.md`. Load both fully — the rationale is the
   philosophical grounding, load-bearing.
3. **Framework structure** — everything under `framework/foundation/`,
   `framework/structure/`, `framework/templates/`, `practitioner/`
4. **Skills** — `skills/ho-skill-overview.md` and each skill's `SKILL.md`
5. **Every Ho project you discovered.** For each: kamae-1 seed, kamae-2 system design,
   kamae-4 ho overview, `ho-process/hos/` contents, `ho-process/agent-tasks/` (or
   equivalent), project CLAUDE.md.
6. **Design-work primary source** —
   `/Users/atmarcus/Vaults/sageframe-no-kaji-dev/shoshin-no-sono/design/design-process-note.md`.
   Read fully before writing about design work.
7. **MCP precedent** —
   `/Users/atmarcus/Vaults/sageframe-irori/sageframe-mcp/` — especially
   `README.md`, `CLAUDE.md`, `docs/discovery.md`, `src/sageframe_mcp/server.py`,
   `src/sageframe_mcp/models.py`, `ho-process/hos/ho-00-orientation.md`. This is the
   template for Kinhin-as-MCP.
8. **External-project contribution case study** —
   `/Users/atmarcus/Vaults/sageframe-no-kaji-dev/supacode/ho-process/` is empty
   currently. The two hos and the PR structure survive in reflog-reachable git objects.
   Use `git show 262a97ee` from within that repo to recover them. Related: the
   `supacode-cli` skill definition.
9. **Kinhin** — `/Users/atmarcus/Vaults/sageframe-no-kaji-dev/kinhin/`. Note: the
   practitioner has decided to reframe as MCP using sageframe-mcp as template. Do not
   re-open that decision.
10. **Prior audit** — `agent-tasks/fable-audit-prior-findings.md`. Argue with it where
    the evidence disagrees.

## Deliverable 1 — Docify the patterns the practice invented

The practice has out-run the framework. Load-bearing patterns were invented under
project pressure and reused, but the framework has not codified them. Author the missing
framework documents. Each sub-section below is a candidate document; mark each as:

- **`draft included`** — you author a full draft into the framework
- **`stub included`** — you author a scaffold with the argument sketched but not
  finished, ready for practitioner completion
- **`recommend future draft`** — you argue the doc should exist and describe what it
  needs to contain, without drafting

Do not overreach. The practitioner will decide what to merge.

### 1.a — Artifact-type registry

The framework currently operates over four (arguably five) artifact types but only names
two clearly. See prior-findings §2. Author the type registry as a first-class framework
document. Types to cover at minimum:

- **Kamae documents** (kamae 1–4)
- **Ho documents** (kamae 5)
- **Agent tasks (ATs)** — currently authored by dandori skill but not clearly labeled
  as an artifact type in the framework. Include the operational properties (procedural,
  no thinking, lighter-model/subagent friendly, escalation clause when new decisions
  surface, one commit per AT typically).
- **Sidequests** — emergent-discovery records from live use/smoke; lifecycle
  `recorded`; named after the finding, not the feature. See prior-findings §4.5.
- **Side tracks / Subprojects** — planned severable capability chains with their own
  mini-Kamae fragment (own `type: ho-overview-phase` doc, local numbering, explicit
  `supersedes:` clause). See prior-findings §4.4. Distinguish clearly from sidequests.

### 1.b — The Ho ↔ AT split

Document the split as the operating layer's counterpart to the Kamae chain — the
session-scale separation of thinking from coding. See prior-findings §3 for the full
signature and operational properties. Cover:

- Ho as architectural artifact, AT as executable artifact
- The `agent-tasks:` frontmatter link
- Per-AT `model:` field as *model choice by task* made concrete
- **The escalation clause** — ATs are autonomous unless they find something new; then
  they stop and unearth the finding
- dandori skill as the load-bearing bridge (elevate its position in the skill catalog)
- One-commit-per-AT as the visible practice signature

This may be the highest-leverage single addition to the framework. Currently implicit;
should be explicit.

### 1.c — Kamae addenda (kamae-N.M)

See prior-findings §4.1. Doctrine on mid-build architectural decisions issued as
decimal-suffixed kamae docs that supersede parts of the original without mutating it.
When to issue one, how to link from downstream hos (`builds-on:`), how the readership
knows what supersedes what.

### 1.d — Visual-design work modality

See prior-findings §4.2 and read `shoshin-no-sono/design/design-process-note.md` fully
first. This is the highest-leverage single existing document in the corpus. Address:

- Framework-level treatment of the 8-step method
- Whether `shape: design` per-ho variant should exist
- Design-ho template (author it if warranted)
- Design-work module for CLAUDE.md import
- Whether a `ho-kamae-design-collaborator` skill should exist
- The living-register artifact as a new artifact category
- Sanction the "Claude Design conversation outside the Ho chain, propagate via
  `.5` ho" pattern explicitly

### 1.e — External-project contribution doctrine

See prior-findings §4.12. supacode is the case study. Framework needs to answer: how
does a practitioner run Ho against someone else's codebase without polluting their PR?
Cover:

- Which Kamae layers compress or elide (usually all — the upstream issue substitutes)
- Verification stack inheritance from host repo
- Coverage-floor renegotiation (usually dropped)
- Commit-style matching
- Stricter forward-only via `supersedes:` frontmatter
- **The `ho-process/` privacy problem** — how to keep Ho artifacts out of upstream
  PR history. supacode used `git filter-branch` as the eventual solution; the
  practice needs a cleaner doctrine (e.g., ho-process/ in a sibling vault directory,
  not in-tree in the fork).

### 1.f — Other invented patterns

For each, decide draft/stub/future. See prior-findings §4:

- Compressed Kamae chains (§4.6) — doctrine on when compression is warranted
- Monorepo Kamae (§4.7) — per-app chains inside a monorepo
- Extraction-hos (§4.8) — named ho category for extracting shared packages
- Tuner-landing hos (§4.3) — hos whose deliverable is "land values by feel"
- Phase-prefixed hos (§4.9)
- Rationale-only projects (§4.10)
- CLAUDE.md → AGENTS.md symlink (§4.11) — external-agent compat
- Ho-overview mutability doctrine (§4 general) — the practice edits the overview
  mid-build across all projects; forward-only is silent on this. Decide and write.
- Closure signaling — three conventions in use; pick one and justify.

### 1.g — Glossary

Draft a canonical framework glossary. Entries at minimum: kamae, ho, dandori, shu-ha-ri,
shape, phase, addendum, tuner, spike, register, sidequest, side track, extraction-ho,
agent task, AT, orientation ho, closure, forward-only, Reflect. Add anything else the
framework uses.

### 1.h — Changelog

Propose a framework changelog convention and seed it with recent structural changes
visible in the git log (forward-only principle addition, canonical kamae filename
convention, ho-task-decomposition integration, kamae-5 dandori-toolkit adoption).

### 1.i — Stale-reference cleanup

Enumerate stale references. Known: practitioner profile says Sutra is Swift; it's Tauri.
`~/.claude/modules/languages-swift.md` still a placeholder. Look for others.

### 1.j — Framework debt

List framework debt visible from a cold read. Prior audit found:
`kamae-project-framing.UPDATES.md` unmerged; `Seed Template Checklist.md` in templates
dir; `private/project-checklist.md` Phase 1 open; INDEX layer-4 skills gap; agent-tasks
INDEX drift.

### 1.k — Concepts without names

List every actively-used concept that lacks a name. Prior-findings §9 has a starting
list. Do NOT propose names — that is practitioner-authored. Describe each concept
precisely enough that a good name is discoverable.

## Deliverable 2 — Vision for the framework's public web presence

The framework has no reader. See prior-findings §6. This is the largest missing piece.

Produce a vision at the resolution of a system design (not a wireframe). No mockups.
Describe how it works and why, in the register the framework uses. This is a Kamae 2
document for the site, essentially.

Answer at minimum:

1. **Audience** — practitioners, curious readers, prospective clients. Same site or
   different landing surfaces?
2. **Onramp** — the framework is philosophical and vocabulary-heavy. How does the site
   handle the gradient from "what is this?" to "I want to try this on my project"
   within one minute?
3. **Information architecture** — INDEX.md's 7-layer taxonomy works for insiders and is
   opaque to newcomers. Propose the site's navigation. Reconcile with INDEX.md.
4. **Skills as first-class citizens** — skills are executable practice. Own pages? How
   does a non-Claude-Code reader understand what a skill is?
5. **Kamae chain visualization** — should there be a canonical rendering? What does it
   look like?
6. **Examples** — Kanyo pilot exists as a real Ho System project. Case study? Walked
   through? Encountered early?
7. **Design language** — precise, restrained, systems-oriented in documents. Describe
   the experience as web presence, not a mood board.
8. **Technical stack** — practitioner uses Eleventy for other static sites, Cloudflare
   Pages deploy via sageframe-dharma. Justify a choice or explain why the choice doesn't
   matter.
9. **Phasing** — v1 in a week, v2 in a month, v3 with real design work. What is the
   minimum viable framework site?
10. **Out of scope** — what the site is NOT. Interactive tooling? Hosted Kinhin? Blog?
    Newsletter? Community?
11. **The external-project use case** — a prospective client wanting to hire the
    practitioner to run Ho against their code needs a landing that isn't the same as
    a practitioner onboarding. How does the site handle this?
12. **Kinhin's relationship to the site** — Kinhin is being reframed as an MCP. Same
    site? Own product page? Invisible until a client engagement?

### Creative-opinion clause — the site as an instance of the practice

Do NOT produce a vanilla docs site. The practitioner has built one ambitious site
already — `pinkteaming.net`, source at
`/Users/atmarcus/Vaults/sageframe-dharma/pinkteaming.net/`. **Read it before you write
Deliverable 2.** Especially:

- `pinkteaming.net/site/index.html` and `pinkteaming.net/site/manifesto/`
- `pinkteaming.net/design/Pink_Teaming_How the Page Reads.md`
- `pinkteaming.net/design/Pink_Teaming_The Practice of Reading.md`

The load-bearing insight of pinkteaming.net is that **the site is an instance of the
practice, not documentation about the practice**. Pink teaming is durational reading,
so the site enforces durational reading — the manifesto walks itself down the left
side of the screen at reading pace with no controls, real embeddings power the "deep"
visualization, level-based content (no code / one demo, light math / two demos, real
math) escalates by reader commitment, and interactive demos are embedded in the reading
argument rather than sequestered in a playground.

The Ho System is a different practice, so a copy of pinkteaming.net would be wrong.
But the *principle* transfers. Bring creative opinions about what the Ho System site
could enforce or embody about the practice it presents. Argue for opinions, not just
list them. Candidates to consider (not exhaustive; propose your own):

- **The Kamae chain as a walked path.** The site's primary navigation could be the
  chain itself — seed → system-design → README → overview → hos — with each step
  gated on the previous. A visitor navigates the framework the way a practitioner
  authors a project. Forward-only browsing.
- **Real artifacts, not sanitized examples.** kanyo pilot is a real Ho System project;
  its actual kamae chain and ho documents could be renderable inline as case study,
  with real frontmatter, real Reflect trailers, real supersedes: fields. The site
  reads real Ho artifacts the way pinkteaming reads real embeddings.
- **Live skill demonstration.** Skills are executable practice. A visitor could see a
  skill's decision-tree unfold in-flow — not a code sample, an animation of the
  conversation the skill would produce with a practitioner. Or a static rendering of a
  real session transcript that ran a real skill.
- **The ho ↔ AT split rendered spatially.** Ho documents on one axis, ATs on another.
  Watching an ho decompose into its ATs as a visual argument for the separation of
  thinking from coding.
- **Site as its own instance.** The site itself could have been built with the Ho
  System, and the site could show that — its own kamae chain, its own hos, its own
  ATs, its own Reflect notes. The framework demonstrating itself.
- **Typography as vocabulary.** pinkteaming.net uses three typefaces for three
  registers (prose / UI / code). Ho System has its own registers — the discursive
  Kamae document, the procedural AT, the emergent sidequest. Different fonts, or
  different weights, or different rhythms for each.
- **Durational discipline.** The framework asks for bounded, forward-only, verified
  work. The site could enforce something analogous — a minimum time-on-page for
  primary reading, a chain-navigation-only mode, no skip-ahead.

Non-goals for the creative-opinion clause:

- Do not propose gimmicks that don't earn their place. Every interactivity choice must
  argue for a specific principle the framework holds. If it doesn't, it is decoration.
- Do not propose interactivity that requires interactive tooling on the site (hosted
  Kinhin, live chat with the framework, agent-backed Q&A). See out-of-scope §10.
  The site is static plus client-side JavaScript. Real embeddings, real artifacts,
  and real animation are all achievable statically; agentic surfaces are not.
- Do not sanitize the framework's voice. The rationale document is philosophical and
  dense; the site should be too. This is not a landing page for a SaaS product. It is
  the front door to a practice.

Argue for two or three concrete interactivity commitments the v1 site should make.
Justify each against the framework's actual principles. If you have a strong
disagreement with the pinkteaming.net-as-reference framing, say so and argue for a
different reference or none.

## Deliverable 3 — Kinhin-as-MCP seed

The decision to reframe Kinhin as an MCP is made. Author the reframed seed. See
prior-findings §5. Use `sageframe-mcp` as the direct template — same stack (Python +
FastMCP + pydantic + mypy strict + 90% coverage floor), same read-only-by-default
posture with an explicit mutation gate, same one-commit-per-AT discipline.

Cover in the seed:

- Product framing — proprietary client-delivery MCP; BYOM; practitioner installs it in a
  client's Claude Code so Ho practice becomes available as tools
- Tool surface enumeration — start from the mapping in prior-findings §5, extend as the
  read of sageframe-mcp's actual tool patterns warrants
- Read/write posture — which tools mutate (`create_ho`, `close_ho`, `record_devlog`) and
  which don't; mutation gate design
- Relationship to the docs site (Deliverable 2, question 12)
- What Kinhin is NOT (not the framework reader; not a practitioner replacement; not
  Sageframe integration)
- Explicit rejection of the prior HTMX + FastAPI + Postgres form and why

This is a real seed, not a critique. Write it as a Kamae 1 document.

## Output — where and how

Write your output to this directory:

```
/Users/atmarcus/Vaults/sageframe-no-kaji-dev/ho-system/private/audit/fable-2026-07-01/
```

Structure:

- `README.md` — one-page executive summary of your findings, with a table of contents
  linking to the deliverables below
- `deliverable-1-docification/` — one file per sub-section (1a–1k). For sub-sections
  where you drafted a full doc, name the file after the doc's proposed framework
  location (e.g., `framework-structure-artifact-type-registry.md`) so the practitioner
  can move it in place after review. For stubs and future-drafts, name descriptively.
- `deliverable-2-web-presence-vision.md` — one file, complete
- `deliverable-3-kinhin-mcp-seed.md` — one file, ready to move to
  `/Users/atmarcus/Vaults/sageframe-no-kaji-dev/kinhin/kinhin-seed.md` after review
- `discovery-corpus-map.md` — the corrected/extended corpus table from the preliminary
  pass
- `disagreements-with-prior-audit.md` — every place you disagreed with
  `fable-audit-prior-findings.md` and why

At the end of the session, return a short summary of what you wrote — file paths,
one-line descriptions each, and any specific claims from the prior audit you overturned.
Do NOT return the full text of the deliverables in the summary; the files are the
deliverables.

## Constraints

1. **Do not propose names for unnamed concepts.** Naming is practitioner-authored.
   Surface the need. Describe each concept precisely enough that a good name is
   discoverable.
2. **Argue with the prior audit where you disagree.** Disagreements as much as
   confirmations. Log them in `disagreements-with-prior-audit.md`.
3. **Do not summarize what the practitioner already knows.** Assume the operating
   discipline is understood. Spend tokens on audit and vision.
4. **Read the design-process-note fully** before writing anything about design work in
   the framework.
5. **Where recommendations depend on a practitioner decision** (site generator choice,
   blog vs no-blog, Kinhin's placement on the site), name the decision and its axes.
   Do not resolve them.
6. **Systems language.** Define developer jargon on first use.
7. **DO NOT modify existing framework files during this session.** This is authorship
   of new documents into `private/audit/fable-2026-07-01/` only. The practitioner will
   review, edit, and merge findings into `framework/` as a separate deliberate pass —
   probably its own ho.
8. **Do not modify anything outside** `/Users/atmarcus/Vaults/sageframe-no-kaji-dev/ho-system/private/audit/fable-2026-07-01/`. Reads across the vault are fine; writes are not.

## Unattended-run discipline

The practitioner is not watching this session. Behave accordingly.

1. **Do not ask clarifying questions.** The brief is complete. If something is
   ambiguous, make a judgment call, execute against it, and log the ambiguity in
   `disagreements-with-prior-audit.md` under a "Practitioner decisions surfaced during
   audit" section. Include: what was ambiguous, what you decided, why, and what the
   alternative was. The practitioner reviews these after the run.
2. **Do not stop to confirm.** No "should I proceed with…?" prompts. No "before I
   continue, want me to…?" Make the call and execute. If the call is wrong, the
   practitioner corrects it in review; that is faster than stalling the whole run.
3. **First step: create the output directory.** `mkdir -p
   /Users/atmarcus/Vaults/sageframe-no-kaji-dev/ho-system/private/audit/fable-2026-07-01/`
   and write `README.md` in it as your first artifact — a one-page skeleton with the
   deliverable structure and "in progress" markers. This way the practitioner sees
   the run is alive and knows where output is landing.
4. **Write incrementally.** Do not hold all authored content in context and write at
   the end. Every completed sub-deliverable gets written to disk immediately. If the
   session dies mid-run, everything completed is on disk.
5. **Update the top-level `README.md`** in the output directory as you complete each
   sub-deliverable — flip its marker from "in progress" to "complete" and add the
   file path. This is the practitioner's dashboard when they return.

## Escalation clause (for real surprises)

The unattended-run discipline above covers ambiguity and scope calls. This clause is
narrower — it covers real architectural surprises:

If you find something that changes the audit's *premise* — the prior audit was wrong
about a load-bearing claim, a project uses a pattern that invalidates a deliverable's
framing, a file the brief instructs you to read doesn't exist or contradicts what the
brief says it contains — log it in `disagreements-with-prior-audit.md` with the
specific file/line evidence. Continue the audit; do not stop and do not ask. If the
surprise is severe enough that continuing would produce misleading deliverables,
write what you can under the new premise, and flag prominently in the top-level
`README.md` that the practitioner should read the disagreements file first.

Do NOT silently absorb the finding into a deliverable. Every real surprise becomes a
surfaced note in the disagreements file.
