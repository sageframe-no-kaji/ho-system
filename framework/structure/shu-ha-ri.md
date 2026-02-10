# Shu-Ha-Ri: The Progression Model

## How the Ho System Adapts to Learner Development

---

## 1. Introduction

### 守破離 — Follow, Break, Transcend

Shu-Ha-Ri (守破離) is a Japanese concept describing the three stages of mastery in any disciplined practice. It originates in martial arts and tea ceremony, but its logic applies wherever structured learning gives way to fluent action:

- **Shu (守)** — *Protect/Follow.* Learn by adhering to the form. The form is not optional; it encodes principles the learner cannot yet see.
- **Ha (破)** — *Break/Detach.* Depart from the form deliberately. The learner understands the principles well enough to adapt, challenge, and reshape the structure to fit the situation.
- **Ri (離)** — *Transcend/Leave.* The form has been internalized. Practice flows without conscious reference to structure. The principles live in the practitioner, not the template.

### Why This Matters for the Ho System

The Ho System uses bounded, structured work sessions (hos) as the fundamental unit of human-AI collaborative development. But a ho designed for someone's first day looks nothing like a ho designed for someone maintaining a production system. The structure must breathe.

This is not a weakness to be managed — it is a core feature of the methodology. The ho structure is *scaffolding*, and good scaffolding is designed to be removed. A framework that stays rigid as the learner develops is no longer serving the learner; it is serving itself.

The Kanyō pilot demonstrated this progression empirically. Early hos (01–03) were tightly prescriptive and worked well as learning instruments. Later work (Ho 05.6, the architecture redesign) broke from the template because the learner's needs had changed. Eventually, operational tasks (debugging, tuning, infrastructure setup) took a minimal problem/solution format with no instructional scaffolding at all. The 2-hour boundary breaking wasn't failure — it was evidence of growth.

This document formalizes that progression so it can be recognized, planned for, and designed around.

---

## 2. The Three Stages

### 2.1 Shu Stage — Follow the Form

> *"Trust the structure. It knows things you don't yet."*

#### Learner Characteristics

The shu-stage learner is new to the domain. They may bring substantial expertise from adjacent fields — systems thinking, design, leadership, research — but they lack the specific technical vocabulary, tool fluency, and pattern recognition that comes from hands-on practice in this domain.

Shu-stage learners:

- Cannot yet distinguish important details from noise in technical output
- Need external structure to prevent scope creep and overwhelm
- Benefit from explicit prerequisite checks (they don't yet know what they don't know)
- Require verification questions to surface hidden misunderstandings
- Are developing their relationship with AI as implementation partner — learning when to trust, when to question, when to push back

#### Ho Characteristics

Shu-stage hos are the most prescriptive form in the system. They follow the full ho template with all structural elements enforced:

- **Duration:** ~2 hours, strictly bounded. The boundary is protective, not arbitrary — it prevents the learner from disappearing into a rabbit hole before they have the navigational skills to find their way back.
- **Parts:** 4–9 sequenced parts, each 10–25 minutes, with explicit instructions for what to do and what to verify before moving on.
- **Prerequisites:** Fully specified. The learner checks each item before beginning.
- **Verification Questions:** Required at the end. These are not quizzes — they test whether the learner can *explain* what they built, not merely confirm that it works.
- **Devlog:** Full reflection including confidence self-assessment (1–5 scale), "what surprised me," and honest identification of what remains opaque.
- **Commit discipline:** Commit after each part. This builds the habit of clean version control and creates recoverable checkpoints.

#### AI Role

In shu stage, AI functions as an *implementation partner with heavy human verification*. The learner commissions work, reviews it, questions it, and accepts or rejects it. The ho structure provides the scaffolding for this review process — verification questions, devlog reflections, and tiered understanding checks all force the learner to engage with the AI's output critically rather than passively.

The facilitator (or the ho author) has done the architectural thinking in advance. The learner is following a path someone else designed, but doing the walking themselves.

#### Kanyō Evidence: Ho 01 — Git Good

[[ho-01-git-good|Ho 01]](examples/kanyo-pilot/ho-01-git-good.md) is a textbook shu-stage ho. It establishes professional project infrastructure (virtual environments, linting, testing, documentation) through step-by-step instruction:

- Each part has explicit commands to run and expected outputs to verify
- The ho explains *why* each tool matters ("Without venv: dependency hell")
- The learner is told what to do and given enough context to understand the rationale
- Duration target: 2 hours
- Prerequisite: Ho 0.5 completed, specific tools verified as installed

The learner does not design the project structure — the ho does. The learner's job is to execute, verify, and begin building the mental model that will later support independent decision-making.

#### When to Use Shu-Stage Hos

- First 3–5 hos of a beginner's first project
- When introducing a fundamentally new domain or toolset (even for experienced learners)
- When the stakes of getting foundational decisions wrong are high (project structure, deployment patterns, security configurations)
- When the learner cannot yet articulate what they don't understand

---

### 2.2 Ha Stage — Break from the Form

> *"You know enough to see where the form doesn't fit. Now reshape it."*

#### Learner Characteristics

The ha-stage learner has context. They understand the project's architecture, can read code with reasonable comprehension, and have begun developing their own opinions about how things should work. Critically, they have started *questioning the AI's suggestions* — not from insecurity, but from emerging judgment.

Ha-stage learners:

- Can identify tradeoffs between competing approaches and articulate why one is better for their situation
- Are making architectural decisions, not just executing prescribed ones
- Notice when something feels wrong in the codebase before they can name the specific problem
- Have internalized basic tool fluency (git, testing, linting) and no longer need step-by-step reminders
- Are developing the capacity to scope their own work — they know what a reasonable session looks like

#### Ho Characteristics

Ha-stage hos relax the prescriptive structure while retaining the elements that continue to serve the learner:

- **Duration:** 2–4 hours, treated as a guideline rather than a strict boundary. The learner has enough self-awareness to recognize when they're productive versus when they're spiraling.
- **Parts:** Self-defined or loosely structured. The ho may specify a problem and an approach, but the learner determines the decomposition into working steps.
- **Prerequisites:** Lighter — the learner can identify their own readiness.
- **Verification Questions:** Still valuable but may shift from "can you explain X?" to "what alternatives did you consider and why did you choose this approach?"
- **Devlog:** Remains required, but shifts in character. Less "what I learned" and more "what I decided and why." The devlog becomes a design document, not a learning journal.
- **Commit discipline:** Maintained, but the learner determines the granularity.

#### AI Role

In ha stage, AI shifts from *implementation partner with verification* to *collaborative problem-solver*. The learner drives the approach; the AI accelerates execution. The conversation looks less like instruction and more like two colleagues working through a design problem together.

The learner is now capable of evaluating AI suggestions against their own understanding of the system. They push back. They propose alternatives. They sometimes overrule the AI and are right. Sometimes they overrule the AI and learn something from the resulting failure.

#### Kanyō Evidence: Ho 05.6 — Architecture Redesign

[[ho-05_6-architecture-redesign|Ho 05.6]](examples/kanyo-pilot/ho-05_6-architecture-redesign.md) is the clearest ha-stage ho in the Kanyō arc. The learner faced a system that wasn't working (tee-based clip extraction) and instead of debugging it, stepped back to ask: *"What are we actually trying to solve?"*

This ho demonstrates ha-stage characteristics:

- **Self-directed scope:** The learner decided to redesign the architecture, not just fix the bug. No ho template told them to do this.
- **Tradeoff analysis:** The document includes a structured comparison of four alternative approaches (fix the tee, frame buffer, keyframe-only buffer, HLS sliding window) with explicit pros and cons.
- **Architectural decision-making:** "Buffer-based wins because: simplicity, precision, debuggability, RAM is cheap." This is judgment, not instruction-following.
- **Duration:** ~4 hours of deep thinking plus implementation. The 2-hour boundary would have been counterproductive here — the learner needed sustained engagement with a complex problem.
- **Reflection quality:** "Step back before debugging," "The boring solution is usually right." These are practitioner insights, not student summaries.
- **800 lines removed.** The learner had enough confidence and system understanding to delete significant working code in favor of a simpler approach.

Also notable: [[ho-06-gui-architecture-planning|Ho 06]](examples/kanyo-pilot/ho-06-gui-architecture-planning.md), the GUI architecture planning ho, shows ha-stage thinking applied to a new sub-domain. The learner was designing frontend requirements, evaluating technology choices, and making deployment decisions — work that demanded system-level reasoning, not step-by-step guidance.

#### When to Use Ha-Stage Hos

- Mid-project work where the learner has established system understanding
- When the learner is making design decisions that aren't predetermined by the ho author
- Integration work that spans multiple components and requires holistic reasoning
- Refactoring or redesign efforts driven by the learner's own judgment
- When the learner starts chafing against the shu template — this is signal, not resistance

---

### 2.3 Ri Stage — Transcend the Form

> *"The system is yours. Maintain it, extend it, keep it running."*

#### Learner Characteristics

The ri-stage practitioner has internalized the methodology's principles. They don't need a template to structure their work — they naturally scope tasks, verify outcomes, document decisions, and maintain quality standards. The principles of the ho (bounded work, clear deliverables, honest self-assessment) have become *how they work*, not something they follow.

Ri-stage practitioners:

- Have full operational understanding of their system
- Can diagnose problems, propose fixes, and evaluate outcomes independently
- Treat AI as an implementation accelerator, not a thinking partner — the thinking is already done
- Self-assess automatically (they know what they know and what they don't)
- Focus on maintenance, extension, and optimization rather than foundational learning

#### Ho Characteristics

Ri-stage work often doesn't look like a "ho" at all. The formal structure has dissolved into a lighter format — what the Kanyō pilot called "agent tasks" or simply concise problem/solution documents:

- **Duration:** No time boundary. Work until the problem is solved. Some tasks take 30 minutes; some take a full day.
- **Format:** Problem specification, not instructional sequence. States the issue, desired outcome, and relevant context. No parts, no prerequisite checks, no verification questions.
- **Documentation:** Commit messages and brief write-ups focused on *what changed and why*, not on what was learned. The documentation serves future maintainers (including the practitioner's future self), not the learning process.
- **Quality standards:** Maintained through internalized practice (linting, testing, type checking), not through template enforcement.

#### AI Role

In ri stage, AI is an *implementation accelerator for well-defined tasks*. The practitioner knows exactly what needs to happen and uses AI to execute faster than they could alone. The conversation is directive and efficient: "Fix the departure clip timing. The bug is that we're extracting from the end of the recording file instead of from the last detection time."

There is little pedagogical value in these interactions and that is fine. The learning happened in shu and ha. Ri is about *doing*, with the full competence that earlier stages developed.

#### Kanyō Evidence

The Kanyō arc shows multiple clear ri-stage examples:

- **[[ho-05_7-state-redesign|Ho 05.7 — State Machine Redesign]](examples/kanyo-pilot/ho-05_7-state-redesign.md):** Simplified a 4-state machine to 3 states, removed 938 lines of code, fixed a departure clip bug, and cleaned up dead code. The document is a concise problem/solution/result format — no instructional scaffolding, no verification questions, just competent system work.
- **[[ho-05_71-stream-outage-fix|Ho 05.71 — Stream Outage Fix]](examples/kanyo-pilot/ho-05_71-stream-outage-fix.md):** Targeted debugging of a specific false-departure issue. The learner identified that outage time was being counted as absence time, traced the bug to a reset-after-each-frame error, and moved the fix into the state machine where it belonged. Pure operational competence.
- **[[ho-05_72-startup-confirmation|Ho 05.72 — Startup Confirmation]](examples/kanyo-pilot/ho-05_72-startup-confirmation.md):** Added a `PENDING_STARTUP` state to prevent false positive notifications on container restart, plus freeze-frame handling for brief outages and extended outage recovery. The document is structured as Problem → Solution → Files Changed → State Diagram → Testing. No learning scaffolding at all — this is a practitioner documenting system changes for operational reference.
- **[[ho-06_12-admin-gui-code-check|Ho 06.12 — Code Quality Check]](examples/kanyo-pilot/ho-06_12-admin-gui-code-check.md):** Found and fixed 5 real bugs and 3 type safety issues through a comprehensive code quality sweep (pytest, black, flake8, mypy). This is maintenance work — the practitioner running quality tools they set up in shu stage, now as routine practice rather than guided exercise.
- **[[ho-06_13-arrival-confirmation-system|Ho 06.13 — Arrival Confirmation]](examples/kanyo-pilot/ho-06_13-arrival-confirmation-system.md):** Multi-frame confirmation system to reduce false positives. Designed, implemented, and tested as a complete feature — self-directed, self-scoped, self-verified.
- **[[ho-06_51-cloudflare-tunnnel|Ho 06.51 — Cloudflare Tunnel]](examples/kanyo-pilot/ho-06_51-cloudflare-tunnnel.md):** Infrastructure setup documented as a deployment runbook. Not a learning document — an operational reference written by someone who knows what they're doing and is recording it for repeatability.

#### When to Use Ri-Stage Format

- System is operational and work shifts to maintenance, debugging, and incremental improvement
- The practitioner has demonstrated they can scope, execute, and verify work independently
- Tasks are well-defined enough that the work is execution, not exploration
- Ongoing system stewardship where the practitioner owns the codebase

---

## 3. Recognizing Transitions

Transitions between stages are not events — they are gradual shifts that a facilitator (or self-directed learner) can recognize through behavioral signals. Moving too early risks building on unstable foundations. Moving too late risks frustrating a learner who has outgrown the structure.

### 3.1 Shu → Ha: From Following to Questioning

The learner is ready for ha-stage work when they:

- **Explain design decisions unprompted.** Not just "I did what the ho said" but "I chose this approach because..." The shift from executing to reasoning is the clearest signal.
- **Question AI suggestions.** When the learner pushes back on an AI recommendation and can articulate *why* ("That adds complexity we don't need" or "That doesn't account for the stream reconnection case"), they are thinking architecturally.
- **Identify tradeoffs.** The learner sees that choices have costs and can weigh them: speed vs. accuracy, simplicity vs. features, now vs. later.
- **Self-scope within a ho.** The learner starts breaking parts differently than prescribed, or identifying when a part is too big or too small. They are developing their own sense of work decomposition.
- **Exceed the time boundary productively.** If the learner consistently runs past 2 hours not because they're lost, but because they're engaged in genuine problem-solving — that's a ha signal, not a discipline failure.

**Caution:** The learner who *says* they understand but cannot explain it to someone else is not ready. Confidence without articulacy is still shu.

### 3.2 Ha → Ri: From Building to Operating

The transition from ha to ri is often driven by the project itself rather than the learner's skill level:

- **The system is live.** Work shifts from "build the next component" to "keep it running and make it better."
- **Tasks become well-defined.** Instead of open-ended design challenges, work is: "fix this bug," "tune this parameter," "add this small feature."
- **The learner owns the codebase.** They can navigate it without guidance, diagnose problems from logs, and make targeted changes with confidence.
- **Patterns are internalized.** The learner commits cleanly, tests automatically, documents as a matter of course — not because the template requires it.
- **The ho template adds friction, not value.** If writing a prerequisites check and verification questions for a 30-minute bug fix feels absurd, the learner has moved past the structure.

**Caution:** Ri-stage work still requires discipline. The danger is not moving to ri too early (that's rare — the project demands usually enforce appropriate structure). The danger is *losing the reflective practice* that shu and ha developed. A ri-stage practitioner who stops documenting decisions, stops testing, or stops self-assessing has not transcended the form — they've abandoned it.

---

## 4. Implications for Ho Authoring

### 4.1 Designing for the Right Stage

The most common authoring mistake is writing every ho as if the learner is in shu. This produces unnecessarily rigid documents for advanced learners and wastes authoring effort on scaffolding that won't be used. The second most common mistake is the inverse: writing open-ended, underspecified hos for beginners who need structure to make progress.

**Match the template to the stage:**

| Element | Shu | Ha | Ri |
|---|---|---|---|
| **Template** | [[shu-ho-template|Shu Ho Template]](framework/templates/shu-ho-template.md) | [[ha-ho-template|Ha Ho Template]](framework/templates/ha-ho-template.md) | [[ri-ho-template|Ri Ho Template]](framework/templates/ri-ho-template.md) |
| **Duration** | ~2 hours (strict) | 2–4 hours (flexible) | Until done |
| **Parts** | 4–9, author-defined | Loosely structured or self-defined | None |
| **Prerequisites** | Explicit checklist | Brief context | Problem statement |
| **Verification** | Required questions | Reflection prompts | Commit + optional write-up |
| **Devlog** | Full (learning focus) | Full (decision focus) | Brief or commit-only |
| **AI guidance** | "Review and verify each output" | "Discuss approach, then execute" | "Here's the task, go" |

### 4.2 When to Enforce Structure vs. When to Relax It

**Enforce structure when:**

- The learner is in a new domain, even if they're experienced in others. Shu resets when the context changes significantly.
- The consequences of foundational errors are high (security, data integrity, deployment patterns).
- The learner cannot yet articulate what they don't understand. Structure compensates for unknown unknowns.

**Relax structure when:**

- The learner is making decisions the ho author didn't anticipate — and making them well.
- The learner's devlogs show judgment, not just comprehension. ("I chose X over Y because..." rather than "I learned that X does...")
- The time boundary is consistently broken productively, not from confusion or scope creep.
- The learner requests more autonomy. This is almost always a valid signal.

### 4.3 The Danger Zones

**Keeping beginners in shu too long:** The learner becomes dependent on the template. They wait for instructions instead of developing initiative. They build skill at *following* hos rather than skill at *thinking through problems*. If a learner has completed 6–8 shu-stage hos and still cannot scope a simple task independently, the methodology has overcorrected toward safety.

**Advancing to ha too quickly:** The learner makes confident-sounding decisions based on incomplete understanding. They break things they don't know how to fix. They skip testing because it "slows them down." They produce code that works today but creates compounding debt. The hallmark is *inability to recover from failure* — a ha-stage learner should be able to diagnose and fix their own mistakes.

**Skipping ha entirely:** This happens when the learner goes directly from following instructions to doing operational tasks (perhaps because the system goes live before their skills have fully developed). The result is a practitioner who can maintain a system but cannot redesign it — they lack the architectural thinking that ha-stage practice develops.

---

## 5. The Kanyō Evidence

The Kanyō pilot was not designed to test shu-ha-ri progression — it was a single-learner project building a production falcon detection system. But the evidence of progression is clear in retrospect, and understanding it retroactively validates the model.

### 5.1 Shu Stage: Ho 00–04

The early hos demonstrate textbook shu-stage learning:

- **[[ho-0_5-tool-mastery|Ho 0.5 — Tool Mastery]](examples/kanyo-pilot/ho-0_5-tool-mastery.md):** Orientation. Getting tools installed, environment configured, basic comfort established.
- **[[ho-01-git-good|Ho 01 — Git Good]](examples/kanyo-pilot/ho-01-git-good.md):** Full prescriptive template. Step-by-step project setup. Explicit verification at each stage. 2-hour target. The learner builds foundational habits (virtual environments, linting, testing, commit discipline) through guided practice.
- **[[ho-02-falcon-vision|Ho 02 — Falcon Vision]](examples/kanyo-pilot/ho-02-falcon-vision.md):** Still structured, but the learner's reflections show emerging understanding. "ffmpeg still feels like incantation" is an honest Tier 1 assessment. Confidence 4/5 with specific gaps identified. The learner is engaging with the [[tiered-understanding|tiered understanding model]](framework/structure/tiered-understanding.md) as a thinking tool, not just a template section to fill out.
- **[[ho-03-live-detection-notification|Ho 03 — Live Detection]](examples/kanyo-pilot/ho-03-live-detection-notification.md):** The shift from batch processing to real-time monitoring. The ho is prescriptive but the problem is harder. The learner encounters real complexity (stream disconnections, error handling, continuous operation) within the safety of the shu structure.
- **[[ho-04-docker-deploy|Ho 04 — Docker Deployment]](examples/kanyo-pilot/ho-04-docker-deploy.md):** A massive shu-stage ho (2,183 lines) that teaches containerization, multi-stream orchestration, and ZFS-backed persistence from scratch. The document explicitly states: "No Docker knowledge needed — we teach you." This is shu-stage work in a new sub-domain (DevOps), even though the learner has gained Python fluency by this point. Shu resets when the context changes.

**What worked:** The structure provided guardrails without being infantilizing. The learner built real infrastructure (not toy exercises) while developing genuine understanding. Devlogs show increasing sophistication of reflection. Time targets were approximately honored in the early hos.

**What to notice:** Ho 02 already ran as "~4 sessions over multiple days" — the first sign that the 2-hour boundary was under pressure from real project complexity. Ho 04's length (over 2,000 lines) suggests the shu template was being stretched to accommodate material that might better have been split into multiple hos or handled with a different structure.

### 5.2 Ha Stage: Ho 05–06

The ha stage emerges mid-project as the learner develops system-level understanding:

- **[[ho-05-deployment-verification|Ho 05 — Deployment Verification]](examples/kanyo-pilot/ho-05-deployment-verification.md):** Verifying the deployed system works. The learner is now operating their own infrastructure.
- **[[ho-05_5-dev-testing|Ho 05.5 — Dev Testing Strategy]](examples/kanyo-pilot/ho-05_5-dev-testing.md):** The learner identifies a workflow problem (45-minute iteration cycles from Docker rebuilds) and designs a volume-mount development strategy. This is self-directed process improvement — nobody assigned this; the learner saw friction and solved it.
- **[[ho-05_6-architecture-redesign|Ho 05.6 — Architecture Redesign]](examples/kanyo-pilot/ho-05_6-architecture-redesign.md):** The pivot point. ~4 hours of deep thinking produced an architectural decision (tee → buffer) that simplified the entire system, removed 800 lines of code, and resolved multiple integration bugs simultaneously. The devlog reads like a technical design document: "Step back before debugging." "RAM is cheap, complexity is expensive." "The boring solution is usually right."
- **[[ho-06-gui-architecture-planning|Ho 06 — GUI Architecture Planning]](examples/kanyo-pilot/ho-06-gui-architecture-planning.md):** Requirements analysis, technology evaluation, and deployment planning for three frontend components. The learner is doing genuine architecture work — defining interfaces, evaluating tradeoffs between htmx/Alpine.js/React, and making deployment decisions (local Docker vs. Cloudflare Pages).

The ha-stage transition is most visible in the *document format*. Ho 05.6 has no prerequisites checklist, no verification questions, no time-boxed parts. It has: Problem → Thinking Process → Alternatives → Decision → Implementation → Lessons Learned. The structure follows the work, not the template.

### 5.3 Ri Stage: Operational Work

After the core system was operational, work shifted to maintenance, debugging, and targeted improvement. The document format compressed further:

- **[[ho-05_7-state-redesign|Ho 05.7 — State Machine Redesign]](examples/kanyo-pilot/ho-05_7-state-redesign.md):** Simplified 4 states to 3, removed 938 lines, fixed departure clip bug. Problem → Solution → What Changed → Results.
- **[[ho-05_71-stream-outage-fix|Ho 05.71 — Stream Outage Fix]](examples/kanyo-pilot/ho-05_71-stream-outage-fix.md):** Traced a false-departure bug to outage time being counted as absence. Identified the specific line where `total_outage_time = 0` was resetting prematurely, moved the fix into the state machine.
- **[[ho-05_72-startup-confirmation|Ho 05.72 — Startup Confirmation]](examples/kanyo-pilot/ho-05_72-startup-confirmation.md):** Added `PENDING_STARTUP` state, freeze-frame handling, and extended outage recovery. Problem → Solution → Files Changed → State Diagram → Testing. Pure operational reference.
- **[[ho-06_12-admin-gui-code-check|Ho 06.12 — Code Quality Check]](examples/kanyo-pilot/ho-06_12-admin-gui-code-check.md):** Comprehensive quality sweep finding 5 real bugs and 3 type safety issues. The practitioner is running the same quality tools they learned to use in Ho 01, now as routine maintenance.
- **[[ho-06_13-arrival-confirmation-system|Ho 06.13 — Arrival Confirmation]](examples/kanyo-pilot/ho-06_13-arrival-confirmation-system.md):** Multi-frame confirmation system to reduce false positives. Designed, implemented, and tested as a complete feature — self-directed, self-scoped, self-verified.
- **[[ho-06_51-cloudflare-tunnnel|Ho 06.51 — Cloudflare Tunnel]](examples/kanyo-pilot/ho-06_51-cloudflare-tunnnel.md):** Infrastructure setup documented as a deployment runbook. Not a learning document — an operational reference written by someone who knows what they're doing and is recording it for repeatability.

These documents share common traits: they're shorter, more direct, and focused entirely on the work. There are no "Why This Ho Matters" sections because the practitioner doesn't need motivation — they're solving problems they identified themselves on a system they own.

### 5.4 The Progression Was Natural

Nobody planned for the Kanyō arc to follow shu-ha-ri. It happened because the learner's needs changed as their capability developed, and the methodology — when it worked well — adapted to meet those needs. When the methodology didn't adapt (the 2-hour boundary becoming constraining in ha stage), the learner naturally broke from the form.

This is the model working as intended. The goal was never permanent adherence to the template. The goal was to build capability that eventually outgrows the template. When a learner *needs* the full shu structure to do good work, they should have it. When they don't, the structure should get out of the way.

**The full Kanyō progression:**

| Ho | Title | Stage | Signal |
|---|---|---|---|
| 0.5 | [[ho-0_5-tool-mastery|Tool Mastery]](examples/kanyo-pilot/ho-0_5-tool-mastery.md) | Shu | Orientation, setup |
| 01 | [[ho-01-git-good|Git Good]](examples/kanyo-pilot/ho-01-git-good.md) | Shu | Step-by-step, full template |
| 02 | [[ho-02-falcon-vision|Falcon Vision]](examples/kanyo-pilot/ho-02-falcon-vision.md) | Shu | Structured, but reflections deepen |
| 03 | [[ho-03-live-detection-notification|Live Detection]](examples/kanyo-pilot/ho-03-live-detection-notification.md) | Shu | Prescriptive, harder problems |
| 04 | [[ho-04-docker-deploy|Docker Deploy]](examples/kanyo-pilot/ho-04-docker-deploy.md) | Shu | New domain (DevOps), back to basics |
| 05 | [[ho-05-deployment-verification|Deployment Verification]](examples/kanyo-pilot/ho-05-deployment-verification.md) | Shu → Ha | Verifying own work, operational focus |
| 05.5 | [[ho-05_5-dev-testing|Dev Testing Strategy]](examples/kanyo-pilot/ho-05_5-dev-testing.md) | Ha | Self-directed process improvement |
| 05.6 | [[ho-05_6-architecture-redesign|Architecture Redesign]](examples/kanyo-pilot/ho-05_6-architecture-redesign.md) | Ha | Tradeoff analysis, architectural judgment |
| 05.7 | [[ho-05_7-state-redesign|State Machine Redesign]](examples/kanyo-pilot/ho-05_7-state-redesign.md) | Ha → Ri | Major simplification, self-directed |
| 05.71 | [[ho-05_71-stream-outage-fix|Stream Outage Fix]](examples/kanyo-pilot/ho-05_71-stream-outage-fix.md) | Ri | Targeted debugging |
| 05.72 | [[ho-05_72-startup-confirmation|Startup Confirmation]](examples/kanyo-pilot/ho-05_72-startup-confirmation.md) | Ri | Feature design and implementation |
| 06 | [[ho-06-gui-architecture-planning|GUI Architecture]](examples/kanyo-pilot/ho-06-gui-architecture-planning.md) | Ha | Requirements analysis, tech evaluation |
| 06.1 | [[ho-06_1-admin-gui-implementation|Admin GUI Implementation]](examples/kanyo-pilot/ho-06_1-admin-gui-implementation.md) | Ha | Large build, collaborative |
| 06.12 | [[ho-06_12-admin-gui-code-check|Code Quality Check]](examples/kanyo-pilot/ho-06_12-admin-gui-code-check.md) | Ri | Routine maintenance |
| 06.13 | [[ho-06_13-arrival-confirmation-system|Arrival Confirmation]](examples/kanyo-pilot/ho-06_13-arrival-confirmation-system.md) | Ri | Self-directed feature |
| 06.5 | [[ho-06_5-public-viewer-web-gui|Public Viewer]](examples/kanyo-pilot/ho-06_5-public-viewer-web-gui.md) | Ha | Substantial new build |
| 06.51 | [[ho-06_51-cloudflare-tunnnel|Cloudflare Tunnel]](examples/kanyo-pilot/ho-06_51-cloudflare-tunnnel.md) | Ri | Infrastructure setup |
| 07 | [[ho-07-yolo-training|YOLO Training]](examples/kanyo-pilot/ho-07-yolo-training.md) | Ha | New domain (ML), planning mode |

Note: The stages don't proceed in strict linear order. The learner moved between ha and ri depending on the nature of the work, and occasionally returned to shu-like structure when entering a genuinely new domain (Docker in Ho 04, ML in Ho 07). This is expected and correct.

---

## 6. Related Framework Documents

- [[ho-structure|The Ho as Unit of Work]](framework/structure/ho-structure.md) — Detailed specification of what a ho is
- [[project-arc|The Project Arc]](framework/structure/project-arc.md) — How hos sequence into complete project arcs
- [[tiered-understanding|Tiered Understanding]](framework/structure/tiered-understanding.md) — The three-tier comprehension model
- [[shu-ho-template|Shu Ho Template]](framework/templates/shu-ho-template.md) — Full prescriptive template for shu-stage hos
- [[ha-ho-template|Ha Ho Template]](framework/templates/ha-ho-template.md) — Flexible template for ha-stage hos
- [[ri-ho-template|Ri Ho Template]](framework/templates/ri-ho-template.md) — Minimal format for ri-stage operational tasks
- [[design-seed|Design Seed]](framework/design-seed.md) — The governing document for the Ho System

---

*This document is part of the Ho System framework. It describes the public methodology for adapting ho structure to learner development. For guidance on facilitating shu-ha-ri transitions with learners, see the facilitation protocols (proprietary).*
