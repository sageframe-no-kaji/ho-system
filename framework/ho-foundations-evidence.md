# The Ho System: Foundations and Evidence

**A Position Paper on Human-AI Collaborative Development**

**Author:** Andrew Todd Marcus
**Date:** February 2026
**Status:** Active — Evolving with practice

---

> _This document lays out the intellectual foundations of the Ho System — the problem it addresses, the traditions it draws from, the evidence from its first pilot, and the questions that remain open. For the structural framework, templates, and practical orientation, see [The Ho System](https://claude.ai/chat/the-ho-system.md)._

---

## Abstract

The Ho System (歩 — _ho_, "step") is a structured methodology for technical development in which a human practitioner maintains architectural authority while using AI as an implementation partner. It is designed to produce both _working systems_ and _genuine understanding_ — rejecting the false choice between "learn everything from scratch" and "let AI do it for you."

The methodology emerged from applied practice: a non-developer used this approach to build Kanyō, a production computer vision system deployed at Harvard's Memorial Hall for real-time falcon monitoring. The system was developed in approximately six weeks of structured sessions — not as a tutorial exercise but as functioning infrastructure now in active use.

This document establishes the philosophical foundations, theoretical grounding, and design principles of the Ho System. It is the _why_ behind the framework — the argument for why this approach exists, what it responds to, and what it makes possible.

---

## Part I: The Problem Space

### 1.1 The Collapse of the Middle

Technical development in the age of generative AI has bifurcated into two inadequate modes:

**Mode A: Traditional Learning Paths.** Multi-year curricula designed for sequential mastery. The learner begins with fundamentals and progresses through increasingly complex material. This path produces deep understanding but assumes the learner's primary identity is "student of this domain." It is slow, often decontextualized, and structurally inaccessible to working professionals, career-changers, and interdisciplinary thinkers whose primary expertise lies elsewhere.

**Mode B: AI-Delegated Production.** The user describes what they want; the AI produces it. This path is fast and produces outputs, but the human develops little transferable understanding. The resulting systems are fragile — the builder cannot debug, extend, or reason about what they've made.

What is missing is a middle path — one in which the human _genuinely learns_ while _actually building_, using AI not as a teacher delivering instruction and not as a contractor delivering product, but as a _collaborator_ whose implementation capacity extends the human's architectural reach.

The Ho System is that middle path.

### 1.2 The Vibe Coding Problem

Mode B has a name now. Andrej Karpathy coined "vibe coding" in early 2025 to describe an approach where the developer describes intent in natural language and accepts whatever the AI produces without fully understanding it. Collins Dictionary named it their Word of the Year for 2025. By 2026, the term has come to represent both a genuine shift in how software gets made and a growing crisis in how that software actually performs.

The evidence against unstructured AI-delegated production is now substantial and empirical, not merely theoretical:

**The code is measurably worse.** A December 2025 analysis by CodeRabbit of 470 open-source GitHub pull requests found that AI-generated code contains approximately 1.7 times more defects than human-authored code across every major quality category — logic, maintainability, security, and performance. Critical and major defects were up to 1.7 times more frequent. Performance inefficiencies appeared nearly eight times more often. Security vulnerabilities — improper password handling, insecure object references — were 1.5 to 2 times more prevalent (CodeRabbit, "State of AI vs Human Code Generation," December 2025).

**It doesn't even make experienced developers faster.** A randomized controlled trial by METR (Model Evaluation & Threat Research) tracked 16 experienced open-source developers completing 246 real tasks on repositories they had contributed to for years. The developers predicted AI tools would reduce their completion time by 24%. In reality, they were 19% _slower_ with AI than without. Most striking: even after experiencing the slowdown, participants still believed AI had sped them up by 20% (Becker et al., "Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity," arXiv:2507.09089, July 2025). The illusion of productivity may be more dangerous than the productivity loss itself.

**Security is a systemic concern.** A December 2025 assessment by Tenzai tested five leading AI coding platforms by having each build the same three applications. Across 15 applications, the tools produced 69 vulnerabilities, including several rated critical. The platforms performed well on generic, well-documented vulnerability patterns — no exploitable SQL injection or cross-site scripting — but failed on broken authorization logic, server-side request forgery, and missing security controls, precisely the issues where distinguishing safe from dangerous code required contextual judgment that vibe coding, by definition, does not develop (Tenzai, December 2025; reported in InfoWorld, January 2026).

**The ecosystem effects are structural.** A January 2026 paper by researchers at Central European University, Bielefeld University, and the Kiel Institute for the World Economy, titled "Vibe Coding Kills Open Source," argues that AI-mediated development disintermediates the user-maintainer relationship that sustains open-source projects. When AI agents select libraries and assemble applications without users reading documentation, filing issues, or engaging with communities, the feedback loops that incentivize open-source maintenance break down. The authors model a scenario in which increased coding velocity coexists with declining software quality — more code, less accountability, fewer people who understand what any of it does.

This is not an argument against AI in development. It is an argument against AI in development _without structure, without judgment, and without the human capacity to evaluate what gets produced_. Vibe coding's failure is not that it uses AI. Its failure is that it produces operators who cannot distinguish good output from bad — and, per the METR findings, cannot even accurately assess their own performance while using it.

### 1.3 The Access-Judgment Gap

The standard narrative about AI tools focuses on _access_ — who can generate code, who can build applications, who gets to participate in software development. This framing treats the problem as a distribution problem: give people better tools, and they'll build better things.

The Ho System is built on a different diagnosis. The bottleneck isn't access to AI tools. Anyone can prompt ChatGPT. The bottleneck is _judgment_ — the ability to evaluate whether what was generated is correct, secure, maintainable, and aligned with design intent. Judgment is what separates "AI generated this" from "I built this using AI."

This gap cannot be closed by better prompting. It cannot be closed by more powerful models. It can only be closed by structured practice that develops the human's capacity to direct, evaluate, and when necessary override AI-generated work. The Ho System exists to close that gap.

The Ho System's structural answer to the overconfidence problem documented in the METR and CodeRabbit research is the four-layer verification stack: automated tests, linting, directed self-review, and cross-agent verification. Verification is not theoretical hygiene — it is the mechanism that makes "architectural authority" real rather than aspirational. An architect who cannot verify the build is not an architect; they are an optimist with a deploy button. See [[verification-practices|Verification Practices]] (framework/structure/verification-practices.md) for the complete framework.

### 1.4 Who This Is For

The Ho System is designed for people who possess significant expertise in domains _adjacent to_ software development — systems thinking, design, architecture, organizational leadership, research — but are not trained developers. People who need to build real, functional systems. People who want genuine understanding of what they build, at a level sufficient to direct, evaluate, debug, and extend it. People who recognize that AI fundamentally changes what a single person can create, and want to engage that change deliberately.

The ideal Ho practitioner is an **architect** in the broad sense: someone who thinks in systems, makes structural decisions, and directs the assembly of complex artifacts. The Ho System gives that person a methodology for building technical systems that matches how they already think.

### 1.5 Why Existing Approaches Fail

**Bootcamps and tutorials** teach through contrived exercises. The learner builds a to-do app to learn React, not because they need a to-do app. Motivation is abstract. Transfer is weak. The implicit message is: _you must become a developer before you can build things._

**AI code generation (unstructured)** produces results without comprehension. The human cannot distinguish good output from bad. Errors compound silently. The implicit message is: _understanding is unnecessary; just prompt better._

**Pair programming with AI** (as currently practiced) is closer, but lacks pedagogical structure. There is no progression model, no bounded scope, no mechanism for distinguishing what the learner _needs_ to understand from what they can safely treat as opaque. Without that structure, the learner either drowns in detail or coasts on surface familiarity.

The Ho System addresses all three failures simultaneously.

---

## Part II: Theoretical Foundations

### 2.1 Constructionism and Building to Learn

The Ho System's deepest intellectual debt is to Seymour Papert's constructionism: the principle that people learn most powerfully when they are building artifacts that matter to them, in contexts where the building itself makes thinking visible. Papert distinguished this from Piaget's constructivism (learning through internal mental construction) by emphasizing the _external artifact_ as both the product and the medium of learning.

In the Ho System, the artifact is not a classroom exercise. It is a production system — software that runs, serves users, processes data. The learner's relationship to this artifact is not "student completing assignment" but "architect directing construction." This distinction is not cosmetic. It changes what the learner attends to, what questions they ask, and what kind of understanding they develop.

### 2.2 The Zone of Proximal Development, Extended

Vygotsky's zone of proximal development (ZPD) describes the space between what a learner can do independently and what they can do with assistance. Traditional scaffolding involves a more-expert human gradually withdrawing support as competence grows.

AI as implementation partner represents a _qualitative expansion_ of the ZPD. The gap between what a systems-thinking non-developer can _design_ and what they can _build alone_ is enormous. AI closes that gap — not by simplifying the design, but by handling implementation details the human has deliberately chosen not to master (yet). This allows the learner to operate at their actual level of systemic sophistication rather than being artificially constrained by implementation mechanics.

Crucially, the Ho System does not leave the ZPD unstructured. Each ho defines _which_ aspects of the work the learner should understand deeply, which they should understand functionally, and which they can treat as opaque. This is the Tiered Understanding Model — the mechanism that prevents AI-assisted development from collapsing into AI-dependent development.

### 2.3 Reflective Practice and the Practitioner's Knowledge

Donald Schön's work on reflective practice describes how professionals develop expertise not primarily through formal instruction but through _reflection-in-action_ (thinking while doing) and _reflection-on-action_ (thinking about what was done). Schön's "reflective practitioner" learns by engaging with messy, real-world problems and developing a repertoire of responses through accumulated experience.

The Ho System embeds reflective practice structurally. Each ho requires a devlog — not a summary of steps taken, but an articulation of what was understood, what remains opaque, and what surprised the learner. Confidence self-assessment forces honest evaluation rather than performative competence. The cumulative documentation trail becomes a knowledge artifact in its own right — externalized cognition that the learner (and others) can revisit and build upon.

This structural embedding matters because the research on AI-induced cognitive offloading suggests that without it, reflection simply doesn't happen. A 2025 study by Gerlich found a significant negative correlation (r = -0.68) between frequent AI tool usage and critical thinking abilities, with cognitive offloading as the primary mediating mechanism. Knowledge workers surveyed by Microsoft Research reported that generative AI made tasks feel cognitively easier — but "feeling easier" and "learning more" are not the same thing (Lee et al., "The Impact of Generative AI on Critical Thinking," CHI '25, April 2025). The Ho System's devlog practice directly counters this dynamic by requiring the learner to reconstruct and articulate their understanding after every session.

### 2.4 Cognitive Apprenticeship, Inverted

Collins, Brown, and Newman's cognitive apprenticeship model identifies key features of effective learning: modeling, coaching, scaffolding, articulation, reflection, and exploration. The Ho System maps onto this framework, with a critical substitution: AI serves as the _implementing agent_ whose work the learner must _evaluate and direct_, rather than a master whose work the learner must _imitate_.

This inversion is important. In traditional apprenticeship, the novice watches the expert and tries to reproduce expert behavior. In the Ho System, the learner _commissions_ implementation and then must understand it well enough to accept, modify, or reject it. This requires a different kind of cognition — not imitation but _architectural judgment_. The learner must develop the capacity to evaluate outputs against intentions, to identify when implementation has diverged from design, and to articulate why something is wrong even when they could not have built it themselves.

This is, notably, exactly the skill that vibe coding fails to develop and that the evidence increasingly shows is essential. The Tenzai assessment found AI tools performed adequately on generic vulnerability patterns but failed where context determined what counted as safe or dangerous. Context is not something that can be prompted into existence. It is a property of the human who directs the work.

### 2.5 Kata, Kaizen, and the Japanese Craft Tradition

The name 歩 (ho) is deliberately chosen. It means "step" in the sense of walking — deliberate, grounded, rhythmic forward motion. Not a leap. Not a sprint. A step.

This connects to several Japanese concepts that inform the system's design:

**Kata** (型) — practiced forms that encode principles. Each ho is a kata: a bounded, repeatable structure that embodies the methodology's principles regardless of content domain.

**Kaizen** (改善) — continuous incremental improvement. The ho sequence is not a curriculum to be "completed" but a practice of ongoing refinement. Each ho improves both the project and the practitioner.

**Shu-Ha-Ri** (守破離) — the three stages of mastery: follow the form, break from the form, transcend the form. Early hos are tightly structured (shu). As the learner develops fluency, hos become more open-ended, with the learner increasingly defining their own scope and objectives (ha), until the structure is internalized and the formal framework dissolves into practiced habit (ri).

The choice of a Japanese term signals that this is not simply a "coding bootcamp with AI." It is a practice — something you cultivate over time, with discipline and intention.

---

## Part III: The Education Context

### 3.1 The Skills Inversion

For most of the history of technical education, the hierarchy was clear: first learn the technical skills, then apply judgment. You learned to write code before you designed systems. You learned syntax before architecture. Knowledge preceded application.

AI inverts this hierarchy. When implementation can be generated on demand, the scarce resource is no longer the ability to write code — it is the ability to evaluate, direct, and decide. The skills that were traditionally taught last (systems thinking, architectural judgment, tradeoff analysis, reflective self-assessment) are now the skills that matter first. The skills that were traditionally taught first (syntax, language mechanics, algorithmic implementation) are increasingly delegatable.

This is not a speculative claim. The World Economic Forum's Future of Jobs Report 2025 identifies analytical thinking, creative thinking, and resilience as the most important workforce skills, with AI and big data literacy listed alongside them — not as a replacement but as a complement. The WEF's AI Literacy Framework, developed jointly with the European Commission and OECD, explicitly calls for education systems to go beyond digital literacy and prioritize critical thinking, ethical reasoning, and the ability to evaluate AI outputs. The emphasis is on _human skills that AI cannot replicate_ — empathy, judgment, collaboration, and the capacity to interrogate rather than passively consume technological outputs.

### 3.2 The Deskilling Evidence

The urgency is not theoretical. A growing body of research documents measurable cognitive decline when AI tools are used without structured engagement:

Gerlich's 2025 study of 666 UK participants found that frequent AI users exhibited significantly diminished critical thinking abilities, with cognitive offloading as the primary mechanism. Younger users (17–25) were most affected — precisely the population currently moving through educational systems (Gerlich, "AI Tools in Society: Impacts on Cognitive Offloading and the Future of Critical Thinking," _Societies_, January 2025).

A 2025 study published in _The Lancet Gastroenterology & Hepatology_ found that endoscopists who routinely used AI assistance in colonoscopies performed measurably worse when the technology was suddenly unavailable — their detection rate for precancerous lesions dropped from 28.4% to 22.4%. The skill didn't wait patiently in the background. It atrophied.

The Communications of the ACM, in a November 2025 feature titled "The AI Deskilling Paradox," synthesized findings across medicine, law, education, and software development. Law professors at Illinois Law School found that students using AI chatbots were more prone to critical errors. The article's framing captures the structural nature of the problem: deskilling is not an individual failure of discipline. It is a predictable consequence of environments that remove the need to practice.

A College Board report from 2025 found that more than 80% of AP teachers agree that AI makes students overly dependent on technology for basic tasks and less likely to develop critical thinking. Meanwhile, 69% of high school students reported using ChatGPT to help with school assignments and homework. The perception gap — students seeing AI as helpful while teachers observe declining engagement with the underlying thinking — mirrors the METR finding about developers: people consistently overestimate the quality of their AI-assisted work.

### 3.3 What Education Should Become

If AI handles implementation with increasing competence, what should education develop?

**Judgment.** The ability to evaluate outputs against intentions. To identify when something is technically correct but architecturally wrong. To distinguish "it works" from "it's good."

**Process.** The ability to decompose complex problems into manageable, sequenced work. To maintain coherence across a project arc. To know what to build next and why.

**Self-assessment.** The ability to honestly calibrate what you understand and what you don't. The Ho System's tiered understanding model — explicitly declaring what you're treating as a black box versus what you genuinely comprehend — is a direct response to the "illusion of competence" that AI-assisted work produces.

**Iteration.** The ability to build, evaluate, revise, and improve. To treat the first version as a draft, not a deliverable. To develop comfort with incompleteness.

**Collaboration with non-human systems.** Not prompt engineering — that's a surface skill with a short half-life. The deeper skill is knowing how to maintain authority over a system that can produce plausible but incorrect work at scale. Knowing when to trust, when to verify, and when to override.

These are not new educational aspirations. Critical thinking, process discipline, and reflective self-assessment appear in every educational philosophy from Dewey forward. What is new is the urgency. When AI can generate a passing essay, a working application, or a plausible diagnosis, the ability to produce those artifacts is no longer evidence of understanding. _Only the ability to evaluate them is._

The Ho System is one answer to this challenge. It doesn't teach technical skills as a prerequisite to building. It develops judgment, process, and self-assessment _through_ building — with the technical skills acquired as a necessary byproduct of doing real work, not as an end in themselves.

### 3.4 Open Knowledge, Facilitated Practice

This leads to a philosophical position about how the Ho System should exist in the world:

**The methodology should be freely accessible.** The structural framework, templates, evidence, and design principles are published openly under Creative Commons licensing. No gatekeeping. No paywalls. If the argument is that judgment matters more than access, then restricting access to the methodology would be self-contradicting.

**Mastery requires facilitation.** Reading about the Ho System isn't the same as practicing it effectively. Designing good ho sequences, calibrating difficulty to learner stage, reading devlogs diagnostically, adapting the methodology to new domains — these are facilitation skills that develop through mentorship and practice. This is where the value exchange happens: not in the knowledge, but in the cultivation of judgment.

This mirrors what the WEF describes as the shift from education-as-content-delivery to education-as-experience. As they put it: the liberal arts were never about content — they were about developing the foundational capacities for self-governance and practical reasoning. The vehicle changes; the purpose does not.

---

## Part IV: Design Principles

### 4.1 Production Over Exercise

Ho projects build real systems, not tutorial applications. The learner's motivation is authentic: they are building something they need or want. This is not incidental — it is foundational. Authentic motivation changes how the learner engages with difficulty, how they evaluate tradeoffs, and what they remember.

### 4.2 Architecture Before Implementation

Every ho begins by establishing the _systemic purpose_ of the work before any code is written. The learner understands where this piece fits in the whole before they begin constructing it. This mirrors how architects work: the plan precedes the build.

### 4.3 Bounded Ambiguity

Each ho has clear objectives and deliverables, but the path to achieving them is not fully prescribed. The learner must make decisions, encounter problems, and exercise judgment within the bounded scope. This is controlled ambiguity — enough uncertainty to require thought, not so much as to produce paralysis.

### 4.4 Honest Self-Assessment Over External Validation

The Ho System does not grade. It asks the learner to grade themselves — and provides the structure (verification questions, tiered understanding, confidence scales) to make that self-assessment meaningful. The premise is that a learner who can honestly evaluate their own comprehension is better positioned than one who performs for an external evaluator.

This principle directly addresses the perception gap documented in both the METR study (developers overestimating their AI-assisted productivity) and the College Board findings (students perceiving AI as helpful while teachers observe declining engagement). Structured self-assessment, practiced repeatedly over a project arc, is the antidote to the illusion of competence.

### 4.5 Cumulative Competence, Not Isolated Skills

Skills developed in earlier hos are exercised in later ones. Understanding compounds. This is not a collection of independent modules but a _progressive system_ where each ho depends on and extends the last. The project arc — the complete sequence from orientation to production — is the unit of learning, not the individual session.

### 4.6 Process Visibility as Transferable Output

The Ho System treats the _methodology itself_ as an artifact worth documenting, sharing, and refining. Devlogs, commit histories, and understanding reflections are not private notes — they are public-facing evidence of a way of working. This makes the Ho System self-documenting: every project arc that uses it produces evidence of how it works.

---

## Part V: Evidence — The Kanyō Pilot

### 5.1 Context

Kanyō (観鷹 — "contemplating falcons") is a computer vision system for real-time monitoring of peregrine falcons at Harvard's Memorial Hall. It was the first project developed using the Ho System, serving simultaneously as real infrastructure and as a test of the methodology.

The developer (the author) is not a software engineer by training. Background: MIT M.Arch, 25+ years in education leadership, systems design, and organizational strategy. Prior programming experience: minimal. The project was built from zero to production deployment in approximately six weeks of structured ho sessions.

### 5.2 What Was Built

A full detection pipeline using YOLOv8 with configurable confidence thresholds and frame sampling. Visit detection with debounce-based event merging. Clip extraction with hardware-accelerated encoding. Thumbnail generation with event-aware timing logic. A FastAPI backend serving real-time data. A React frontend for live observation. Professional project structure with virtual environments, dependency management, configuration systems, and logging. 55+ passing tests at 54% code coverage. Complete documentation including architecture docs, data model specs, and setup guides.

This is not a prototype. It is functioning infrastructure in active use.

### 5.3 What the Pilot Demonstrated

**The tiered understanding model works.** The developer achieved Tier 2 (functional) understanding of Python project structure, configuration management, testing frameworks, ffmpeg usage, event detection logic, and API design — while maintaining Tier 1 (deliberate black-box) treatment of YOLOv8 internals, CSS frameworks, and React build tooling. This was appropriate. The developer can modify, debug, and extend the system precisely because the methodology directed attention to the _right_ things.

**Production output from non-developers is achievable.** The quality of the codebase meets professional standards — not because the developer became a professional software engineer, but because the Ho methodology embedded those practices from session one.

**AI acceleration without AI dependency.** If AI tools were removed, the developer could maintain, debug, and extend the existing system. New feature development would be slower, but not impossible. This is the critical test: the Ho System produces people who _used_ AI, not people who _depend on_ AI.

**The devlogs are genuinely valuable.** Months after completion, the devlogs serve as functional reference material — not "what I did" summaries but operational documentation of how the system works, what matters, and where the gotchas live.

**Branching reveals real learning.** The planned arc of 11 sequential hos became 19 documents across three levels of branching. The gap between plan and reality is itself diagnostic: where branches concentrate reveals where real complexity lived, where stages shifted reveals where the developer grew, and what was deferred reveals what the project didn't need.

### 5.4 What Was Learned About the Methodology

The Kanyō pilot also revealed areas where the methodology needed refinement, which directly informed the framework's current structure:

Early hos were too long and too tightly prescribed. The ~2 hour target held, but the number of internal parts needed to be reduced. Structure should bound the session, not fill it.

The shu-to-ha transition was more gradual and domain-specific than initially modeled. The developer was simultaneously at different stages for different technologies — shu for Docker while at ha for Python — which led to the multi-domain stage model now documented in the framework.

Reflection quality varied dramatically. Early devlogs were performative summaries. Later devlogs were genuine decision records. The devlog template evolved accordingly, with different structures for different stages.

---

## Part VI: Open Questions

### 6.1 Generalizability

Can the Ho structure transfer to non-software domains? The underlying principles — bounded sessions, tiered understanding, reflective documentation, human-AI collaboration — are domain-agnostic. But the specific structure (commit histories, test suites, deployment verification) is software-native. What does a ho look like for writing, research, organizational design, or physical fabrication?

### 6.2 Multi-Learner Contexts

The pilot involved a single learner. How does the Ho System work with groups? Does the architectural role remain with one person, or can it be distributed? How do devlogs function in collaborative settings?

### 6.3 Assessment and Credentialing

The system relies on honest self-assessment. Is there a role for external evaluation? Could a portfolio of devlogs, working systems, and understanding reflections serve as a credible alternative to traditional credentials? The evidence suggests it might be more reliable — devlogs cannot be generated by AI in the way that code can.

### 6.4 AI Model Evolution

The Ho System is designed to be model-agnostic, but in practice the pilot used Claude. As AI capabilities change, how should the methodology adapt? If AI becomes better at architectural reasoning, does the human role shift? The Ho System's position is that architectural authority must remain human — not because AI can't reason architecturally, but because the human must develop the judgment to evaluate whether the architecture is right. The methodology develops the human, not just the system.

### 6.5 Relationship to Formal Education

Where does the Ho System sit relative to universities, bootcamps, and professional development? The skills inversion described in Part III suggests it may be more complementary than alternative — the Ho System develops precisely the judgment, process, and self-assessment skills that formal education increasingly recognizes as essential but struggles to teach through traditional means.

---

## References

Becker, J. et al. (2025). "Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity." arXiv:2507.09089. July 2025.

Budzyń, K. et al. (2025). "Endoscopist deskilling risk after exposure to artificial intelligence in colonoscopy: a multicentre, observational study." _The Lancet Gastroenterology & Hepatology_, 10(10), 896–903. August 2025.

CodeRabbit. (2025). "State of AI vs Human Code Generation Report." December 2025.

Collins, A., Brown, J.S., & Newman, S.E. (1989). "Cognitive Apprenticeship: Teaching the Crafts of Reading, Writing, and Mathematics." In L.B. Resnick (Ed.), _Knowing, Learning, and Instruction_.

The College Board. (2025). AI in Education Report. October 2025.

Gerlich, M. (2025). "AI Tools in Society: Impacts on Cognitive Offloading and the Future of Critical Thinking." _Societies_, 15(1). January 2025.

Greengard, S. (2025). "The AI Deskilling Paradox." _Communications of the ACM_. November 2025.

Karpathy, A. (2025). Introduction of the term "vibe coding." February 2025.

Koren, M., Békés, G., Hinz, J., & Lohmann, A. (2026). "Vibe Coding Kills Open Source." CEPR Discussion Paper No. 21145. arXiv:2601.15494. January 2026.

Lee, H. et al. (2025). "The Impact of Generative AI on Critical Thinking: Self-Reported Reductions in Cognitive Effort and Confidence Effects From a Survey of Knowledge Workers." CHI '25, April 2025. Yokohama, Japan.

Papert, S. (1991). _Situating Constructionism_. In I. Harel & S. Papert (Eds.), _Constructionism_.

Schön, D. (1983). _The Reflective Practitioner: How Professionals Think in Action_. Basic Books.

Tenzai. (2025). Security assessment of AI coding platforms. December 2025.

Vygotsky, L.S. (1978). _Mind in Society: The Development of Higher Psychological Processes_. Harvard University Press.

World Economic Forum. (2025). _Future of Jobs Report 2025_.

World Economic Forum & European Commission/OECD. (2025). AI Literacy Framework (AILit). Draft released for consultation, 2025.

---

_This is a living document. It will evolve as the Ho System is applied to new projects, tested with new learners, and refined through practice. The framework documents contain the structural specification; this paper contains the argument for why it exists._

_— ATM, February 2026_

# License

© 2026 Andrew Todd Marcus

This work is licensed under the Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License.

You are free to:

- Share — copy and redistribute the material in any medium or format

Under the following terms:

- Attribution — You must give appropriate credit, provide a link to the license, and indicate if changes were made
- NonCommercial — You may not use the material for commercial purposes
- NoDerivatives — If you remix, transform, or build upon the material, you may not distribute the modified material

For consulting, training, or commercial use inquiries, contact: andrew@blueprintforcreativity.com

Full license: https://creativecommons.org/licenses/by-nc-nd/4.0/

---

# License

© 2026 Andrew Todd Marcus

This work is licensed under the Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License.

You are free to:

- Share — copy and redistribute the material in any medium or format

Under the following terms:

- Attribution — You must give appropriate credit, provide a link to the license, and indicate if changes were made
- NonCommercial — You may not use the material for commercial purposes
- NoDerivatives — If you remix, transform, or build upon the material, you may not distribute the modified material

For consulting, training, or commercial use inquiries, contact: andrew@blueprintforcreativity.com

Full license: https://creativecommons.org/licenses/by-nc-nd/4.0/
