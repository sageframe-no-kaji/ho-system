# Learning Interview

A reference for orientation-shaped hos. Loaded when authoring ho-00, a learning ho, or a replan checkpoint that requires a concept primer for new territory.

The learning interview is the function that produces section 2 of an orientation document — the concept primers for tech the practitioner is meeting for the first time in the project. It exists because architectural-authority practitioners frequently work with stacks where some pieces are familiar and some are new, and the new pieces need framing before the practitioner can direct work against them.

---

## What this function does

The interview walks the project's tech stack — every dependency, library, protocol, model, format, and standard the project commits to — and identifies what's new territory for the practitioner. For each new piece, it generates a concept primer: a paragraph-level introduction that names what the thing is, what role it plays in the project, what to know going in, and where to read more.

The output goes into section 2 of the ho-00 orientation document (or section 2 of a learning ho, or the framing section of a replan checkpoint that introduces new tech).

The output also classifies each new concept by depth-of-engagement:

- **Pick-up-in-flight** — common, well-documented, short ramp. Brief primer + a link, learn while using.
- **Pre-read** — non-trivial mental model required before productive use. Substantive primer + canonical doc, read before opening the ho that touches it.
- **Its-own-ho** — large enough that learning is the work itself. Becomes a learning ho or a Think-phase decision in a future ho.

---

## Process

### 1. Walk the stack

Read the project's `CLAUDE.md`, system design, and architecture document (or equivalent). Extract every named dependency, library, protocol, format, model, and standard. Be inclusive — the goal is a complete list before triage. Examples of what counts:

- Language frameworks and libraries (FastAPI, Pydantic, SQLAlchemy, Click, HTMX)
- Storage backends (SQLite, sqlite-vec, FTS5)
- Models (bge-m3, bge-reranker-v2-m3)
- Protocols (MCP, WebSockets)
- Algorithms (RRF, BM25, HNSW)
- Format standards (JSON Lines, OpenAPI)
- Build/dependency tools the project actually uses (uv, ruff, hatchling)

### 2. Triage against practitioner familiarity

For each item, ask: does the practitioner already work with this? Don't guess from inference. **Don't assume familiarity from transitive tool use.**

The trap: "the project uses FastAPI, so the practitioner knows pydantic" — wrong. FastAPI uses pydantic internally, but a practitioner who reaches for FastAPI for the route layer may never have explicitly written a pydantic model. Same trap applies to: "the project uses Click, so the practitioner knows argparse"; "the project uses uv, so the practitioner knows pip"; "the project uses HTMX, so the practitioner knows server-side rendering."

When in doubt, include and let the practitioner say "skip this, I know it." The cost of an unneeded primer is one paragraph the practitioner skims past. The cost of a missing primer is the practitioner directing work against tech they don't actually understand.

Ask the practitioner directly when stack items are ambiguous: "Are you familiar with pydantic? sqlite-vec? MCP?" Don't ask twenty questions in a row — group the ambiguous items, ask in one batch.

### 3. Generate primers at the right depth

For each new concept, write a concept primer that includes:

- **What it is** — one to two sentences, plain language, no buried jargon.
- **Role in the project** — what specific job does this thing do here. (Not "what could it do" — what it does in this project.)
- **What to know going in** — the mental model that makes the rest comfortable. The thing that, if missing, makes everything else feel opaque.
- **Resource** — one canonical link to read more. Official docs preferred over tutorials.

Length: paragraph-level. Two to four sentences plus the resource. Not a one-liner ("pydantic is a validation library"); not a tutorial.

If the concept has subtle pieces that interact in this project, name them. E.g., for sqlite-vec: "stores vectors as virtual tables, joins to FTS5 for hybrid search, distance via `vec_distance_cosine`." That's not a tutorial; it's the shape of how the thing fits the project.

### 4. Classify depth

For each primer, attach a classification:

- **Pick-up-in-flight** — primer alone is enough. Practitioner reads it, opens the ho that uses the thing, learns by doing.
- **Pre-read** — practitioner should read the canonical doc before opening the relevant ho. Add the explicit instruction.
- **Its-own-ho** — the concept is large enough that "learning it" is the work of a session. Promote to a learning ho.

The classification is a hint, not a contract. The practitioner can override.

### 5. Order by appearance in the build

Order primers by which ho first uses each concept, not alphabetically and not by importance. The practitioner reading top-to-bottom encounters the concepts in the order the build will encounter them. This makes the document feel like a tour of the project, not a glossary.

If two concepts both appear in ho-01, group them together but keep the within-group order arbitrary.

---

## Anti-patterns

- **One-line primers.** "Pydantic is a data validation library." That's not a primer; that's a definition. Practitioner reading this doesn't know what they need to know.
- **Tutorial-length primers.** Five paragraphs explaining how to write a pydantic model. Too long; that's what the canonical doc is for.
- **Missing the role-in-project sentence.** Primers that describe what the thing is in general but don't say what it does here. The practitioner has to infer the connection — costly and error-prone.
- **Listing concepts the practitioner already uses fluently.** Padding the section with "Python," "Git," "SQLite (the engine, not the extensions)" — concepts the practitioner has been working with for years. Trim these. The section is for new territory.
- **Inferring familiarity from transitive tool use.** Already named above. Worth repeating: FastAPI ≠ pydantic familiarity, Click ≠ argparse familiarity, HTMX ≠ server-rendering familiarity. Ask, don't assume.

---

## Output shape (section 2 of orientation document)

```markdown
## 2. New concepts

The project introduces tech that may be new territory. Each concept below has a primer + a resource. Classification:

- **Pick-up-in-flight** — read primer, learn while using.
- **Pre-read** — read the resource before opening the relevant ho.
- **Its-own-ho** — promoted to a separate learning ho (see ho-X.5).

### <Concept name> — *<classification>*

[Primer paragraph. What it is, role in the project, what to know going in.]

Resource: [canonical link]

### <Next concept> — *<classification>*

[...]
```

Order: by first appearance in the build sequence.

---

## When the interview gets re-run

Concept primers aren't only ho-00 work. They get re-run when:

- **A replan checkpoint introduces new tech.** The original ho-00 didn't anticipate this concept; it surfaces mid-build because of a discovery or a scope shift. The replan checkpoint gets a small primer section for the new piece.
- **A learning ho is dedicated to one concept.** The whole ho is the primer plus exploration. The shape is still orientation, but section 2 is deeper than ho-00's typical paragraph-level treatment.
- **A new dependency lands mid-build.** If ho-N pulls in a library that ho-00 didn't anticipate (because the architecture evolved), the per-ho document for ho-N includes a small "new concept" callout in its Think phase rather than a full orientation re-run.

Re-runs follow the same process: walk → triage → generate → classify → order. The output is shorter (often one or two new concepts, not a full section) but structurally identical.

---

## Lesson from real practice

In the session that authored shodō ho-00, the first draft missed pydantic. The reasoning was: "the project uses FastAPI, so pydantic is in scope but probably familiar." This was wrong — the practitioner hadn't written pydantic models directly before. The miss was caught when the practitioner pushed back on what was actually new for him.

The lesson is encoded above as the anti-pattern: don't infer practitioner familiarity from transitive tool use. When in doubt, include and let the practitioner trim. The asymmetry of cost (one extra paragraph vs. opaque tech downstream) heavily favors inclusion.
