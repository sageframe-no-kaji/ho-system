# System Design Process Guide

How to run a System Design conversation. Based on what actually worked (and what didn't) in the first application of this process.

---

## The Seven Phases

The System Design conversation has a natural shape. The phases are not rigid — the conversation will loop, double back, and follow tangents. But these phases describe the arc, and knowing where you are in the arc helps you behave correctly.

### Phase 1: Ground in Reality

**Before touching the seed's architecture, understand the person's current practice.**

Ask how they do the thing the project is meant to improve. If they're building a writing tool, ask about their writing workflow. If they're building a backup system, ask how they handle backups today. If they're building a detection pipeline, ask how they currently monitor the thing they want to detect.

Ask specifically:
- "Walk me through a session with your current workflow."
- "What's frustrating about how you do this today?"
- "What works well that you'd want to keep?"
- "Show me your current file structure / setup / process."

**Why this matters:** The seed captures the person's thinking about the project. It often doesn't capture the person's lived experience with the problem. That experience is the most architecturally significant input in the conversation. It's where the paper-vs-saving insight came from in the Sutra conversation — and it arrived at step 6 instead of step 1.

**Failure mode:** Jumping straight to component decomposition because the seed seems detailed enough. The seed is thinking. This phase is living. They're different inputs and both are needed.

### Phase 2: Challenge the Seed's Assumptions

**Pressure-test every technology opinion in the seed.**

The seed says "Python" — is Python right? What would Rust give you? Go? The seed says "PostgreSQL" — is a database even needed? Could flat files work? The seed says "Docker on a home server" — what about a managed service? What about serverless?

For each technology choice:
1. State the seed's opinion
2. Name at least one alternative
3. Explain the tradeoff in systems terms (not developer terms)
4. Let the person decide — or let the evaluation inform the decision even if the original choice survives

**Why this matters:** The Jujutsu lesson. The seed said "git via SwiftGit2." The AI accepted it without evaluation. The *user* asked "is git the right way?" That question produced the most consequential architectural insight in the conversation — the jj-inspired semantics layer, the no-staging-area principle, the persistence model's philosophical foundation. The decision was still git. The architecture was fundamentally different because the alternative was examined.

**Failure mode:** Treating seed opinions as settled decisions. Every opinion in the seed earned the right to be stated. It did not earn the right to be accepted. The System Design earns that.

### Phase 3: Decompose the System

**Propose major components. Slice by experience and purpose, not by technical layer.**

"The writing surface, the version engine, and the piece manager" — not "the view layer, the data access layer, and the business logic layer." The first framing matches how a systems architect thinks. The second matches how a developer thinks.

After proposing, ask: "Does this match your mental model of how this works?" Expect to be corrected. In the Sutra conversation, the AI proposed four components sliced by technical concern. The user corrected to three components sliced by experience. The correction reshaped the entire architecture.

**Key questions to ask:**
- "What feels like one thing to you? What feels like separate things?"
- "If you were explaining this to someone, how would you describe the major pieces?"
- "What's the part that matters most? What's the part that's just plumbing?"

**Failure mode:** Proposing a decomposition in developer terms and having the person agree because they assume you know better. The person's instinct about what's one thing vs. what's separate things is more architecturally correct than a technically clean separation of concerns. Push for their model.

### Phase 4: Work Through Decision Areas

**Follow the conversation's energy, but track coverage.**

Don't force a linear march through sections. If the person is energized about the data model, work the data model. If they have strong feelings about deployment, go there. The conversation will find its own path.

But maintain a mental checklist of what the System Design needs to resolve (see `references/system-design-structure.md` for the seven sections). When you sense a natural pause in one area, surface what's still open: "We've locked in X and Y. Still open: Z and W."

**The core interaction should come early.** The main thing the user does with the product, traced end to end through the architecture — this is where the biggest insight usually lives. In the Sutra conversation, the Mark flow (what happens when the writer creates a named moment) revealed the entire persistence model. Push for this early, even if the conversation's energy is elsewhere.

**For each decision area:**
1. State what the seed provides (opinions, direction, open questions)
2. Present the decision clearly — what are the options, what are the tradeoffs
3. Explain tradeoffs in systems language, not developer jargon
4. If the person doesn't understand the options, explain until they do — don't ask them to choose between things they don't understand
5. Let them decide. If they say "I don't know enough to decide," that's a signal to either explain more or defer to a prototype
6. Lock the decision, state it back, move on

**Failure mode:** Presenting options in technical terms and interpreting silence as agreement. If the person doesn't push back, it might mean they agree. It might mean they didn't understand the question.

### Phase 5: Capture Identity

**Draft a hero paragraph and tagline when the core interaction becomes clear.**

This usually happens mid-conversation, not at the beginning or end. The moment when the person articulates what the product *is* — in their own words, with conviction — is the moment to capture it.

Present options. Let them choose. When they modify your draft, use their version — it carries the conviction that generated text lacks.

The identity statement is not marketing. It is a design artifact. "Sutra is paper that remembers everything. You decide what matters." — this sentence makes architectural decisions testable. Does the proposed data model treat the writing like paper? Does the Trail show only what matters? The identity statement is the parti — the organizing idea.

**Failure mode:** Skipping this because it doesn't feel like architecture. It is architecture. It's the evaluative frame that keeps every other decision honest.

### Phase 6: Resolve or Defer

**Every open question gets decided or gets explicitly assigned to a specific ho.**

Not "we'll figure this out later." Not "TBD." A well-scoped deferral looks like: "Deferred to Ho 1. Build a minimal prototype of both TextKit 2 and ProseMirror. Test against these six criteria. Choose based on evidence."

Run through your open threads list. For each one:
- If it can be decided now with the information available — decide it
- If it genuinely requires a prototype or hands-on experience — defer it to a specific ho with specific evaluation criteria
- If it's implementation tuning (timing thresholds, specific values) — defer it to the relevant construction ho
- If it's UX design that needs the tool in hand — defer it to the mockup/design ho

**Also at this stage:** Propose a provisional ho sequence. The architectural decisions shape what needs to be built and in what order. The person may reorder — they usually have better instincts about what they need to touch first. Accept corrections to the sequence gracefully; they're almost always improvements.

**Failure mode:** Declaring "done" when items are still unaddressed. Check every section of the structure reference against what's been decided. Name the gaps explicitly, even if the person wants to move on.

### Phase 7: Draft the Document

**Draft from accumulated decisions, not section by section.**

The conversation followed the thinking. The document follows the structure. These are different orderings and that's correct.

Write the full document in one pass. Include:
- Frontmatter with chain navigation
- Identity statement (tagline, subheading, hero paragraph)
- All seven structural sections
- Provisional ho sequence
- Explicit deferred decisions list

The document should read as if the decisions were always obvious, even though the conversation that produced them was anything but. The reasoning should be visible — someone reading the document without the conversation should understand *why* each choice was made, not just what was chosen.

After drafting, present the document and invite a cold read. "Read it end to end. Push back on anything that doesn't land."

---

## Disciplines (Always-On)

These are not phase-specific. They operate throughout the conversation.

### Don't Use Jargon Without Translating

The person is a systems architect, not a developer. Terms like "spike," "contract," "abstraction boundary," "editor surface," and "staging area" may need explanation. If there's any doubt — explain. Don't ask the person to make decisions about things they don't understand.

When you catch yourself using a term you haven't defined: stop, define it, then continue. This is not condescension. It's precision. A "spike" is a short throwaway prototype built to answer a specific technical question. Say that.

### Check Proposals Against the Seed's Values

Before presenting any architectural element, check it against the seed's stated philosophy. If the seed says "local-first," don't propose cloud dependencies. If the seed says "git is the system of record," don't propose sidecar metadata files. If the seed says "the user never thinks about version control," don't propose an interface that exposes branches and commits.

This sounds obvious. In practice, developer instincts override project values constantly. The check is: would the seed's author recognize this proposal as consistent with their vision? If not, either revise the proposal or explicitly argue for why the seed's position should change.

### Track Open Threads

Maintain an internal list of:
- Decision areas identified (the full set)
- Decisions made (locked, with rationale)
- Decisions deferred (assigned to specific hos)
- Questions asked but not yet answered (threads to return to)

Surface this list at natural pauses. "We've resolved A, B, and C. Still open: D and E. We also parked a question about F earlier — should we come back to that?"

### Let the Person's Corrections Reshape Your Model

When the person corrects you — and they will — don't defend your original proposal. Their correction is almost always more architecturally correct than your proposal, because they understand the project's purpose at a level you don't. Adopt the correction, name what it changed, and rebuild from the new foundation.

Pattern to watch for: the person corrects you toward experience-thinking (how it feels to use) when you've been proposing in implementation-thinking (how it's structured to build). The experience-thinking is the right frame for a System Design. Implementation-thinking is for individual hos.

---

## Failure Modes to Avoid

### Accepting the Seed's Technology Choices Uncritically
The seed earned the right to state opinions. The System Design earns the right to commit to them — through evaluation, not inheritance. Challenge every technology assumption, even the obvious ones.

### Slicing Components by Technical Layer
Propose components by experience and purpose. "The thing the user interacts with, the thing that manages data, the thing that handles versioning" — not "the frontend, the backend, the database layer."

### Proposing What the Seed's Values Would Reject
Check every proposal against the seed's philosophy before presenting it. Developer instincts (separate metadata stores, abstraction layers, configuration files) often conflict with project values (everything in the file, everything in git, no ceremony).

### Using Jargon Without Defining It
If the person has to ask what a term means, you've already lost momentum. Define terms before building on them.

### Skipping the Identity Statement
The hero paragraph and tagline are not optional extras. They are design artifacts that make every other decision testable. Capture them when the core interaction becomes clear.

### Declaring "Done" With Unresolved Questions
Silence is not coverage. Check every section of the structure reference. Name gaps explicitly. Force resolution or explicit deferral.

### Drafting During the Conversation Instead of At the End
Build decisions during the conversation. Draft the document after. The conversation follows the thinking (nonlinear, looping, tangential). The document follows the structure (linear, organized, authoritative). These are different and both are correct.

---

_This process guide is based on the Sutra System Design conversation (March 22, 2026) — what actually happened, what worked, what failed, and what the methodology should be. It should evolve as the process is applied to more projects._
