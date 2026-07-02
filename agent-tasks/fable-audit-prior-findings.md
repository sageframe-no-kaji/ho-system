---
created: 2026-07-01
type: agent-task-context
purpose: Prior-audit findings for the Fable framework audit (see fable-audit-brief.md)
audit-model: Claude Opus 4.7 (1M context)
audit-scope: sageframe-no-kaji-dev/ + sageframe-irori/sageframe-mcp
status: reference
---

# Prior-Audit Findings — Ho System

Compressed synthesis of a four-pass exploration of the Ho System framework and its
application across the practitioner's project corpus, run 2026-07-01. Referenced by
`fable-audit-brief.md`. Fable should read this before beginning its audit; treat it as
evidence, not doctrine — argue with it where the evidence disagrees.

---

## 1. Corpus map — every Ho project discovered

| Project | Purpose | Kamae layers | Hos (state) | Stack | Notable pattern |
|---|---|---|---|---|---|
| **shodo** | Local-first AI conversation corpus engine | 1,2,3,4,5 | 30+ hos + side-track + sidequests | Python (uv, ruff, mypy strict, pytest) | Subproject-A-Harvester side track; 7 smoke-sidequests; ho-smoke-collaborator project skill |
| **sharibako** | Age-encrypted secrets vault (app + CLI) | 1,2,3,4,5 | 4 hos, all closed / in-flight | Swift (multi-product package) | `kamae-2.N` decision addenda (2.1, 2.2); clean per-AT commits |
| **shoshin-no-sono** | Procedurally-generated map / cartographer | 1,2,3,4,5 | 15 hos + ho-A hachure branch | JS (no-build vanilla ESM) | 8-step visual-design method; ho-04 executed outside chain; tuner-landing hos (06.5, 07.5, 07.6); A/B branch |
| **sutra** | Writer's editor, per-piece git as version primitive | 1,2,3,4,5 | 5 hos, ho-00/01 closed, ho-02.1/.2/.3 draft | Tauri 2 (Rust + Svelte + CodeMirror 6) | Adopted+adapted shoshin's design method (HTML output, Pass A/B loop); `splits-from` frontmatter |
| **satori-internal-affairs** | Diagnostic-reasoning ER sim | 1,2,4 (README stands in for 3) | 13 hos across two phases | TS/JS (SvelteKit-ish) | **Phase-prefixed hos** (P1-H0N, P2-H0N); decimal-addendum hos (H03.2) |
| **edelmore/apps/diary** | Cottage-core private diary for children | 1,2,4 (README stands in for 3) | 8 hos + 11 AT specs | SvelteKit 2, Svelte 5, SQLite | **Per-app Kamae chain in a monorepo**; **extraction-hos** as a named ho category; `Ho-N-AT-M` AT naming |
| **edelmore/apps/reader** | Read-along EPUB reader (sibling app) | 1,2,4 | 3 hos, all extraction-shaped | SvelteKit (planned) | Extraction-ho convention (extract shared packages when 2nd consumer arrives) |
| **sageframe-mcp** | Homelab MCP server (hosts, docker, systemd) | Compressed: README=kamae 1+3, discovery.md=kamae 2+4 | ho-00 closed, ho-01 in flight | Python 3.11+, FastMCP, pydantic, mypy strict, 90% coverage | **Compressed Kamae chain** documented in ho-00; MCP as callable verbs over YAML+SSH; read-only-by-default with `SAGEFRAME_MCP_ALLOW_WRITES=0` gate |
| **supacode** (PR contribution) | OSC 11 per-pane background PR against `supabitapp/supacode` | None — GitHub issue #442 substitutes for seed/design/README | 2 hos: ho-01 closed with wrong assumptions, ho-02 supersedes | Swift + Zig (Ghostty embed) | External-project contribution pattern; `ho-process/` scrubbed via git filter-branch; verification stack inherited from host; `supersedes:` frontmatter enforces forward-only |
| **dandori** (meta) | Prep-discipline skill/spec repo | No Kamae (meta-project) | `agent-tasks/deck/` repurposed for a marketing deck build | Mixed (JSX/HTML + skill package) | Non-canonical `agent-tasks/` use; imports `operating-discipline-rationale.md` |
| **pink-reader** | Reading-the-model posture project | CLAUDE.md only | None | N/A | **Rationale-only project** — loads `operating-discipline-rationale.md`, no build |
| **kinhin** | Planned client-delivery workbench (dormant) | Seed + LLM-integration concept | None on disk | HTMX + FastAPI + Postgres (planned) | Now candidate to reframe as MCP using sageframe-mcp template |

### Vocabulary-leak projects (CLAUDE.md but no Ho chain)

- **glassroom** — CLAUDE.md says "Lint with ruff before completing any Ho" — vocabulary present, no chain scaffolding. Python + Playwright scraper.
- **supacode local repo** — `ho-process/` scaffold-only; `CLAUDE.md` is a symlink to `AGENTS.md` (host-repo compat).

### Predates the practice

`audiobook-qc`, `constructive-interference`, `forteller`, `hozo-sentinel`, `kanyo-contemplating-falcons-dev`, `kanyo-viewer`, `m4bmaker`, `palana`, `personal-projects`, `shodo-web`, `ssh-actually`, `claude-gmail-multi`, `claude-leaked-files`, `image_2_ppt-dev`, `kokoro-fastapi` (vendor fork).

---

## 2. Framework artifact-type registry (currently implicit)

The framework operates over **four artifact types**, but only two are named canonically:

| Type | Named in framework? | Purpose | Lifecycle | Origin |
|---|---|---|---|---|
| **Kamae document** | Yes (kamae 1–4) | Project-scale authoring chain | drafted → committed | Practitioner authors in discursive session |
| **Ho document** | Yes (kamae 5) | Bounded session unit; architectural decisions + scope | draft → complete/closed | Practitioner authors after Kamae chain |
| **Agent task (AT)** | Partial — dandori skill authors them, but the framework doesn't clearly label them as a distinct artifact type | Executable delegation; no thinking; lighter-model or subagent friendly | ready → executed (typically one commit) | Authored by dandori after ho decisions are settled |
| **Sidequest** | No | Emergent-discovery record from live use / smoke testing | recorded | Written during use, not planned |

Fifth artifact discovered in shodo, structurally distinct from all four:

| **Side track / Subproject** | No | Bounded severable capability chain within a project; own mini-Kamae fragment | phase-overview → local hos → complete | Practitioner authors when a capability wants its own arc without needing a separate seed/design/README |

---

## 3. Ho ↔ AT split — the operating-layer separation

The most load-bearing pattern in the operating layer. Framework does not currently label
it clearly.

**Structural signature** (from sharibako, sageframe-mcp, sutra, edelmore, satori):

```
ho-process/
├── hos/
│   └── ho-01-vault-core.md          # architectural, human-authored, no code
└── agent-tasks/
    ├── Ho-01-AT-01.md               # executable, dandori-authored, agent-runnable
    └── Ho-01-AT-02.md
```

**The ho document is architectural.** Reads like an architectural decision record. Names
scope, out-of-scope, deferred decisions being resolved, and the AT children via an
`agent-tasks:` frontmatter field. No code. No function signatures. No implementation
detail. Example (sharibako ho-01):

> The vault's complete data model in code. `SharibakoCore` gets the nine operations
> the system design specifies — every vault read and write — with thorough tests and
> no user-facing surface attached.

**The AT is executable.** Has its own frontmatter:

```
type: agent-task
project: sharibako
parent-ho: "01"
task: "01"
model: claude-sonnet-4-6
status: ready
```

Reads as a pastable agent instruction: specific goal, named files, named classes,
named methods, layouts as ASCII, verification commands. Example (Ho-01-AT-01):

> Implement the `SharibakoCore` filesystem layer: the `Shell.run()` internal utility,
> `VaultError` enum, vault directory layout helpers, Yams + Codable models, and the
> five vault operations that don't require `age` decryption — `listScopes`, …

**Operational properties of ATs (practitioner-confirmed 2026-07-01):**

1. **No thinking.** All architectural decisions have been extracted to the ho. The AT is
   procedure.
2. **Lighter-model / subagent friendly.** Because there is no thinking, the AT can be
   dispatched to Sonnet, Haiku, a subagent, or any executor that reliably follows a
   procedural spec. This is the operating discipline's *model choice by task* principle
   made concrete.
3. **Escalation clause: unless the AT finds something new.** If the AT surfaces a
   decision the ho didn't anticipate — an assumption the ho got wrong, a scope surprise,
   an unanticipated dependency — the AT must *unearth it*: stop, surface the finding,
   kick the decision back to the practitioner (or to a heavier model in a discursive
   session). No silent architectural decisions inside an AT. This is the operating
   discipline's *stop for decisions, not permissions* rule applied at the AT layer.
4. **One-commit-per-AT.** Visible signature in the git log. Example from sageframe-mcp:
   `feat(inventory): Host/Service models + sageframe_hosts (Ho-01-AT-01)` →
   `feat(services): SshExecutor + compose walker + sageframe_services (Ho-01-AT-02)` →
   `feat(systemd): Semaphore fan-out + sageframe_systemd_failed (Ho-01-AT-03)`.

**The dandori skill is the bridge.** It reads ho decisions and authors AT specs. This is
not a utility — it's the load-bearing skill of the operating layer. The framework's skill
catalog should probably label it as such.

**The two-tier separation of thinking from coding:**

- **Project-scale**: Kamae chain — thinking artifacts (seed / system-design / README /
  overview) separated from doing artifacts (per-ho documents). Thinking in chat, doing
  in Claude Code.
- **Session-scale**: Ho ↔ AT — architectural thinking (ho) separated from executable
  delegation (AT). Ho authored anywhere; AT executed in Claude Code by whatever executor
  is cheapest that satisfies the spec.

---

## 4. Invented patterns not yet in the framework

Ordered by breadth of use across the corpus.

### 4.1 Kamae addenda (kamae-N.M)

Mid-build architectural decisions issued as decimal-suffixed kamae docs that supersede
parts of the original without mutating it.

- **sharibako**: `kamae-2.1-sharibako-injection-decision.md`,
  `kamae-2.2-sharibako-ownership-decision.md`
- Not observed elsewhere in the corpus (worth Fable verifying against wider search)

This is the forward-only principle applied to architecture. The original kamae-2 stays
frozen; the addendum records what changed and why. Sharibako's ho-03 cleanly cites both
via `builds-on:`.

### 4.2 Visual-design work modality

8-step method for design work partly outside the Ho chain, propagating back via numbered
`.5` hos and a living-register artifact.

- **shoshin-no-sono**: canonical instance. `design/design-process-note.md` documents the
  method: isolate questions → one-question Claude Design sessions → freeze in
  `visual-register.html` → coherence check → spike → live tuners → by-feel landing → A/B
  spikes. Kamae-4 anticipates it with a decision-trigger checkpoint after ho-04 (only
  non-phase-boundary checkpoint). Session prompts live in `design/claude-design/`.
- **sutra**: adopts by explicit reference (`ho-02.1:25`), adapts: HTML output not SVG;
  tuners only for interaction thresholds not visual params; adds Pass A / Pass B loop
  (Claude Design conversation → real-Tauri-WebView iteration); 3-layer alphabet /
  grammar / signature grouping. All drafted, none executed.

Two instances → pattern. Framework has no home for it. Needs: framework-level design
document, `shape: design` per-ho variant, design-ho template, design-work module,
possibly a `ho-kamae-design-collaborator` skill.

### 4.3 Tuner-landing hos

Hos whose deliverable is "land values by feel against real data," not "add feature X."
Distinct topology.

- **shoshin-no-sono**: ho-06.5, ho-07.5, ho-07.6, ho-A-6.0
- **shodo**: informally in ho-04.7 territory
- **sutra**: folded into ho-03/ho-05; by-feel landing deferred to v0.4→v0.5 replan
  checkpoint

### 4.4 Subprojects / side tracks

Bounded severable capability chains within a project, with their own mini-Kamae fragment
and local numbering.

- **shodo**: `Subproject-A-Harvester/` — a whole new toolchain (live-web-API harvester)
  added as a severable side track. Own `00-phase-overview.md` with
  `type: ho-overview-phase`, `stage: kamae-4`, `subproject: A-Harvester`. Local numbering
  ho-A1 through ho-A4. Explicit `supersedes:` clause against main kamae-4 ("browser-
  extension capture path" bullet). Cascade reading order documented. Own smoke pass.

Distinguishing signature vs. a subproject splitting off into its own project: "the work
is a capability, not a separate project — no separate seed, system design, or README."

### 4.5 Sidequests

Emergent discovery records from live use / smoke testing. Not build work.

- **shodo**: 7 files, all `type: smoke-sidequest`, `parent-ho: "04"` (or "05"),
  `status: recorded`. Named after the *finding*, not the *feature*:
  `ho-04-smoke-sidequest-1-education-pivot.md`,
  `ho-04.5-smoke-sidequest-1-untrustworthy-realization.md`,
  `ho-04-smoke-sidequest-2-anthropic-first.md`,
  `ho-04-smoke-sidequest-3-daitzman.md`,
  `ho-05-smoke-sidequest-1-ho-means-step.md`, plus early forms.
- Content: the practitioner's actual query as a user, attempts made, ranked results,
  the finding.
- Lifecycle `recorded`, not `complete` — a distinct third artifact type.

### 4.6 Compressed Kamae chains

Deliberate combination of Kamae layers when project size warrants it.

- **sageframe-mcp**: `ho-00-orientation.md:19` documents it: "README.md serves as
  combined seed + README (Kamae 1 + 3), `docs/discovery.md` serves as combined system
  design + ho-overview (Kamae 2 + 4)."
- **satori-internal-affairs**, **edelmore/apps/diary**, **edelmore/apps/reader**: elide
  a distinct kamae-3 file; README stands in.
- **supacode PR**: no upstream Kamae; GitHub issue #442 stands in for seed+design+README.

Framework needs doctrine on when compression is warranted and what the compression
signals.

### 4.7 Monorepo Kamae (per-app chains)

Root CLAUDE.md imports language module; each app carries its own kamae-1/2/4 +
`ho-process/`.

- **edelmore**: canonical instance. `apps/diary/` and `apps/reader/` each have their own
  seed, system-design, ho-overview, and hos. Root commit: "codify per-app Kamae chains +
  extraction-ho scope discipline."

### 4.8 Extraction-hos

Named ho category. Extract shared packages when the second consumer arrives.

- **edelmore**: three extraction-hos in `apps/reader/ho-process/hos/`:
  `ho-01-extract-design`, `ho-02-extract-book`, `ho-03-extract-narration`. Each extracts
  a `packages/@edelmore/*` package from `apps/diary` when reader becomes a second
  consumer.

### 4.9 Phase-prefixed hos

Hos organized into major phases with a prefix.

- **satori-internal-affairs**: `P1-H0N-DONE-*.md`, `P2-H0N-*.md`. Not observed elsewhere.

### 4.10 Rationale-only projects

A project that exists purely to load `operating-discipline-rationale.md` for posture,
with no build artifacts.

- **pink-reader**: CLAUDE.md stub imports the rationale module. No ho-process/. No code.
- A new *project shape*.

### 4.11 CLAUDE.md → AGENTS.md symlink

External-agent-compat hack.

- **supacode** (local repo): `CLAUDE.md` is a symlink to `AGENTS.md` so both Claude Code
  and other agent hosts read the same file.

### 4.12 External-project contribution

Ho against someone else's codebase. supacode PR is the case study.

**What got kept:** document shape (frontmatter, Think/Execute/Reflect, `supersedes:`),
planning discipline (in-scope/out-of-scope, gate, escalation), agent-runnable execute
prompts.

**What got renegotiated:** no upstream Kamae (GitHub issue substitutes); verification
stack inherited from host repo (`make format && make lint && make test && make build-app`);
commit style matched to host log (no scope prefix, no Co-Authored-By); no coverage floor;
per-step manual verification gates instead of automated coverage; strict staging discipline
("Do not `git add .`; stage only files this ho names"); forward-only enforced harder
("Editing ho-01" listed as first forbidden move); `ho-process/` scrubbed from PR history
via `git filter-branch`.

**Friction the framework has no doctrine for:** the `.gitignore` add-revert-track-filter-
branch sequence to keep Ho artifacts private in a PR against someone else's repo. The
practitioner discovered this needs a stronger firewall than the practice anticipates.

**Load-bearing evidence:** ho-01's Think was wrong on three counts (correctly named);
ho-02 exists purely to correct them via `supersedes:`. Cleanest single instance of
forward-only working under real pressure.

---

## 5. Kinhin — reframe as MCP

Prior state: seed proposes HTMX + FastAPI + Postgres single-user workbench, dormant since
March 2026, not a git repo, no code.

**Insight from sageframe-mcp survey:** the practitioner has already built the pattern
Kinhin needs — a Python + FastMCP server with mypy strict, 90% coverage floor, read-only-
by-default with `SAGEFRAME_MCP_ALLOW_WRITES=0` mutation gate, pydantic models with
`Discriminator("kind")` for structured error unions, per-tool docstrings written for LLM
audience, injection-testable tool impls. Walks a config repo's YAML + compose files, joins
live host state via SSH. Under active development, 13 commits in 2 weeks, real-data
iteration.

**Direct one-to-one mapping to Kinhin:**

- `sageframe_hosts()` → `ho_projects()` — enumerate Ho projects under a vault root
- `sageframe_services(host)` → `ho_status(project)` — return kamae completeness + open/
  closed hos
- `sageframe_systemd_failed()` → `ho_open_across(projects)` — cross-project view
- YAML inventory + compose walker → kamae + ho-process walker
- Read-only-by-default + explicit mutation gates: same pattern applies

Kinhin-as-MCP:
- No duplicate storage. Vault's `ho-process/` remains canonical.
- Client value: BYOM — install MCP in client's Claude Code, Ho practice available as
  tools.
- No Postgres, no HTMX, no Docker deploy.
- Loses the *visualization dashboard* the original seed imagined. If load-bearing,
  add a lightweight static-site-from-vault generator alongside the MCP.

Fable's Kinhin job: draft the reframed seed using sageframe-mcp as template, enumerate
the tool surface. Not "should this be an MCP?" — that decision is made.

---

## 6. The webpage / public-access gap

The framework currently has no reader. Access is:
1. Clone the repo, read markdown in an Obsidian-shaped vault
2. Invoke skills in Claude Code
3. Read GitHub's rendered markdown

No docs site. No rendered navigator. `INDEX.md` is a good taxonomy but requires opening
files to consume. A first-time visitor lands on the README and has no path in.

Kinhin (as either HTMX app or MCP) is *not* the access tier. It's a practitioner workbench
or agent-tool layer; both serve people who already know the framework. The reader tier is
a separate build.

Framework is mature enough to be publicly presented. This is the largest missing piece.

---

## 7. Framework debt visible from a cold read

- `framework/structure/kamae-project-framing.UPDATES.md` — pending updates never merged
  into canonical doc
- `framework/templates/Seed Template Checklist.md` — active TODO list living in a
  templates directory
- `private/project-checklist.md` — Phase 1 codification still open
- `INDEX.md` layer 4 has no entry for `skills/` — the most-active directory is outside
  the numbered taxonomy
- `agent-tasks/` INDEX drift (newer 2026-05-25 spec unindexed)
- Practitioner profile says Sutra is Swift; Sutra is Tauri (Rust + Svelte)
- `~/.claude/modules/languages-swift.md` — placeholder, still not filled in

---

## 8. Nomenclature drift

- **`shape:` values** in use: `ha`, `ri`, `orientation`, occasionally `shu`. No canonical
  enumeration anywhere.
- **Directory naming** for ATs: `agent-tasks/` (sharibako, sageframe-mcp, ho-system root),
  `ho-agent-tasks/` (shodo). Skill named `dandori`. Three names for the same concept.
- **Closure signaling**: `status: complete` in frontmatter (shodo), Reflect trailer line
  `_Executed and Reflected: <date>._` (sharibako with frontmatter permanently `draft`),
  file rename prefix `-DONE-` (satori). Three conventions in three projects.
- **Ho-adjacent artifact types**: sidequest, side track, extraction-ho, tuner-landing ho,
  phase-overview, decision-addendum. All in active use, none in canonical framework
  vocabulary.
- **Ho-overview mutability**: forward-only is clear for closed hos; silent on the ho
  overview. In practice the overview is edited mid-build across all projects (shodo has
  many overview revision commits). Doctrine gap.
- **The word "Ho" itself** — glassroom uses "Ho" in its CLAUDE.md without running the
  chain. Framework has no definition of "Ho" for readers who don't know the practice.

---

## 9. Concepts in active use without names

Do NOT propose names for these. Naming is practitioner-authored. Surface the concept
precisely enough that a good name is discoverable:

1. The mid-build architecture addendum (currently `kamae-N.M` file convention).
2. The visual-design-work modality (currently unnamed method, described in
   design-process-note.md).
3. The "land values by feel" ho category (currently just decimal-suffixed hos
   06.5 / 07.5 / 07.6).
4. The living-register artifact (currently just `visual-register.html`).
5. The severable-capability-chain-inside-a-project pattern (called "side track" and
   "subproject" interchangeably; needs one name).
6. The "escalation clause when the AT finds something new" property of ATs.
7. The two-tier separation of thinking from coding (Kamae chain at project scale; Ho↔AT
   at session scale). This is *the* framework's central operating principle and it has
   no single name.

---

## 10. Open decisions the audit deferred to the practitioner

These are practitioner-authored decisions Fable should surface rather than resolve:

- **Kill or park Kinhin?** Prior recommendation: park with revised positioning (BYOM
  client-delivery MCP; not the framework access tier).
- **Site generator choice** for the docs site (Eleventy is house tooling; Astro / Docusaurus
  are contenders).
- **Kinhin's relationship to the docs site** — same site, own product page, or
  invisible-until-engaged.
- **Sanction or forbid the ho-overview mutation** the practice does.
- **Single closure convention** — pick one of the three current signals.
- **Directory naming for ATs** — pick one of the three current names.
- **`shape:` enumeration** — decide the canonical list and its extensibility.

---

## End of prior-audit findings

The evidence for every claim above is in the four exploration passes run 2026-07-01.
Fable should verify against live files. If evidence disagrees with these findings,
trust the files, not the summary.
