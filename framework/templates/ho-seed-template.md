---

## created: 2026-03-04T16:35 updated: 2026-03-05 status: draft-v2

# The Project Seed

## The First Kamae Document

---
---

## created: 2026-03-04T16:35 updated: 2026-03-05 status: draft-v2

# The Project Seed

## The First Kamae Document

---

## What This Is

The seed is the first document in the Kamae project framing chain:

```
Seed → System Design → README → Ho Overview
```

Each document in this chain serves a different function, and the order matters.

The seed establishes the core idea — the philosophical foundation of the project. What's the problem, why does it matter, what's been tried, what should exist. Everything that follows is evaluated against this foundation. The System Design makes architectural decisions that serve the seed's intent. The README defines concrete scope that implements those architectural decisions. The Ho Overview can sequence the build precisely _because_ the System Design and README have already made the structural decisions.

The seed has an opinion about architecture, but it is not the System Design. It sets the philosophical ground for the README, but it is not the README. It is the core set of statements against which all downstream decisions are judged. When something isn't working three hos in, you go back to the seed and ask: is the core idea still right? If yes, the problem is downstream — fix the implementation. If no, change the seed. Deliberately, with eyes open. No sunk cost fallacy. A core idea that has been honestly revised is stronger than one that's been silently outgrown.

A seed is not the beginning of the work. It is the first visible output of work that already happened. The research, the reflection, the honest examination of the problem space — that upstream thinking is what makes a seed worth reading. A thin seed usually means the problem hasn't been lived with long enough. A rich seed means someone did the real work before the visible work began.

---

## Where Seeds Come From

Before there is research, before there is precedential thinking, there is friction. A problem you hit. A tool that doesn't exist. An external pressure. A curiosity that won't let go. Something that isn't working, and you can't stop noticing.

That friction is the origin. Not the seed — the seed comes later — but the thing that makes a seed worth writing. The most boring architectural site is a flat field: no slope, no neighbors, no trees, no constraints. There is nothing to push against, and without something to push against, there is nothing to design. Every interesting idea comes from friction between intent and reality — between what you want to do and what the world currently allows.

Some friction is personal: a workflow that wastes your time, a need nobody has built for. Some is external: a new technology that creates possibilities that didn't exist before, a change in context that breaks existing solutions. Some is intellectual: a question you keep coming back to, a gap in how a field thinks about itself. The source doesn't matter. What matters is that the friction is real — that it comes from genuine encounter with a problem, not from a desire to build something for its own sake.

What follows the friction — the research, the landscape scanning, the precedential thinking — is how you take that raw encounter and develop it into something buildable. Without the friction, there's nothing to develop. Without the development, there's nothing to build.

---

## Precedential Thinking

The quality of a seed depends on the quality of the thinking that preceded it. That thinking has a name: **precedential thinking** — the discipline of understanding what exists in a problem space before proposing to build in it.

This means: before you write the seed, you should have already engaged with the territory. What tools exist? What approaches have been tried? What constraints do other solutions operate under? Where are the real gaps — not the gaps you imagine, but the ones you've confirmed through research? The person who has done this work produces a seed grounded in reality. The person who hasn't produces a seed grounded in assumptions.

Precedential thinking is not a literature review. It's honest investigation — looking at what's been built, what's been tried, what's failed, and why. It can involve conversations with AI, web research, trying existing tools, reading documentation, talking to people who have the problem. The medium doesn't matter. What matters is that by the time you sit down to write the seed, you know where you stand.

The full intellectual case for precedential thinking — its origins in studio-based design, its relationship to AI collaboration, and why it matters now — is in [Precedential Thinking](https://sageframe.substack.com/p/precedential-thinking) on the Constructive Interference Substack.

---

## How to Use This Template

This is a thinking tool, not a form. The sections below identify the ground you should cover, not the order you should cover it in and not fields to fill in.

Brainstorming is not linear. You might start with architecture and work backward to motivation. You might start with a name and discover the problem while explaining it. You might start with "I want to learn X" and the project crystallizes around that. Write however you think. Then use the sections as a checklist: what did I cover? What's still implicit? What's missing entirely?

**Working with a collaborator — human or AI — the seed should be developed through probing, not prompting.** The right question is not "you didn't fill in the Audience section." The right question is "who actually has this problem?" A good collaborator pushes on the thinking, asks the questions you haven't asked yourself, and surfaces the assumptions you're making without realizing it. The template gives the collaborator a map of the territory that should be explored; the exploration itself is a conversation, not a form completion exercise.

Not every section will be relevant to every project. Skip what doesn't apply — but make that a conscious choice, not an oversight.

---

## The Sections

### 1. The Problem

What isn't working? What's missing? What's painful, inefficient, or impossible right now?

State the problem in terms of human experience, not technical gaps. "The problem is that there's no tool that does X" is a solution wearing a problem mask. The problem is the thing that's hard or broken or missing. The tool is the response to it.

You should be able to articulate not just that the problem exists, but _why it persists_. What have others tried? Why hasn't it been solved? Is it a technology limitation, a market gap, a design failure, or a problem nobody has framed correctly? Understanding why the problem exists — not just that it does — is what separates a reaction from a diagnosis.

Someone who has never thought about this domain should be able to read this section and understand why you care.

---

### 2. The Landscape

What already exists in this space? What have you looked at, tried, or dismissed — and why?

This section comes before the vision deliberately. Your vision should be informed by what exists, not invented in isolation and then retroactively justified against the landscape. The discipline is: define the problem, research the territory, _then_ articulate what you're going to do about it.

Name the things that exist. Explain what they get right and where they fail. If something comes close but misses on one critical dimension — it's not open source, it doesn't handle your specific use case, it requires infrastructure you don't have — say so. If nothing exists, explain why you think that is.

Two failure modes to watch: skipping this section entirely (assuming your idea is novel without checking), and dismissing existing solutions too quickly ("I looked at X and it doesn't work" without explaining _why_ or _how you evaluated it_).

This is where the seed earns its credibility. A builder who can articulate what exists, what's been tried, what constraints others operate under, and where the genuine gap lives — that's someone whose design will stand on solid ground. It's the same discipline as checking load-bearing walls before you start renovating.

---

### 3. The Vision

What does this project do? Not how it works — what it accomplishes. If it existed and worked perfectly, what would be different?

State it in plain language. "It lets me search my entire conversation history in natural language" is a vision. "It uses ChromaDB with semantic embeddings" is architecture. The vision describes the core value, not the implementation and not every feature you can imagine. If this section is three pages long, it probably contains system design that belongs elsewhere.

---

### 4. Audience

Who is this for?

Enough specificity that design decisions can be evaluated against a real person's needs. "Teenagers who love medical dramas and want to learn clinical reasoning" is useful. "Users" is not.

If the audience is "me," say so explicitly. Personal tools are legitimate. But even for personal tools, defining your own needs precisely helps scope decisions. Watch for audience descriptions that are actually value propositions — "people who want a better way to X" describes what the product does, not who uses it.

---

### 5. Identity

What is this project called? Why?

This section is optional for many projects. A working name and a sentence about why is enough. A placeholder with "I'll figure this out later" is also fine — just flag it. But if naming matters to you — if the name carries meaning, signals lineage, communicates values — capture that reasoning here. It's easy to forget later.

---

### 6. Project Nature and Intent

What kind of project is this? What is its relationship to the world?

Open source or proprietary? Personal tool or shared infrastructure? Learning vehicle or production system? Product or proof of concept?

State the intent consciously. This shapes downstream decisions more than people expect. Open source projects need different documentation standards, dependency choices, and deployment models than proprietary ones. A learning project tolerates different technology choices than a production system. A tool you'll share needs different error handling than one only you will use. Stating the nature early prevents architecture decisions that fight the intent later.

---

### 7. Architecture Direction

How might this thing work? What are the major components, how do they relate, and what technologies are you considering?

This is thinking out loud about _how_ — not a specification, but the first pass at the system's shape. Consider:

- **Data layer** — What needs to be stored? How? What's the shape of the core data?
- **Interface layer** — How do people interact with it? Web? CLI? API? Desktop app?
- **Processing/logic layer** — What does the system actually do with its data?
- **Deployment model** — Where does it run? Local? Cloud? Docker? Someone else's infrastructure?
- **Integration points** — What external systems does it talk to? APIs, databases, services, hardware?
- **Key technology choices** — What languages, frameworks, databases, or tools are you drawn to, and why?

Depth should match what you actually have in your head. A systems architect may produce a full multi-layer architecture on first pass. Someone earlier in their journey may have "I think I need a database and a web frontend." Both are valid seeds. The template shouldn't create pressure to invent architectural thinking you don't yet have.

Mark provisional decisions as provisional. Architecture that reads as settled in the seed will be treated as settled by whoever plans the project. If a technology choice is a guess, say so.

Technology choices should be informed by what you learned in the Landscape. If existing tools fail because they're cloud-dependent, that's a reason to choose local-first architecture. If existing approaches struggle with scale, that shapes your data layer. Architecture informed by precedent research solves the actual problem, not the problem you imagined before you looked.

---

### 8. Constraints

What does reality impose on this project?

Constraints are different from scope boundaries. Scope is what you choose to exclude. Constraints are what reality excludes for you — hardware limitations, budget, time, available skills, infrastructure you do or don't have access to, regulatory requirements.

Name them. A project that needs to run on a Raspberry Pi lives in a different architectural universe than one with access to cloud GPUs. A project with two weeks lives in a different universe than one with six months. These aren't embarrassing limitations to minimize — they're design parameters. Some of the best architectural decisions come from constraints, not despite them.

---

### 9. Scope Boundaries

What is this project? What is it not? What's in the first version, and what's explicitly deferred?

At least one clear "this is NOT" statement, and a sense of where the MVP line falls — the smallest version that delivers the core value.

The future vision is welcome, but it should be explicitly separated from the initial scope. "The MVP does X. Future versions could add Y and Z" is clearer than a features list that doesn't distinguish now from later.

---

### 10. Success Criteria

How do you know this project worked?

Observable, evaluable outcomes. Not features — results. "A natural language question returns a relevant answer from conversation history in under three seconds" is testable. "It works well" is not. "The system supports authentication" is a feature. "A new user can create an account and start working in under two minutes" is a success criterion. The difference: criteria describe outcomes from the user's perspective; features describe capabilities from the builder's.

At least three concrete statements that someone other than the builder could verify.

---

### 11. Where I'm Starting From

What do I already know? What's genuinely new territory?

Honest self-assessment of current capabilities relative to what this project requires. This is distinct from what you want to learn (that's next) — this is where you actually are.

In the Ho System, this drives shu-ha-ri assignment: the things you already know well will be ri-stage work in the project arc; the things that are completely new will be shu. The gap between this section and the next is where the learning lives, and it directly shapes how hos get scoped, paced, and structured.

Be specific. "I know Python" is less useful than "I've written scripts and small CLI tools but never built an API or managed a database." The planning process can only calibrate to what you honestly declare.

---

### 12. What I Want to Learn

What skills, technologies, or practices does this project develop in you?

This is distinct from the project's purpose. The project might serve its users perfectly while you learn nothing new, or the project might be primarily a vehicle for learning. Either is fine, but which one it is changes everything about how it should be planned.

When present, this section prevents a common failure mode: optimizing the plan for speed when the builder's actual goal is depth. If you want to understand database design, a plan that auto-generates the schema defeats the purpose — even if it's faster.

---

### 13. Open Questions

What don't you know yet? What decisions are you deferring? What assumptions might be wrong?

This is the most important section for downstream planning. Open questions are where the real work lives. A seed with thorough open questions makes the transition to System Design fast because the hard thinking has been surfaced. A seed with no open questions forces the planning process to do the excavation — and the questions it finds may surprise you.

If the seed has no open questions, either the project is trivially simple or you haven't thought hard enough about what could go wrong.

---

## After the Seed

The seed is done when you've said what's in your head. It doesn't need to be polished, complete, or balanced. Some sections will be dense. Some will be a sentence. Some will be empty with a note that says "haven't thought about this yet."

That asymmetry is information. It tells whoever is planning the project — you, a collaborator, an AI — where the thinking is rich and where it needs work.

Seeds should not all look the same. A seed for a personal knowledge tool will be heavy on problem definition and architecture, light on audience. A seed for an educational game will be heavy on audience and content boundaries, light on deployment. A seed for a learning-vehicle project will be heavy on "What I Want to Learn," which reshapes every other section. The template provides coverage; the project provides emphasis. If every seed you write has the same shape and weight distribution, you're writing to the template instead of writing about the project — and the template has failed.

### How the Seed Feeds the Chain

The seed's sections don't map one-to-one to the remaining Kamae documents. They map many-to-many. But the relationship between the documents is not "increasing detail." It is _increasing commitment_.

The **System Design** takes the seed's opinions about architecture and makes them into decisions. The seed says "I think I need a database and a web frontend, and here's why." The System Design says "PostgreSQL, FastAPI, here's the data model, here's how the components connect." The seed's core idea — the problem, the vision, the audience — becomes the evaluative frame against which architectural decisions are justified.

The **README** takes the System Design's decisions and makes them into concrete scope. What does this thing do? How do you install it? How do you use it? The README changes as the project develops — details sharpen, features are added or cut — but its philosophical stem comes from the seed. The seed says what the project is _for_. The README says what the project _is_.

The **Ho Overview** can sequence the build precisely because the System Design and README have already made the hard decisions. You can't plan a build order for a system whose architecture is undecided. You can't scope sessions for a project whose boundaries are undefined. The Ho Overview is possible because the upstream documents did their work.

### The Seed Doesn't Get Filed Away

The seed stays in the project not as a historical curiosity but as an active reference. It is the document you return to when decisions start feeling arbitrary, when scope keeps creeping, when something isn't working and you can't figure out why.

The question is always: does this decision serve the core idea? If the answer keeps being no, the problem might not be the decision. It might be the core idea. And if the core idea needs to change — because you learned something, because the landscape shifted, because you were wrong — you change it. In the seed. Explicitly. With a note about what changed and why. The revised seed then becomes the new evaluative foundation for everything downstream.

This is a design discipline, not a software practice. In architecture, the _parti_ — the core organizational idea of a building — serves exactly this function. Every design decision is evaluated against the parti. When the parti changes, everything reorganizes around the new idea. The seed is the parti of the project.

---

## Quick Reference

1. **The Problem** — What's broken or missing? Why does it persist?
2. **The Landscape** — What exists? What's been tried? Where's the real gap?
3. **The Vision** — What does this project accomplish?
4. **Audience** — Who is this for?
5. **Identity** — What's it called and why?
6. **Project Nature** — Open source? Personal tool? Learning vehicle?
7. **Architecture Direction** — Data, interface, logic, deployment, integrations, tech choices?
8. **Constraints** — What does reality impose? Hardware, budget, time, skills?
9. **Scope Boundaries** — What is it / what is it not / where's the MVP line?
10. **Success Criteria** — How do you know it worked?
11. **Where I'm Starting From** — What do I already know? What's new territory?
12. **What I Want to Learn** — What does building this teach me?
13. **Open Questions** — What don't I know yet?

Write in whatever order makes sense. Use the sections to check coverage. Skip what genuinely doesn't apply — but note that you skipped it.

---

_The Project Seed is the first document in the Kamae project framing chain (Seed → System Design → README → Ho Overview). It establishes the core idea of the project — the evaluative foundation against which all downstream decisions are judged. The remaining Kamae documents make that foundation into architecture, scope, and a buildable sequence._

---

_This document is part of the Ho System framework. It defines the structural specification for the Project Seed. For guidance on facilitating seed development with learners — including how to coach precedential thinking, diagnose thin seeds, and guide honest self-assessment — see the facilitation layer (proprietary)._