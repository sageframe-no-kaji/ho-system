---
id: "2.8"
title: "Ho-Task Decomposition"
type: structure
stage: any
status: draft
version: "1.0"
tags: [ho-system, structure, agent-task, decomposition]
---

# Ho-Task Decomposition

## How Hos and Agent Tasks Compose One Architectural Thought

A mature practitioner doesn't write executable specs inside a ho. The ho carries the architecture at the level of decisions; agent tasks carry the same architecture at the level of executable surface. They are one design, written at two resolutions, in separate documents bound by an explicit parent-child relationship.

This document names that relationship as canonical. Inline execution within a ho is the exception, reserved for moves too small to warrant their own document. Extraction is the default.

---

## 1. The Two Registers

A ho document operates at the **architectural register**. It carries decisions, the reasoning behind them, deferred discoveries, and post-execution reflection. It is read by humans — the practitioner, future maintainers, anyone trying to understand why the system is the way it is. Its content is durable: it survives the project and gets revisited years later.

An agent task operates at the **executable register**. It carries exact files, exact changes, exact acceptance criteria, exact verification commands. It is read primarily by an autonomous coding agent — and secondarily by the practitioner reviewing the agent's output. Its content is operational: once the work is committed and verified, the task's job is done.

Mixing the registers in one document damages both. The architectural reasoning gets buried under SQL and pydantic models; the executable spec gets diluted by paragraphs of context the agent doesn't need. The reader — whether human or agent — has to constantly shift cognitive register to extract what they came for.

Extraction resolves the conflict structurally. Each document speaks in one voice. The relationship between them is preserved by explicit binding.

---

## 2. Three Relationship Types

Agent tasks relate to hos in three distinct ways. The framework's earlier documentation conflated the second and third.

### 2.1 Constitutive Children

An agent task that is a **constitutive child** of a ho cannot be understood without the parent. Its Context section references architectural decisions from the parent's Think phase. Its Goal realizes commitments the parent made. Its sibling tasks decompose the same architectural thought into ordered executable units. Detached from the parent, the child task is a list of operations without a reason.

This is the dominant pattern in mature practice. Most agent tasks under a ha-shaped ho are constitutive children.

**Signal:** the task's Context section says "Architectural decisions from [parent ho]'s Think phase" and lists them. The task makes sense only because the parent's reasoning is available.

### 2.2 In-Session Delegations

An agent task written mid-session for a bounded piece of work the practitioner is about to delegate. The architectural framing is light or absent — the task captures what to do, not why. The parent ho references the task in its Execute phase, but the task's content is mostly self-contained.

This pattern appears when a session involves both architectural thinking and one or two delegated executions that don't warrant the full decomposition treatment.

### 2.3 Standalone Tasks

An agent task that exists outside any ho. Quick fixes, maintenance work, small features that don't merit a session wrapper. The task is fully self-contained. No `parent:` field in the frontmatter; the task stands alone.

Standalone tasks are common in ri-stage work and during ongoing project maintenance.

---

## 3. The Parent-Child Binding

When an agent task is a constitutive child of a ho, the relationship is made structural through frontmatter and document conventions.

### 3.1 Frontmatter

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

**Child task** declares its parent:

```yaml
---
created: YYYY-MM-DD
type: agent-task
status: complete
parent: hos/ho-02-threader-and-chunker.md
---
```

The binding is bidirectional. A reader arriving at either document can navigate to the other without searching. Tooling that audits framework coherence can verify that every declared child exists and every claimed parent contains the corresponding entry.

### 3.2 Naming convention

Constitutive children follow the parent's number:

- `Ho-02-AT-01.md` — task 01 under ho-02
- `Ho-02-AT-02.md` — task 02 under ho-02
- `Ho-02-AT-03.md` — task 03 under ho-02

The naming makes the parent-child relationship visible in the file listing. Standalone tasks use the date-slug convention (`agent-task-YYYY-MM-DD-slug.md`); they do not share the ho's number space.

### 3.3 Context section

The child task's Context section names the parent's relevant decisions explicitly:

> Architectural decisions from ho-02's Think phase:
>
> - Exchanges are stored entities; one row per exchange in a new `exchanges` table
> - Threading lives on exchanges (`thread_id`, `position_in_thread`); no separate threads table
> - ...

This is not redundant with the parent. The parent argues the decisions; the child surfaces only the decisions the task is realizing. The agent reading the task gets exactly what it needs to execute, without needing to absorb the full architectural reasoning.

---

## 4. The Execute Phase as Structural Index

When a ha-shaped ho has constitutive children, its Execute phase changes function. It is no longer the section where the practitioner does the work. It is the section that **indexes** the children — naming each, stating what it produces, and linking to its document.

A typical Execute phase under decomposition:

```markdown
## Phase 2 — Execute

The work decomposes into three agent tasks, executed in order. Each is its own document at `ho-process/agent-tasks/`.

### Ho-02-AT-01 — Schema and models

Migration 002 adds `exchanges` and `chunks` tables, plus `messages.exchange_id` via `ALTER TABLE`. ... No threading or chunking logic — just the data shape and the configuration surface.

→ `ho-process/agent-tasks/Ho-02-AT-01.md`

### Ho-02-AT-02 — Threader

...

### Ho-02-AT-03 — Chunker

...
```

Each entry is a paragraph naming what the child does, followed by a link. The architectural reasoning lives upstream in Think; the operational precision lives downstream in the task documents themselves. Execute is the seam between them.

This is the canonical shape. Execute as a section where the practitioner writes code inline is the special case, reserved for moves too small to warrant their own document — a one-line config change, a single test addition, a trivial dependency bump.

---

## 5. Discoveries Crossing Back

Real work surfaces things the design didn't anticipate. When a child task's execution reveals that a parent ho's decision needs revision, the revision is recorded in the **parent**, not the child.

The pattern from Shodō ho-02:

1. ho-02's Think phase committed to `THREAD_MAX_EXCHANGES = 4` as Decision 3.
2. During AT-03 execution, real-data inspection revealed the 4-cap forced false splits in 75% of threads.
3. The cap was raised to 50.
4. The revision is documented in ho-02's Think section (with a note marking it as revised post-execution) and in the Reflect section (with the full evidence).
5. AT-03 itself contains the executable result of the revised decision — not the deliberation.

The principle: **the ho is the durable architectural record.** Decisions live there, including decisions revised mid-execution. The agent task documents the work that was actually done; the ho documents the design, including changes the work surfaced. A reader returning to the project two years later reads the ho to understand the architecture; they read the agent tasks only if they need to know exactly what code shipped.

---

## 6. When to Extract

The decomposition question — "does this work warrant a child task, or stay inline?" — has a clear default and a small set of exceptions.

**Default: extract.** Any executable spec long enough to compete with the ho's architectural narrative for attention belongs in a child task. SQL schema, pydantic models, function signatures, test bodies, exact file modifications — these all live in the task, not the ho.

**Exception: trivial moves.** A change small enough that documenting it inline does not damage the ho's legibility may stay. Signals:

- A single short code block (a config constant, a one-line import addition)
- Work the practitioner intends to do by hand rather than delegate
- A change too small to verify with its own acceptance and verification sections

Even these may extract. The rule is conservative: when in doubt, extract. The cost of an unnecessary child task is one small document; the cost of a ho cluttered with executable detail is degraded architectural legibility.

---

## 7. When the Ho Dissolves

If a piece of work has no architectural content — no decisions to record, no deferred discoveries, no reflection that needs preserving — there should be no ho. The agent task is sufficient on its own. This is what standalone tasks are for.

**Test:** if the ho would contain only a frontmatter, a one-line Context, and a link to a single child task, the ho is not earning its document. Write the task as standalone; skip the ho.

The threshold is architectural content, not work size. A small change that resolves a non-trivial deferred decision deserves a ho. A large change that's pure implementation of an already-decided architecture is a standalone task.

This rule also prevents a degraded pattern: hos written as ceremonial wrappers around tasks that would be clearer standalone. The ho should appear when there is architectural thinking to capture, and only then.

---

## 8. Worked Example: Shodō ho-02

The canonical example of constitutive-children decomposition. The full documents live at:

- Parent: `hos/ho-02-threader-and-chunker.md`
- Children: `agent-tasks/Ho-02-AT-01.md`, `Ho-02-AT-02.md`, `Ho-02-AT-03.md`

What the parent carries:

- Six architectural decisions, each argued in 1–3 paragraphs
- One discovery deferred to execution time (threading patterns on real data)
- A Phase 2 that indexes the three children with a one-paragraph narrative each
- A Phase 3 Reflect that records what survived, what was revised, and the evidence behind the revision

What each child carries:

- A Context section naming the specific architectural decisions it realizes
- Exact files (created and modified)
- Required Changes at executable resolution — SQL schemas, pydantic class definitions, test bodies, shell commands
- Acceptance criteria each verifiable by a command
- Verification commands matching acceptance one-to-one
- A commit format

Neither document tries to do the other's job. Read in isolation, the ho explains why; the tasks explain what. Read together, they are one architectural thought at two resolutions.

The 4-to-50 cap revision is the load-bearing evidence: it happened during AT-03's execution, but it is recorded in ho-02's Decision 3 (revised inline with a note) and Reflect (full evidence). AT-03 itself contains the executable result; it does not re-argue the decision. This is the durable-record principle in action.

---

## 9. Related Framework Documents

- [[ho-structure|Ho Structure]] (framework/structure/ho-structure.md) — what makes a ho a ho; the five invariants
- [[agent-task-spec|Agent Task Specification]] (framework/templates/agent-task-spec.md) — the format and use of agent tasks
- [[ha-ho-template|Ha Ho Template]] (framework/templates/ha-ho-template.md) — the shape under which constitutive-children decomposition most often appears
- [[ri-ho-template|Ri Ho Template]] (framework/templates/ri-ho-template.md) — where standalone tasks are most common
- [[template-selection-guide|Template Selection Guide]] (framework/templates/template-selection-guide.md) — which template to use when

---

_This document is part of the Ho System framework. It defines how a single architectural thought decomposes across a parent ho and one or more child agent tasks. The pattern is canonical, not optional._
