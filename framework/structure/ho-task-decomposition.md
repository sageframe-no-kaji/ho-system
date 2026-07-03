---
id: "2.8"
title: "Ho-Task Decomposition"
type: structure
stage: any
status: draft
tags: [ho-system, structure, agent-task, decomposition, model-choice]
---

# Ho-Task Decomposition

## How Hos and Agent Tasks Compose One Architectural Thought

A mature practitioner doesn't write executable specs inside a ho. The ho carries the
architecture at the level of decisions; agent tasks carry the same architecture at the
level of executable surface. They are one design, written at two resolutions, in
separate documents bound by an explicit parent-child relationship.

This document names that relationship as canonical, and names the four operational
properties that make the agent task a distinct artifact rather than a formatting
convention: procedural content, executor portability, the escalation clause, and
one-commit-per-task.

---

## 1. The Two Registers

A ho document operates at the **architectural register**. It carries decisions, the
reasoning behind them, deferred discoveries, and post-execution reflection. It is read
by humans — the practitioner, future maintainers, anyone trying to understand why the
system is the way it is. Its content is durable: it survives the project and gets
revisited years later.

An agent task operates at the **executable register**. It carries exact files, exact
changes, exact acceptance criteria, exact verification commands. It is read primarily
by an autonomous coding agent — and secondarily by the practitioner reviewing the
agent's output. Its content is operational: once the work is committed and verified,
the task's job is done.

Mixing the registers in one document damages both. The architectural reasoning gets
buried under schemas and signatures; the executable spec gets diluted by paragraphs of
context the agent doesn't need. Extraction resolves the conflict structurally: each
document speaks in one voice, and the relationship between them is preserved by
explicit binding.

### 1.1 The two-tier separation of thinking from coding — mind and hand

The registers are the session-scale instance of the methodology's central operating
principle, which runs at two scales:

- **Project scale — the Kamae chain.** Thinking artifacts (seed, system design,
  README, overview: Kamae 1–4) are separated from doing artifacts (per-ho documents:
  Kamae 5). Thinking happens in discursive sessions; the documents carry the
  conclusions into build sessions.
- **Session scale — the Ho ↔ AT split.** Architectural thinking (the ho's Think
  phase) is separated from executable delegation (the AT). The ho is authored in a
  discursive register; the AT is executed in an agentic IDE by whatever executor is
  cheapest that satisfies the spec.

The same move at both scales: extract the thinking into a durable document, so the
doing can be delegated, verified, and repeated without re-litigating the thinking.

This principle is named **mind / hand**: *mind* is the thinking (the Kamae chain at
project scale, the ho's Think phase at session scale); *hand* is the doing (the
Kamae-5/per-ho layer at project scale, the agent task at session scale). One
architectural thought held at two registers — the thinking extracted into a durable
artifact so the hand can be delegated (cheaper model, later session, agent) without
re-litigating the mind. The Latin epigraph is *mens et manus*. The distinction earns a
diagnostic phrase: **a hand problem** is the execution fighting you while the design is
sound; **a mind problem** is the design itself being wrong.

---

## 2. Three Relationship Types

Agent tasks relate to hos in three distinct ways.

### 2.1 Constitutive Children

An agent task that cannot be understood without the parent. Its Context section
references architectural decisions from the parent's Think phase; its Goal realizes
commitments the parent made; its sibling tasks decompose the same architectural
thought into ordered executable units. This is the dominant pattern in mature
practice.

**Signal:** the task's Context names the parent ho's decisions ("Strict error handling
was chosen over lenient mode in v1; see ho-01 Decision 2") and makes sense only
because that reasoning exists.

### 2.2 In-Session Delegations

A task written mid-session for a bounded piece of work the practitioner is about to
delegate. Architectural framing is light or absent; the parent ho references the task
in its Execute phase, but the task's content is mostly self-contained.

### 2.3 Standalone Tasks

A task outside any ho: maintenance ("bump dependency X, fix any breaks"), surgical
fixes between hos, pre-ho exploration ("inspect a real export and report on its
schema"). Fully self-contained; no parent fields in the frontmatter. Common in
ri-stage work and ongoing maintenance.

---

## 3. The Four Operational Properties

These properties define the agent task as an artifact type. A spec missing any of
them is not yet an AT.

### 3.1 Procedural — no thinking inside

All architectural decisions have been extracted to the ho. The AT is procedure. If
writing the spec requires making a decision, the decision belongs upstream — either
the ho's Think phase resolves it first, or the spec is not ready to author. The
kamae-5 skill enforces this as an anti-pattern: "Authoring dandori specs before the
Think phase has resolved."

### 3.2 Executor-portable — the model field

Because there is no thinking in it, the AT can be dispatched to a lighter model, a
subagent, or any executor that reliably follows a procedural spec. Every AT therefore
names its executor in a required frontmatter field:

```yaml
model: claude-sonnet-4-6    # vendor-agnostic; whatever runs the spec
```

**An unset model is an unmade decision.** The choice follows the operating
discipline's model-choice guidance, matched to the spec's work:

- Architectural / design-heavy content folded into the spec → the most capable
  reasoning model
- Implementation and test-writing → a strong-at-code model, often a cheaper tier
- Verification / review → the most capable model available (a weaker-than-implementer
  reviewer won't catch what the implementer missed)
- Trivial, high-throughput mechanical work → a fast, cheap model

This field is where the operating discipline's *model choice by task* stops being
advice and becomes an encoded, auditable decision. Reading a project's `agent-tasks/`
directory shows exactly which tier executed which work.

### 3.3 Escalating — autonomous until something new surfaces

The AT is executed autonomously — *unless it finds something new*. When execution
surfaces something the ho didn't anticipate — a schema mismatch, a wrong assumption in
the spec, a real-data shape that contradicts the fixtures, an unanticipated
dependency — the executing agent stops and surfaces the finding. It does not adapt
silently, does not rationalize the surprise as "probably fine," and does not acquire
architectural authority because something feels suboptimal mid-execution.

Two mechanisms carry this:

- **Stop Conditions** (a spec section) name the *anticipated* surprises: "If a real
  export reveals shape mismatches with the schema, stop and surface findings before
  modifying the data model."
- **The general rule** covers the unanticipated ones: any surprise halts the loop.
  This is kokoroe guideline 4 ("Halt and surface") and it applies whether or not a
  Stop Condition section exists.

This is the operating discipline's *stop for decisions, not permissions* rule applied
at the AT layer. The decision that comes back goes to the practitioner (or a heavier
model in a discursive session); its resolution lands in the parent ho (see §6), and
execution resumes against the updated record.

This property — bounded autonomy with a halt-and-surface tripwire — is what makes an AT
safe to delegate to a cheaper executor: the AT is **tripwired**. *Escalation* is the
mechanism; *tripwired* is the property. It is the word to reach for in the explanatory
sentence — "ATs are safe to delegate because they are tripwired."

### 3.4 One commit per task

Each AT lands as a single commit. Child-task commits use the `ho-NN:` prefix and
reference the spec by its full ID:

```
ho-01: add VaultCore filesystem primitives (Ho-01-AT-01)
```

Standalone tasks use a conventional prefix appropriate to the work (`deps:`, `fix:`,
`docs:`). The result is a git log that reads as a sequence of ATs grouped by ho —
the decomposition's visible signature, auditable without opening a single document:

```
feat(inventory): Host/Service models + sageframe_hosts (Ho-01-AT-01)
feat(services): SshExecutor + compose walker + sageframe_services (Ho-01-AT-02)
feat(systemd): Semaphore fan-out + sageframe_systemd_failed (Ho-01-AT-03)
```

---

## 4. The Parent-Child Binding

### 4.1 Frontmatter

**Parent ho** declares its children:

```yaml
---
ho: "02"
shape: ha
agent-tasks:
  - Ho-02-AT-01.md
  - Ho-02-AT-02.md
  - Ho-02-AT-03.md
---
```

**Child task** declares its parent by number, plus the fields that make it
independently executable:

```yaml
---
created: 2026-06-30
type: agent-task
project: sharibako
parent-ho: "01"        # parent ho number, zero-padded, quoted
task: "01"             # task number within the ho
model: claude-sonnet-4-6
status: ready          # ready | in-progress | complete | blocked
---
```

**Standalone task:**

```yaml
---
created: 2026-05-09
type: standalone-agent-task
project: shodo
model: claude-haiku-4-5
status: ready
---
```

The binding is bidirectional: a reader arriving at either document navigates to the
other without searching, and tooling can verify that every declared child exists and
every claimed parent lists it.

> Revision note: the original 2.8 specified a `parent:` path field
> (`parent: hos/ho-02-threader-and-chunker.md`). No project uses it; the practiced
> and dandori-specified form is `parent-ho` + `task` above. This revision adopts the
> practiced form.

### 4.2 Naming and location

Child tasks: `Ho-NN-AT-MM.md`, zero-padded, in `ho-process/agent-tasks/` — a sibling
directory to `ho-process/hos/`, because tasks are referenced from hos but read
independently by the agent, standalone tasks share the directory, and pattern-matching
across all tasks is easiest in one place.

Standalone tasks: `Standalone-AT-YYYY-MM-DD-slug.md`, in the same directory. This keeps
standalone tasks in the `AT` family alongside child `Ho-NN-AT-MM.md` and mirrors the
`type: standalone-agent-task` frontmatter; the date orders them. Exploration tasks are a
*use-case* of the standalone type, not a distinct type — they take the same filename, not
an `Exploration-AT-` prefix. Historical lowercase `agent-task-*` filenames stand as
record; new projects conform to the `Standalone-AT-` form.

### 4.3 Spec anatomy

The full format — required sections (frontmatter, Goal, Files, Required Changes,
Acceptance, Verification, Commit) and optional sections (Context, Problem, Do Not,
Stop Condition) — lives in the dandori FORMAT reference embedded in the kamae-5 skill.
The structural rules this document owns: acceptance criteria verify by command,
verification commands match acceptance one-to-one, and optional sections appear only
when warranted.

---

## 5. The Execute Phase as Structural Index

When a ha-shaped ho has constitutive children, its Execute phase is no longer where
the practitioner does the work — it **indexes** the children: one paragraph per task
naming what it produces, followed by a link.

```markdown
## Phase 2 — Execute

Two tasks with a clean seam at the encryption boundary.

### Ho-01-AT-01 — Filesystem primitives and pure-filesystem operations

Shell.run() utility, VaultError enum, models, layout helpers, and the five
operations that don't require decryption. No `age` binary needed for these tests.

→ `ho-process/agent-tasks/Ho-01-AT-01.md`

### Ho-01-AT-02 — age invocation and encryption operations
...
```

The architectural reasoning lives upstream in Think; operational precision lives
downstream in the task documents. Execute is the seam. Inline execution in the ho is
the exception, reserved for moves too small to warrant their own document.

---

## 6. Discoveries Crossing Back

When a child task's execution reveals that a parent ho's decision needs revision, the
revision is recorded in the **parent**, not the child. The Shodō ho-02 pattern: the
Think phase committed to a cap of 4; AT-03's real-data inspection showed the cap
forced false splits in 75% of threads; the cap was raised to 50; the revision lives in
ho-02's Decision 3 (marked as revised post-execution) and Reflect (full evidence);
AT-03 carries only the executable result.

The principle: **the ho is the durable architectural record.** A reader returning two
years later reads the ho to understand the architecture, including changes execution
surfaced; they read the tasks only to know exactly what shipped.

This section and §3.3 are the same loop seen from two ends: the escalation clause is
how a discovery gets *out* of the AT; this rule is where the resolution gets
*recorded*.

---

## 7. When to Extract, When the Ho Dissolves

**Default: extract.** Any executable spec long enough to compete with the ho's
architectural narrative belongs in a child task. When in doubt, extract — the cost of
an unnecessary child is one small document; the cost of a cluttered ho is degraded
architectural legibility.

**Exception: trivial moves.** A single short code block, work the practitioner does by
hand, a change too small to carry its own acceptance section.

**When the ho dissolves:** if the work has no architectural content — no decisions, no
deferred discoveries, no reflection worth preserving — there should be no ho. Write a
standalone task. The threshold is architectural content, not work size: a small change
resolving a non-trivial deferred decision deserves a ho; a large change that is pure
implementation of decided architecture is a standalone task.

**Cardinality guidance** (from the kamae-5 skill): zero tasks (orientation, simple ri,
small ha that fits one conversation), one task (a bounded change worth a surgical
spec), or N tasks (typical ha decomposition; three to five is common; more than seven
suggests the ho should split).

---

## 8. The Dandori Bridge

The translation from a ho's Think phase to executable specs is a craft move with a
name: **dandori** (段取り — preparation, staging). It is operationalized twice:

- **Embedded in the kamae-5 skill** (`dandori/` toolkit: DANDORI.md, FORMAT.md,
  KOKOROE.md, examples) — the Ho System-scoped variant that authors constitutive
  children during per-ho authoring.
- **As a standalone skill** — the lean variant for tasks outside any ho.

The five translation moves (decisions → interface specifications; architectural
commitments → file boundaries; out-of-scope items → Do Not entries; deferred
discoveries → Stop Conditions; verification dependencies → ordering) live in
FORMAT.md. The five kokoroe guidelines the *executing* agent operates under
(context-first, spec as authorization, verify by command, halt and surface, honor the
boundary) live in KOKOROE.md and install at the project level via CLAUDE.md.

Dandori is not a utility skill. It is the load-bearing bridge of the operating layer —
the mechanism by which the two registers stay separated in practice. The skill catalog
should present it at that altitude.

---

## 9. Worked Examples

- **Shodō ho-02** (threader and chunker): six argued decisions, one deferred
  discovery, three constitutive children, and the 4→50 cap revision — the canonical
  discoveries-crossing-back instance.
- **sharibako ho-01** (Vault Core): two children with a clean seam at the encryption
  boundary (AT-01 needs no `age` binary; AT-02 does), sequenced so AT-01 commits
  before AT-02 opens; Reflect records that the anticipated ho-level split was not
  needed because the AT seam carried the density.
- **sageframe-mcp ho-01**: three children, three commits, `model:` field per spec —
  the one-commit-per-AT signature quoted in §3.4.

---

## 10. Related Framework Documents

- [[ho-structure|Ho Structure]] — what makes a ho a ho; the five invariants
- [[artifact-type-registry|Artifact Type Registry]] — the AT as one of the artifact
  types; sidequests and dogfood findings distinguished
- [[agent-task-spec|Agent Task Specification]] — ⚠ predates this document; its
  `[NNN] - Agent Task` filename convention and `tasks/` location are the Kanyō-era
  format and should be updated or marked historical (see framework debt)
- [[kamae-project-framing|Kamae: Project Framing]] §2.6 — canonical paths
- [[template-selection-guide|Template Selection Guide]]

---

_This document is part of the Ho System framework. It defines how a single
architectural thought decomposes across a parent ho and one or more child agent
tasks, and the four operational properties of the agent task as an artifact. The
pattern is canonical, not optional._
