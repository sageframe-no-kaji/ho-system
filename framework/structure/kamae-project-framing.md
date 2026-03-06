---
id: "2.1"
title: "Kamae: Project Framing"
type: structure
stage: n/a
status: draft
version: "1.0"
tags: [ho-system, kamae, project-planning, framing]
---

# Kamae (構え): Project Framing

## The Pre-Ho Phase: From Idea to Buildable Plan

---

## 1. What Kamae Is

In martial arts, **kamae (構え)** is the ready stance — how you position yourself before engaging. You're not fighting yet, but you're oriented correctly. Feet set, weight balanced, awareness open. You don't rush in; you set your stance first.

In the Ho System, kamae is everything that happens before the first ho. It takes a project from its earliest articulation — the core idea, the problem it addresses, the research that grounds it — and produces a structured plan that hos can be written from. Without it, the first ho is guessing at scope, the second ho discovers the architecture was wrong, and the third ho gets thrown out entirely.

Kamae begins with ideation, not after it. The discipline of understanding a problem space before proposing to build in it — what the Ho System calls [[precedential-thinking|precedential thinking]] — is the first act of project framing, not a prerequisite to it. The [[project-seed|Project Seed]] (framework/templates/project-seed-template.md) captures the output of that thinking and becomes the evaluative foundation against which all downstream decisions are judged.

The Kanyō pilot did this implicitly — the Ho-00 overview document was a single artifact that combined brainstorming, architecture, tool selection, and ho sequencing into one 350-line document. It worked, but it conflated concerns that serve different purposes and that change at different rates. Separating them makes the process repeatable and the outputs more useful.

Project Framing produces four documents, in order. Each builds on the previous one and increases the level of commitment:

```
Seed → System Design → README → Ho Overview
```

When the Ho Overview is complete, you have enough to write individual hos from the [[shu-ho-template|shu-stage template]] (framework/templates/shu-ho-template.md) or whichever template is appropriate for the learner's level.

---

## 2. The Four Documents

### 2.1 The Project Seed

**What it is:** The core idea of the project — the philosophical foundation from which everything else follows. The seed captures the problem, the landscape of what exists, the vision, the audience, the constraints, an initial architectural opinion, and honest self-assessment of what the builder knows and wants to learn. It is the product of precedential thinking: research, reflection, and genuine engagement with the problem space.

**What it's NOT:** A brainstorming dump. The seed is structured enough that someone else — a collaborator, an AI, a future version of yourself — can read it and understand what you're trying to build and why. Enthusiasm is welcome. Messiness is fine in places. But the seed should reflect understanding of the territory, not just excitement about the destination.

**What "done" looks like:**

Someone reading this document should understand:

- What the problem is and why it persists (not just that it exists)
- What exists in the space and where the genuine gap lives
- What the project does, in plain language
- Who it's for
- The builder's initial architectural thinking (opinions, not specifications)
- What constraints reality imposes (hardware, budget, time, skills)
- What the builder already knows and what's genuinely new territory

**What "done" does NOT look like:**

- Committed technology choices (those are finalized in System Design)
- File structures or code architecture (too early)
- Concrete scope (that's the README)
- A polished narrative (this is working material)

**The seed's evaluative role:** The seed is not a document you write once and file away. It is the reference point against which all downstream decisions are judged. When something isn't working three hos in, you go back to the seed and ask: is the core idea still right? If yes, the problem is downstream. If no, you change the seed — deliberately, with eyes open, with a note about what changed and why. The revised seed becomes the new evaluative foundation.

In architecture, this is the _parti_ — the core organizational idea that every design decision is evaluated against. The seed is the parti of the project.

**Relationship to what comes next:** The seed is the input to the System Design. It has opinions about architecture; the System Design turns those opinions into decisions. It has a vision of what the project accomplishes; the README turns that vision into concrete scope.

**Example from Kanyō:** The origin story (conversation with Claudia Goldin on a flight), the vision (automated falcon timeline), the motivating enthusiasm, the landscape research into existing wildlife monitoring tools. In the Kanyō pilot this was never written as a standalone document — the seed was internalized by the builder, whose background in systems architecture and design meant the precedential thinking happened naturally. The framework makes this phase explicit so it can be taught and facilitated.

**Example from Hōzō:** The seed document that described the pain point (home-lab users want off-site ZFS backups without a second NAS), the landscape (existing tools require both machines running), the idea (wake-on-demand orchestrator), the constraints (specific hardware, home network), and the architectural opinion (single controller container, WOL-based).

**Template:** See [[project-seed|Project Seed Template]] (framework/templates/project-seed-template.md) for the full template with section descriptions.

---

### 2.2 System Design

**What it is:** The structured technical vision. How the system works, what the major components are, how they connect, what technology choices anchor the architecture. Written for someone who needs to understand _what you're building_ at a systems level.

The System Design takes the seed's architectural opinions and turns them into decisions. The seed says "I think I need a database and a web frontend, and here's why." The System Design says "PostgreSQL, FastAPI, here's the data model, here's how the components connect." Every decision should be traceable back to the seed's core idea — if an architectural choice doesn't serve the project's stated purpose, it needs justification or removal.

**What it's NOT:** Implementation details. The System Design says "the controller sends a Wake-on-LAN packet, waits for SSH, runs syncoid, and shuts down the remote machine." It does NOT say "use paramiko with a 120-second timeout and poll every 5 seconds." Those decisions happen in individual hos.

**What "done" looks like:**

Someone reading this document should understand:

- The system architecture (major components and how they interact)
- Data flow (what goes in, what comes out, what's stored where)
- Technology stack (languages, frameworks, key libraries — with rationale)
- Deployment model (where does this run, how is it accessed)
- Scope boundaries (what's v1, what's future, what's explicitly out)

A good System Design includes at least one architecture diagram (even ASCII art) showing the major components and their relationships.

**What "done" does NOT look like:**

- File-level project structure (that's the README or Ho 0)
- Step-by-step build instructions (that's individual hos)
- An exhaustive technology comparison (decide and move on)

**Relationship to what comes next:** The System Design is the input to the README. The System Design answers "how does it work." The README answers "how do I use it."

**Example from Kanyō:** The technical architecture section of Ho-00: detection pipeline diagram, infrastructure choices (Proxmox, Docker, ZFS), event detection types, data model. This was the strongest part of the combined document.

**Example from Hōzō:** The system architecture diagram (main server → WOL → remote backup box), the workflow sequence (wake → SSH → syncoid → verify → shutdown), the proposed component breakdown (controller container, remote agent, VPN layer).

---

### 2.3 README

**What it is:** The polished project scope, written as if the project already exists. What it does, how to install it, how to use it. This is the document that ships with the repository.

**Why write it now:** Writing the README before the code exists forces clarity about what "done" looks like. If you can't describe how someone would install and use the finished product, you don't yet know what you're building. The README is a forcing function for scope.

The README takes the System Design's decisions and makes them into concrete, user-facing scope. Its philosophical stem — what the project is _for_, who it serves, why it matters — comes from the seed. Its technical content — what it does, how it works, how to install it — comes from the System Design. The README is where intent meets specification.

The README changes as the project develops. Details sharpen after each ho. Features are added or cut. But the core of it — what this project is and who it's for — should trace back to the seed. When the README starts drifting from the seed's intent, that's a signal to revisit the seed and decide whether the intent has changed.

**What "done" looks like:**

The README should be publishable as-is on day one of the repository. It should include:

- Project name and one-sentence description
- What the project does (user-facing, not technical)
- Installation / setup instructions (can include placeholders like "coming soon" for unbuilt features)
- Usage examples (the commands or interactions a user would perform)
- Requirements (what the user needs to have)
- Development setup (how a contributor would get started)
- License

**What "done" does NOT look like:**

- A tutorial (that's what hos are for)
- An architecture document (that's the System Design)
- A marketing page (factual, not promotional)

**Relationship to what comes next:** The README, along with the Seed and System Design, is fed to AI to generate the Ho Overview. It provides the concrete scope definition that constrains what the ho sequence needs to accomplish.

**Example from Kanyō:** The repo structure, success criteria (minimum viable and stretch goals), and technology stack sections of Ho-00 served this purpose. A standalone README was not written until later.

**Example from Hōzō:** The README.md included in the Ho-00 document — installation instructions, quick start with config example, requirements for controller and remote box, development setup.

---

### 2.4 Ho Overview

**What it is:** The sequence plan. What hos are needed, in what order, with what dependencies. This is the project arc made concrete — it turns the System Design and README scope into a buildable sequence of bounded sessions.

The Ho Overview is possible precisely because the upstream documents have already made the hard decisions. You can't plan a build order for a system whose architecture is undecided. You can't scope sessions for a project whose boundaries are undefined. The Seed established the core idea. The System Design committed to architecture. The README defined scope. The Ho Overview sequences the execution.

**What it's NOT:** The individual hos themselves. The Ho Overview says "Ho 3 implements the backup workflow: wake, SSH, syncoid, verify, shutdown." It does NOT provide the step-by-step instructions for doing that work. Those come from writing the actual ho using the appropriate template.

**What "done" looks like:**

The Ho Overview should define:

- A numbered sequence of hos covering the full project scope
- For each ho: title, goal (one sentence), deliverable (one concrete artifact), and estimated stage (shu/ha/ri)
- Dependencies between hos (what must be complete before each one can start)
- Phase groupings (orientation → foundation → construction → integration → polish)
- Explicit identification of what's in the arc vs. what's deferred

**What "done" does NOT look like:**

- Detailed ho content (that's what templates are for)
- A rigid contract (the arc will change as the project develops)
- Every possible ho identified (new hos emerge mid-project; that's expected and fine)

**Relationship to what comes next:** The Ho Overview is the document from which individual hos are written. Feed the Ho Overview plus the relevant template to AI and ask it to generate Ho N.

**Example from Kanyō:** The "Ho Structure" section of Ho-00 that outlined the phases (Foundation → Automation → Frontend → Polish) with specific ho titles and scope. Compact but sufficient.

**Example from Hōzō:** Not yet written as a standalone document. The Ho-00 project setup document was written directly, effectively skipping this step. The framework formalizes what Kanyō did informally.

---

## 3. The Chain

The four documents form a chain of increasing commitment. Each one takes the previous document's output and narrows what's possible — turning opinions into decisions, decisions into scope, scope into a buildable sequence:

```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│  SEED              "Here's the problem, the territory,       │
│  (core idea)        and what I think should exist."          │
│                     → Problem, landscape, vision, audience,  │
│                       constraints, architectural opinion     │
│                                                              │
│         ↓ opinions become decisions                          │
│                                                              │
│  SYSTEM DESIGN     "Here's how it would work."               │
│  (architecture)     → Components, data flow, tech stack      │
│                                                              │
│         ↓ decisions become scope                             │
│                                                              │
│  README            "Here's what it does and how to use it."  │
│  (polished scope)   → Install, usage, requirements           │
│                                                              │
│         ↓ scope becomes sequence                             │
│                                                              │
│  HO OVERVIEW       "Here's how we build it."                 │
│  (sequence plan)    → Numbered hos, dependencies, phases     │
│                                                              │
│         ↓ generates                                          │
│                                                              │
│  INDIVIDUAL HOS    "Here's what to do in this session."      │
│  (from templates)   → Parts, verification, devlog            │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

Each step increases commitment. The seed is exploratory — opinions are provisional, architecture is directional, scope is approximate. The System Design commits to architecture. The README commits to scope. The Ho Overview commits to a build order. The individual hos commit to specific session-level work.

**The chain is not always linear.** Writing the System Design may reveal that the seed's vision is too broad. Writing the README may expose a gap in the System Design. Writing the Ho Overview may reveal that a component needs to be split across multiple hos that weren't obvious from the System Design alone. Expect to loop back and revise earlier documents as later ones surface problems.

**The seed persists as the evaluative reference.** When looping back, the question is always: does this revision serve the core idea? If the System Design is fighting the seed, either the architecture needs to change or the seed does. Making that distinction consciously — rather than letting the project drift — is one of the most important disciplines in the framing phase.

---

## 4. When to Do What

### For a Brand-New Project

Write all four documents in order. Don't skip steps. The seed might take an hour or two — but the precedential thinking that feeds it may have taken days, weeks, or longer. The System Design might take an hour. The README might take 30 minutes. The Ho Overview might take an hour. Total investment in the documents themselves: 3–5 hours before writing any hos.

The seed's time budget is different from the other documents because it draws on upstream work. The precedential thinking — researching the landscape, trying existing tools, talking to people who have the problem — may happen over days or weeks before you sit down to write the seed. The writing itself captures what you've already been thinking. The other three documents are produced in a more concentrated burst, each building on the last.

This is not wasted time. It's the cheapest place to discover that your scope is wrong, your architecture doesn't work, or your project needs to be split into two projects. Fixing these problems in the framing phase costs hours. Fixing them in Ho 6 costs weeks.

### For a Project That Already Has Momentum

If you've already started building (as happened with Kanyō), you can write the framing documents retroactively. This is less about planning and more about _documenting decisions that were already made_ — which still has value, because it surfaces implicit assumptions and makes the arc visible.

Writing the seed retroactively is particularly valuable. The precedential thinking already happened — you already know the landscape, the constraints, the core idea. Writing it down creates the evaluative reference that lets you assess whether your in-progress decisions still serve the original intent.

### For a Follow-Up Project Using Existing Patterns

If you've completed one project with the Ho System and are starting a second (as with Hōzō following Kanyō), the framing phase is faster. You already know the methodology, the templates, and the rhythm. The seed and System Design are where the new thinking happens. The README and Ho Overview follow established patterns.

---

## 5. Relationship to the Project Arc

Project Framing produces the _plan_. The Project Arc is the _execution_. The Ho Overview is the bridge between them — it's the last framing document and the first arc document.

The project arc phases (from the [[design-seed|Design Seed]] (framework/design-seed.md) §3.2):

|Phase|Hos|Focus|
|---|---|---|
|**Orientation**|0–0.5|Tooling, environment, mental model formation|
|**Foundation**|1–2|Project structure, core infrastructure, first working component|
|**Construction**|3–6|Primary system capabilities, iterative build-out|
|**Integration**|7–9|Assembly, deployment, system-level concerns|
|**Polish & Launch**|10+|Refinement, documentation, public deployment|

The Ho Overview assigns each planned ho to a phase. Individual hos may shift between phases as the project develops — that's expected. The arc is a guide, not a contract.

---

## 6. What This Document Does NOT Cover

**How to engage AI at each stage.** The framing documents are natural places for AI collaboration (developing a seed through conversation, evaluating architecture decisions, generating a ho sequence from a system design). But the _how_ — what prompts to use, which AI mode (thinking vs. agent) is appropriate, how to evaluate AI suggestions at each stage — is a facilitation concern, not a structural one. That guidance lives in the facilitation layer, not here.

**How to facilitate seed development.** The [[project-seed|Project Seed Template]] (framework/templates/project-seed-template.md) defines the structure. How to coach someone through precedential thinking, how to diagnose a thin seed, how to probe rather than prompt — that's facilitation work, not structural specification.

**How to write good hos from the Ho Overview.** That's what the ho templates (`framework/templates/`) are for. The Ho Overview tells you _what_ to write. The template tells you _how_ to structure it.

**How to adapt the framing process for different project types.** The four-document chain was developed from software projects. Whether it applies unchanged to other domains (data analysis, research, hardware, writing) is an open question. See [[design-seed|Design Seed §6]] (framework/design-seed.md) for open questions about domain adaptation.

---

## 7. The Kanyō Evidence

The Kanyō pilot combined all four framing concerns into a single Ho-00 document (351 lines). This worked well enough for a single-learner pilot, but it had consequences:

**What combining them cost:**

- The seed (origin story, vision, landscape research) and the technical architecture were interleaved, making it hard to update one without touching the other.
- The ho sequence was embedded at the end rather than being a first-class document that could be revised independently as the project evolved.
- Tool guidance (Claude vs. Claude Code vs. Copilot) was mixed into the overview rather than being part of individual ho templates. This meant it was read once and then forgotten, rather than being reinforced at each session.
- The README wasn't written as a standalone document until well into the project, which meant the "what does done look like?" question was answered implicitly rather than explicitly.

**What was invisible:**

- The seed phase happened but was never documented. The builder's background in systems architecture and design education meant the precedential thinking — understanding the problem space, researching existing tools, defining the core idea — happened naturally, as internalized practice. The framework now makes this phase explicit so it can be taught and facilitated with learners who haven't developed that discipline yet.

**What combining them got right:**

- Everything was in one place. The learner could read one document and understand the whole project.
- The document was written with genuine enthusiasm, which matters for motivation.
- It was fast to produce — no ceremony, just write it.

The framework separates these concerns not because the combined approach failed, but because separated concerns are easier to maintain, revise, and reuse across projects. The overhead is small (four short documents instead of one long one) and the clarity gain is significant.

---

## 8. Related Framework Documents

- [[design-seed|Design Seed]] (framework/design-seed.md) — The governing document for the Ho System
- [[project-seed|Project Seed Template]] (framework/templates/project-seed-template.md) — Template for the first Kamae document
- [[shu-ha-ri|Shu-Ha-Ri Progression]] (framework/structure/shu-ha-ri.md) — How ho structure adapts to learner development
- [[shu-ho-template|Shu Ho Template]] (framework/templates/shu-ho-template.md) — Template for writing prescriptive ho sessions
- [[project-arc|Project Arc]] (framework/structure/project-arc.md) — How hos sequence into complete project arcs

---

_This document is part of the Ho System framework. It describes the structural specification for the Project Framing phase. For guidance on facilitating Project Framing with learners — including AI engagement strategies, prompting patterns, and coaching protocols — see the facilitation layer (proprietary)._
