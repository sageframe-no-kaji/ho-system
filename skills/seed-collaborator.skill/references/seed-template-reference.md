# Project Seed Template — Reference

This is a condensed reference for the Ho System Project Seed Template. The seed is the first document in the Kamae project framing chain (Seed → System Design → README → Ho Overview). It establishes the core idea of a project and becomes the evaluative foundation against which all downstream decisions are judged.

The seed is a thinking tool, not a form. These sections describe territory to explore, not fields to fill in.

---

## Where Seeds Come From

Before there is research or precedential thinking, there is friction. A problem encountered. A tool that doesn't exist. A curiosity that won't let go. The friction is the origin — the thing that makes a seed worth writing. Without something to push against, there is nothing to design.

## Precedential Thinking

The quality of a seed depends on the quality of the thinking that preceded it. Precedential thinking is the discipline of understanding what exists in a problem space before proposing to build in it. Not a literature review — honest investigation. By the time someone writes the seed, they should know where they stand.

---

## The 13 Sections

### 1. The Problem
What isn't working? Why does it persist? State in human terms, not technical gaps. "No tool does X" is a solution wearing a problem mask. The problem is what's hard or broken.

### 2. The Landscape
What exists? What's been tried? Why does it fall short? This comes before the vision deliberately — the vision should be informed by what exists. Two failure modes: skipping it entirely, or dismissing solutions without explaining why.

### 3. The Vision
What does this project accomplish? Not how it works — what it does. Plain language. If this section is three pages, it contains system design that belongs elsewhere.

### 4. Audience
Who is this for? Specific enough that design decisions can be evaluated. If "me," say so explicitly. Watch for audience descriptions that are actually value propositions.

### 5. Identity
What is it called? Why? Optional for many projects. A working name is fine. Capture naming reasoning if it matters — easy to forget later.

### 6. Project Nature and Intent
Open source or proprietary? Personal tool or shared? Learning vehicle or production? This shapes downstream decisions more than expected.

### 7. Architecture Direction
How might this work? First-pass thinking about data, interface, logic, deployment, integrations, tech choices. Opinions, not commitments. Mark provisional decisions as provisional. Depth should match what the person actually has in their head — don't create pressure to invent architecture.

### 8. Constraints
What does reality impose? Hardware, budget, time, skills, infrastructure. Different from scope boundaries — scope is what you choose to exclude; constraints are what reality excludes for you. Some of the best architecture comes from constraints.

### 9. Scope Boundaries
What is this project? What is it NOT? Where's the MVP line? At least one "this is NOT" statement.

### 10. Success Criteria
How do you know it worked? Observable outcomes, not features. Testable by someone other than the builder. At least three concrete statements.

### 11. Where I'm Starting From
What do they already know? What's genuinely new territory? Drives shu/ha/ri assignment in the ho sequence. "I know Python" is less useful than "I've written scripts but never built an API."

### 12. What I Want to Learn
What does building this develop in them? Prevents optimizing for speed when the goal is depth.

### 13. Open Questions
What don't they know yet? What assumptions might be wrong? Most important section for downstream planning. No open questions = either trivially simple or not enough thinking.

---

## After the Seed

The seed is done when the person has said what's in their head. Asymmetry is information — dense sections and thin sections reveal where thinking is rich and where it needs work.

Seeds should not all look the same. The template provides coverage; the project provides emphasis. If every seed has the same shape, the person is writing to the template instead of about the project.

The seed stays in the project as an active evaluative reference — the parti. When decisions aren't working, return to the seed and ask: is the core idea still right?

## The Kamae Chain

The seed feeds the remaining Kamae documents:

- **System Design** takes the seed's architectural opinions and turns them into decisions
- **README** takes the System Design's decisions and turns them into concrete scope
- **Ho Overview** sequences the build because the upstream documents already made the hard decisions

Each document increases commitment. The seed is exploratory. The System Design commits to architecture. The README commits to scope. The Ho Overview commits to build order.
