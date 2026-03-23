# System Design Structure — Reference

This is the structural reference for the Ho System System Design document. The System Design is the second document in the Kamae project framing chain (Seed → System Design → README → Ho Overview). It takes the seed's architectural opinions and turns them into committed decisions.

The System Design is a decision document, not a description document. Every section should contain decisions with rationale — not descriptions of possibilities.

---

## Where System Designs Come From

The seed provides the input: a core idea, an audience, constraints, architectural opinions, scope boundaries, and open questions. The System Design takes that input and answers the question the seed deliberately left open: *how does this actually work?*

The seed says "I think I need a database and a web frontend, and here's why." The System Design says "PostgreSQL, FastAPI, here's the data model, here's how the components connect." Every decision should be traceable back to the seed's core idea — if an architectural choice doesn't serve the project's stated purpose, it needs justification or removal.

---

## The Seven Sections

### 1. Architecture Overview

The major components of the system and how they connect. This is the diagram — at minimum ASCII art, ideally a clear visual showing what talks to what.

**Decompose by experience and purpose, not by technical layer.** The components should match how the person thinks about the project, not how a developer would organize the codebase. "The writing surface, the version engine, and the piece manager" — not "the view layer, the data access layer, and the business logic layer."

For each component: one sentence on what it does, one sentence on what it owns, one sentence on what it talks to. The full detail comes in the component breakdown below.

### 2. Component Breakdown

What each component does, what it owns, how it communicates with other components.

For each component, specify:
- **Responsibility** — what it does and doesn't do
- **Interface** — the operations it exposes to other components (if the person is technical enough to engage with this)
- **Boundaries** — what stays inside vs. what crosses the boundary
- **Replaceability** — if relevant, whether this component could be swapped (e.g., a replaceable editor behind a contract)

### 3. Core Interaction

The main thing the user does with the product, traced end to end through the architecture. Not abstractly — concretely.

"The user does X. Component A handles Y. Data moves to Z. State changes to W. The user sees V."

This is usually the most productive section to develop in conversation. It's where the architecture proves it works — or where gaps become visible. If the core interaction can't be traced cleanly through the proposed components, the architecture is incomplete.

A product may have more than one core interaction, but there's usually one that matters most. Start with that one.

### 4. Data Model

What lives on disk (or in the cloud, or in a database). What format. Where. How it's found and managed.

This section should answer:
- What does a single [primary artifact] look like as files/records?
- Where does everything live? What's the directory/storage structure?
- What metadata does the system track? Where does it live?
- What format decisions were made, and why?

**Check data model decisions against the seed's philosophy.** If the seed values portability, the data model should use standard formats. If the seed values simplicity, the data model shouldn't require a database. If the seed says "X is the system of record," don't introduce a second system of record.

### 5. Technology Stack

Languages, frameworks, key libraries — with rationale for each choice.

**Every technology choice from the seed should show evidence of evaluation.** If the seed said "Python" and the System Design says "Python," there should be a sentence explaining why Python is correct, not just an assertion that it's the choice. If alternatives were evaluated (even briefly), note that.

Format as a table for quick reference, with a rationale column. Follow with prose sections for any choices that required significant evaluation (e.g., comparing two libraries, evaluating a newer technology against an established one).

### 6. Deployment Model

Where does this run? How does it get there? How does the user access it?

For a desktop app: distribution method, signing/notarization, auto-update mechanism.
For a server: hosting, containerization, CI/CD, DNS.
For infrastructure: which hosts, what networking, what access model.

Note any deployment decisions that constrain the architecture (e.g., App Store sandboxing limiting file system access, container networking affecting component communication).

### 7. Scope Boundaries

The seed's scope boundaries restated as architectural commitments. These are not aspirational omissions — they are things the architecture enforces.

Two subsections:

**MVP Architectural Commitments.** What the system will NOT do at MVP, stated as constraints. "Single user — no multi-user access at the data layer" is an architectural commitment. "We'll add multi-user later" is a wish.

**Architecturally Prepared For (Not Built).** Future capabilities that the architecture explicitly accommodates without implementing. "The branch model supports guest access; the networking and UI for it are not built." This section demonstrates that the architecture has room to grow without being redesigned.

---

## Additional Outputs

### Identity Statement

A hero paragraph and tagline developed during the conversation. This is a design artifact — a one-paragraph statement of what the product is that every subsequent decision can be tested against. The tagline captures the philosophy. The hero paragraph captures the experience.

### Provisional Ho Sequence

A table of planned hos with titles, stages (shu/ha/ri), and purposes. This is directional — it will be refined in the Ho Overview (Kamae 4). But the System Design is the right place to propose it because the architectural decisions shape what needs to be built and in what order.

### Deferred Decisions

An explicit list of open questions, each assigned to a specific ho. Not "to be determined" — "deferred to Ho 1, which will prototype both options and test against these criteria." Well-scoped deferrals are decisions about how to decide.

---

## What "Done" Looks Like

Someone reading the System Design should understand:

- The system architecture (major components and how they interact)
- Data flow (what goes in, what comes out, what's stored where)
- Technology stack (languages, frameworks, key libraries — with rationale)
- Deployment model (where does this run, how is it accessed)
- Scope boundaries (what's v1, what's future, what's explicitly out)

The document includes at least one architecture diagram showing the major components and their relationships.

No decision area has unanswered questions that block the next document in the chain. Every open question is either resolved or explicitly deferred to a specific ho with evaluation criteria.

The README (Kamae 3) can be written from this System Design without needing to come back and resolve anything.

## What "Done" Does NOT Look Like

- File-level project structure (that's the README or Ho 0)
- Step-by-step build instructions (that's individual hos)
- An exhaustive technology comparison (decide and move on)
- Implementation details (specific library APIs, timeout values, polling intervals)

---

## Frontmatter Convention

```yaml
---
created: [date]
status: draft
type: system-design
project: [project-name]
stage: kamae-2
kamae-chain: concept → **system-design** → readme → ho-overview
builds-on: [project]-kamae-1-seed
next: [project]-kamae-3-readme
---
```

## Filename Convention

`[project]-kamae-2-system-design.md`

---

## The Kamae Chain

The System Design sits between the seed and the README:

- **Seed (Kamae 1)** provides opinions — "I think I need X, here's why"
- **System Design (Kamae 2)** commits to architecture — "here's how the system works, here's what was decided and why"
- **README (Kamae 3)** commits to scope — "here's what it does and how to use it"
- **Ho Overview (Kamae 4)** commits to build order — "here's the sequence and dependencies"

Each document increases commitment. The seed is exploratory. The System Design is structural. The README is concrete. The Ho Overview is operational.
