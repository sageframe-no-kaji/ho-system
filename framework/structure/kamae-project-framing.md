# Kamae (構え): Project Framing

## The Pre-Ho Phase: From Idea to Buildable Plan

---

## 1. What Kamae Is

In martial arts, **kamae (構え)** is the ready stance — how you position yourself before engaging. You're not fighting yet, but you're oriented correctly. Feet set, weight balanced, awareness open. You don't rush in; you set your stance first.

In the Ho System, kamae is everything that happens before the first ho. It takes a raw idea and produces a structured plan that hos can be written from. Without it, the first ho is guessing at scope, the second ho discovers the architecture was wrong, and the third ho gets thrown out entirely.

The Kanyō pilot did this implicitly — the Ho-00 overview document was a single artifact that combined brainstorming, architecture, tool selection, and ho sequencing into one 350-line document. It worked, but it conflated concerns that serve different purposes and that change at different rates. Separating them makes the process repeatable and the outputs more useful.

Project Framing produces four documents, in order. Each builds on the previous one and feeds into the next. The chain is:

```
Concept → System Design → README → Ho Overview
```

When the Ho Overview is complete, you have enough to write individual hos from the [[shu-ho-template|shu-stage template]](framework/templates/shu-ho-template.md) or whichever template is appropriate for the learner's level.

---

## 2. The Four Documents

### 2.1 Concept

**What it is:** The raw idea. Brainstorming output. What's exciting, what problem it solves, who it's for, why you care. Written for yourself.

**What it's NOT:** A spec. A commitment. A polished pitch. The concept document is *thinking out loud on paper*. Messy is fine. Enthusiasm is fine. "This could be huge" energy is fine. That energy is fuel — it gets refined later, not suppressed now.

**What "done" looks like:**

Someone reading this document should understand:

- What the project does (in plain language, not technical spec)
- What problem it solves and for whom
- Why it's worth building
- Rough scope — what's in, what's out, what's "maybe later"
- Any constraints (hardware, budget, time, skills)

**What "done" does NOT look like:**

- Technology choices (those come in System Design)
- File structures or code architecture (too early)
- A polished narrative (this is working material)

**Relationship to what comes next:** The concept is the input to the System Design. It answers "what and why." The System Design answers "how."

**Example from Kanyō:** The origin story (conversation with Claudia Goldin on a flight), the vision (automated falcon timeline), the motivating enthusiasm. In the Kanyō pilot this was embedded in Ho-00 rather than being a separate document.

**Example from Hōzō:** The concept document that described the pain point (home-lab users want off-site ZFS backups without a second NAS), the idea (wake-on-demand orchestrator), the potential audience, and the excitement about the project.

---

### 2.2 System Design

**What it is:** The structured technical vision. How the system works, what the major components are, how they connect, what technology choices anchor the architecture. Written for someone who needs to understand *what you're building* at a systems level.

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

**Relationship to what comes next:** The README, along with the Concept and System Design, is fed to AI to generate the Ho Overview. It provides the concrete scope definition that constrains what the ho sequence needs to accomplish.

**Example from Kanyō:** The repo structure, success criteria (minimum viable and stretch goals), and technology stack sections of Ho-00 served this purpose. A standalone README was not written until later.

**Example from Hōzō:** The README.md included in the Ho-00 document — installation instructions, quick start with config example, requirements for controller and remote box, development setup.

---

### 2.4 Ho Overview

**What it is:** The sequence plan. What hos are needed, in what order, with what dependencies. This is the project arc made concrete — it turns the System Design and README scope into a buildable sequence of bounded sessions.

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

The four documents form a refinement chain. Each one takes the previous document's output and adds structure:

```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│  CONCEPT           "What if we built...?"                    │
│  (raw idea)         → Problem, audience, excitement, scope   │
│                                                              │
│         ↓ refines into                                       │
│                                                              │
│  SYSTEM DESIGN     "Here's how it would work."               │
│  (architecture)     → Components, data flow, tech stack      │
│                                                              │
│         ↓ refines into                                       │
│                                                              │
│  README            "Here's what it does and how to use it."  │
│  (polished scope)   → Install, usage, requirements           │
│                                                              │
│         ↓ refines into                                       │
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

Each step narrows and structures. The concept is broad and excited. The system design is structured but still high-level. The README is polished and scope-bounded. The Ho Overview is sequenced and dependency-mapped. The individual hos are prescriptive and session-ready.

**The chain is not always linear.** Writing the System Design may reveal that the concept's scope is too broad. Writing the README may expose a gap in the System Design. Writing the Ho Overview may reveal that a component needs to be split across multiple hos that weren't obvious from the System Design alone. Expect to loop back and revise earlier documents as later ones surface problems.

---

## 4. When to Do What

### For a Brand-New Project

Write all four documents in order. Don't skip steps. The concept might take 30 minutes. The System Design might take an hour. The README might take 30 minutes. The Ho Overview might take an hour. Total investment: 2–3 hours before writing any hos.

This is not wasted time. It's the cheapest place to discover that your scope is wrong, your architecture doesn't work, or your project needs to be split into two projects. Fixing these problems in the framing phase costs hours. Fixing them in Ho 6 costs weeks.

### For a Project That Already Has Momentum

If you've already started building (as happened with Kanyō), you can write the framing documents retroactively. This is less about planning and more about *documenting decisions that were already made* — which still has value, because it surfaces implicit assumptions and makes the arc visible.

### For a Follow-Up Project Using Existing Patterns

If you've completed one project with the Ho System and are starting a second (as with Hōzō following Kanyō), the framing phase is faster. You already know the methodology, the templates, and the rhythm. The concept and system design are where the new thinking happens. The README and Ho Overview follow established patterns.

---

## 5. Relationship to the Project Arc

Project Framing produces the *plan*. The Project Arc is the *execution*. The Ho Overview is the bridge between them — it's the last framing document and the first arc document.

The project arc phases (from the [[design-seed|Design Seed]](framework/design-seed.md) §3.2):

| Phase | Hos | Focus |
|---|---|---|
| **Orientation** | 0–0.5 | Tooling, environment, mental model formation |
| **Foundation** | 1–2 | Project structure, core infrastructure, first working component |
| **Construction** | 3–6 | Primary system capabilities, iterative build-out |
| **Integration** | 7–9 | Assembly, deployment, system-level concerns |
| **Polish & Launch** | 10+ | Refinement, documentation, public deployment |

The Ho Overview assigns each planned ho to a phase. Individual hos may shift between phases as the project develops — that's expected. The arc is a guide, not a contract.

---

## 6. What This Document Does NOT Cover

**How to engage AI at each stage.** The framing documents are natural places for AI collaboration (brainstorming with a concept, evaluating architecture decisions, generating a ho sequence from a system design). But the *how* — what prompts to use, which AI mode (thinking vs. agent) is appropriate, how to evaluate AI suggestions at each stage — is a facilitation concern, not a structural one. That guidance lives in the facilitation layer, not here.

**How to write good hos from the Ho Overview.** That's what the ho templates (`framework/templates/`) are for. The Ho Overview tells you *what* to write. The template tells you *how* to structure it.

**How to adapt the framing process for different project types.** The four-document chain was developed from software projects. Whether it applies unchanged to other domains (data analysis, research, hardware, writing) is an open question. See [[design-seed|Design Seed §6]](framework/design-seed.md) for open questions about domain adaptation.

---

## 7. The Kanyō Evidence

The Kanyō pilot combined all four framing concerns into a single Ho-00 document (351 lines). This worked well enough for a single-learner pilot, but it had consequences:

**What combining them cost:**
- The concept (origin story, vision) and the technical architecture were interleaved, making it hard to update one without touching the other.
- The ho sequence was embedded at the end rather than being a first-class document that could be revised independently as the project evolved.
- Tool guidance (Claude vs. Claude Code vs. Copilot) was mixed into the overview rather than being part of individual ho templates. This meant it was read once and then forgotten, rather than being reinforced at each session.
- The README wasn't written as a standalone document until well into the project, which meant the "what does done look like?" question was answered implicitly rather than explicitly.

**What combining them got right:**
- Everything was in one place. The learner could read one document and understand the whole project.
- The document was written with genuine enthusiasm, which matters for motivation.
- It was fast to produce — no ceremony, just write it.

The framework separates these concerns not because the combined approach failed, but because separated concerns are easier to maintain, revise, and reuse across projects. The overhead is small (four short documents instead of one long one) and the clarity gain is significant.

---

## 8. Related Framework Documents

- [[design-seed|Design Seed]](framework/design-seed.md) — The governing document for the Ho System
- [[shu-ha-ri|Shu-Ha-Ri Progression]](framework/structure/shu-ha-ri.md) — How ho structure adapts to learner development
- [[shu-ho-template|Shu Ho Template]](framework/templates/shu-ho-template.md) — Template for writing prescriptive ho sessions
- [[project-arc|Project Arc]](framework/structure/project-arc.md) — How hos sequence into complete project arcs

---

*This document is part of the Ho System framework. It describes the structural specification for the Project Framing phase. For guidance on facilitating Project Framing with learners — including AI engagement strategies, prompting patterns, and coaching protocols — see the facilitation layer (proprietary).*
