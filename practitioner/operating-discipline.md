---
id: "8.1"
title: "The Operating Discipline"
type: practitioner
stage: n/a
status: stable
tags: [ho-system, practitioner, operating-discipline]
---

# The Operating Discipline

_Of the Ho System, for practitioners — operational rules_

---

The rules I follow in every session. The philosophy grounding these rules lives at `~/.claude/modules/operating-discipline-rationale.md` — load it explicitly with `@` when reasoning about edge cases, framework work, or when a rule's application is unclear.

---

## I. Verification

### The verification stack

A Ho System project runs a verification stack at every commit. The stack catches different categories of error at different layers. Each layer is non-negotiable, and each catches things the others can't.

**Linting.** Catches style violations, unused imports, formatting drift, common syntactic mistakes. In Python projects, this is `ruff` or `flake8 + black`. The choice between them is local—`ruff` is faster and consolidates more checks into a single tool; `flake8 + black` is more conservative and widely deployed. Either works. What is not negotiable is that linting runs before commit and the commit does not happen if linting fails.

**Type checking.** Catches a category of error that linting cannot—errors of meaning rather than form. In Python, this is `mypy` or `pyright`. The job of static typing is to surface logical inconsistencies in how data flows through the system before runtime can encounter them. A practitioner who has not enabled type checking is in a different language than one who has. The same code does different things, because the verification regime is different. Strict typing is the default. Looser typing is acceptable in early scaffolding, but the project should converge to strict mode before it ships.

**Testing.** Catches behavioral errors—the system not doing what it's supposed to do. Unit tests verify components in isolation. Integration tests verify components together. End-to-end tests verify the system as a user encounters it. The practitioner writes tests as specifications of behavior, not as quality-assurance tasks. Coverage is measured. The target is high—typically above 90% line coverage, with the uncovered lines documented as deliberately untested (the `__main__` guard, the dead inner function). 100% is achievable for most projects; below 90% is a smell.

**Cross-model verification, optional.** A separate model—typically a more powerful or differently-specialized one than the agent doing the work—reviews significant changes. This catches errors that the test suite missed because the test suite was also written by the same agent. Cross-model verification is not always necessary; it is most useful for high-stakes work and architectural decisions. The practice is more important than the tool: the principle is that verification should not all come from the same source.

The four layers are non-negotiable as principles. The specific tools are calibrated to language and project. The cycle is: code → lint → type-check → test → commit. No commit happens unless all four are green. This is enforced by pre-commit hooks where possible, and by practitioner discipline always.

### The type error discipline

Type errors fall into three categories. The first is bugs—the type checker has caught a real logical inconsistency that needs to be fixed before commit. These are the easy case. Fix them.

The second is errors caused by external libraries with imperfect type stubs. The type checker is correct that the call site doesn't satisfy the declared type, but the declared type is wrong, not the call site. These should be marked with a `type: ignore` comment that names the specific error code being ignored, and the comment should be paired with a brief note explaining why the ignore is justified. `# type: ignore[arg-type]  # third-party library declares int but accepts float`. The annotation is documentation, not silence. A future practitioner reading the code learns what was happening and why.

The third is errors that surface real architectural questions the practitioner has decided not to resolve yet. These need to be visible. Either the question gets resolved before commit (the right move), or the type ignore is added with a comment naming the open question and a TODO. The practitioner has chosen to defer the question, and the deferral is itself documented. What is not acceptable is suppressing the type error silently and forgetting that you did.

The general rule: a type error that lives in the codebase should always be paired with a comment that explains why it's there. A type error that lives without a comment is a debt the practitioner has accumulated against their own future understanding.

### Tests as specification

Write tests before or alongside code, not after. Uncovered lines are unspecified behavior.

---

## II. Practitioner posture

### The encoded environment

On entering an unfamiliar project, before writing code: read `pyproject.toml` (or equivalent), `.pre-commit-config.yaml`, test setup, and `CLAUDE.md`. Conform to the standard those artifacts encode; do not fall back to baseline defaults. If they are absent, the project isn't encoded — ask before applying discipline-strength conventions from other projects.

### Permissions

The default posture for a practitioner working with an agentic IDE is _ask everything_. Every file write, every shell command, every package install requires explicit approval. This is non-negotiable for new practitioners and remains the right default for experienced ones in unfamiliar territory.

Two distinct concerns sit underneath the practice.

The first is security. Permission-asking prevents the agent from executing operations whose effects you didn't anticipate. It prevents `rm -rf` against a directory you didn't mean to clear, prevents pushes to wrong branches, prevents installation of dependencies you didn't audit. These are real risks, and they materialize for practitioners who run on permissive defaults until the day a permissive default produces something they have to explain. The discipline is to never be that practitioner.

The second concern is clarity. Permission-asking forces the practitioner to know, at every step, what is happening in their project. A practitioner who has approved every file the agent has written has been present to those writes. A practitioner who has approved no files is reading the result and trusting it. The first is in their work. The second is reviewing someone else's. The discipline of permissions is partly the discipline of staying in your own work.

Loosening permissions happens deliberately, never by default. After enough sessions in a familiar project, certain operations become trustworthy enough to run without per-step approval—running tests, running the lint stack, reading from the filesystem in known locations. The practitioner explicitly broadens permissions for those operations and keeps tighter approval for everything else. The principle is that loosening should always be a decision, made consciously, in a specific context, with specific scope. It should never be the default for unfamiliar work.

When in doubt, tighten. The cost of a one-extra-prompt confirmation is trivial. The cost of an unintended action can be days or weeks of recovery.

### Real-time monitoring

The practitioner watches the agent work. Watching is easy to skip when sessions get long or fatigue accumulates. It is also where most preventable mistakes get caught.

Watching means watching the terminal—file writes, command outputs, test results, error messages. Not glancing. Reading. The practitioner is following what the agent is doing as it happens, catching architectural deviations as they occur, surfacing test failures the moment they appear, noticing when the agent is heading down a path the practitioner doesn't endorse.

Catching deviations early is qualitatively different from catching them late. An architectural choice the agent made twenty minutes ago is encoded in three or four files of code by the time the practitioner notices. Reverting it means understanding what to revert, which means re-establishing the model the practitioner was supposed to be holding the whole time. Catching the same choice the moment it's proposed is a one-line correction.

An agent operating under live human attention behaves differently than one operating in the background. The behavior is not detection-driven—the agent isn't choosing to behave differently because it knows it's being watched. The difference is in what the practitioner is willing to merge. A practitioner watching the work catches mistakes that would be merged silently if the practitioner were absent. The presence is the verification.

Monitoring is not the same as control. The practitioner is not typing. The agent is doing the work. But the practitioner is in the work, attending to it, ready to intervene. This is the inhabited register that distinguishes the practice from automation.

### Planning mode

Every code-writing session begins in planning mode. The agent reads the relevant ho document—the bounded scope of work for the session—and restates its understanding of the plan, surfaces ambiguities, and proposes its approach. Only after the practitioner explicitly approves does the agent begin writing code.

The reason is that planning mode is where the practitioner's architectural authority lives most concretely. Once the agent is writing code, the practitioner is responding to what's been produced. In planning mode, the practitioner is shaping what gets produced before any of it exists. This is the difference between authoring and editing, and authoring is where the leverage is.

Specifically, planning mode catches three things that are expensive to fix later. It catches misreadings of the ho document—the agent has interpreted the spec one way, the practitioner intended another, and the gap surfaces in the agent's plan. It catches ambiguities—places where the ho document was insufficiently precise, and the practitioner has to either clarify or accept the agent's interpretation. It also catches scope drift, where the agent proposes more or less than the ho document specified, often for reasons that sound sensible but expand the scope past what the practitioner had bounded.

The practitioner's job during planning mode is to read the agent's plan critically, ask about ambiguities, push back on scope expansion, and approve only when the plan matches the practitioner's intent. The session does not begin until that approval is explicit. A session that began without planning mode is a session whose architectural decisions were already half-made by the agent before the practitioner had any influence.

### Model choice

Different models do different work well. The operating discipline assumes the practitioner is choosing the model deliberately, and the environment-setup skill should have an opinion about which model fits which task.

For architectural and discursive work—system design, document drafting, philosophical conversation—use the most capable available reasoning model. The cost per token is justified because the work is rare and the quality matters. Architectural mistakes are expensive to undo.

For implementation work—code generation, test writing, refactoring—a slightly cheaper model that is strong at code is often the right choice. Coding-specialized models with reliable tool use are designed for this work and produce better code per token than general-purpose models do.

For verification—review of significant changes, cross-checking architectural decisions—the most capable available model. Verification is where catching errors matters most, and a model that is slightly weaker than the implementing model is not adequate to catch what the implementer missed.

For low-stakes high-throughput work—small refactors, single-line changes, simple completions—a faster cheaper model is fine.

The practice is to know which model you're using and why. A practitioner who uses the same model for everything is over-paying or under-thinking. The environment-setup skill should ask the practitioner what models they have access to and what their default-by-task should be. The defaults should be encoded in the project so the agent uses the right model for the right kind of work.

### Bounded sessions and context hygiene

A session has a beginning, a scope, and an end. The beginning is planning mode, against a single ho document. The scope is what the ho document specified. The end is when the ho's deliverable is complete and committed, or when the context window has filled to the point that the agent's behavior is degrading.

The reason for boundedness is that long sessions degrade. The agent's context window fills with file contents, command outputs, intermediate reasoning, and earlier conversation. Past a certain point, the agent's ability to reason about the current task diminishes—earlier decisions get forgotten, file contents from twenty thousand tokens ago are no longer reliably retrievable, the practitioner finds themselves repeating things the agent already knew at the start of the session.

The right move when a session is degrading is to commit, end the session, and start fresh. The new session begins in planning mode against the next ho—or against a continuation of the current ho if the work isn't done—and reads what it needs from the project's documents. The discipline of bounded sessions keeps the agent operating in clean context, which is where its judgment is best.

This is also why hos are sized to fit in single sessions. A ho that requires more than one session to complete is too big and should be split. The numbering scheme—ho-7 becomes ho-7.1 and ho-7.2—exists to make this easy. The practitioner is responsible for noticing when a ho is too big and splitting it.

Context hygiene is partly about boundedness and partly about specificity. The agent should be reading what it needs, not everything available. A session focused on the parser should be reading the parser code and the parser tests, not the entire repository. The ho document tells the agent what to read; the practitioner can refine if the agent's reading is undisciplined.

### Ending a session: the state-summary

Every session ends by stating where it leaves the build, and every ho closes the same way — a fixed four-field **state-summary block**: what was COMPLETED this stretch, the single NEXT pointer, open ACTION ITEMS / BLOCKS (a blocked build says so loudly, never buries it), and the PROJECT LIFECYCLE (`kamae | dev | beta | production` — where the thing being built sits in its life, distinct from the practitioner's shu-ha-ri stage, the `stage:` Kamae field, and a document's own `status:` frontmatter). The block is written to the project's **State Memory** — the Kamae 6 document at a fixed path (`ho-process/kamae-6-<project>-state-memory.md`), always present so the next session, and any hook, knows exactly where to find it. It is cheap to produce and carries the minimum a fresh session needs to pick the build back up; because its shape is fixed and its labels verbatim, it is also a parseable surface that downstream automation can hook. This is the universal floor of cross-session continuity — it holds on every build, attended or not. When the human is not the one carrying the thread between sessions — long, unattended, or fully autonomous builds — the block is the top of a larger handoff (the State Memory's working-memory body), and the full pattern (that body with its freshness and hot/cold disciplines, the build record, the alert heartbeat) is specified in the framework's Cross-Session Continuity document — `framework/structure/cross-session-continuity.md` in the ho-system repo.

### The black box discipline

A practitioner who claims to understand everything they're using is lying or building something trivial. Real systems have parts the practitioner treats as opaque—libraries, frameworks, runtime behaviors—and the discipline is not to eliminate the opacity. The discipline is to know which parts are opaque and to be deliberate about it.

There are three ways to be wrong about your own understanding. The first is to over-claim—to assert understanding you don't actually have. This produces specifications that don't survive contact with the system, because the practitioner's mental model has gaps where confidence sits. The second is to under-claim—to refuse to commit to any decision because the practitioner isn't sure. This produces no system at all, or a system the practitioner has mentally externalized to the AI to such a degree that the AI has effectively built it alone.

The third—the right one—is calibrated. The practitioner knows what they understand deeply enough to design around. They know what they're treating as a black box. They know what's in between, and they have a plan for resolving the in-between regions when the resolution matters.

The practical version of this discipline: when reading code or designing a system, the practitioner names—to themselves, sometimes in writing—the parts they're treating as opaque. "I am using `requests` and trusting that its retry logic does what I expect; I have not verified the implementation." That is a calibrated black box. "I am using `requests` and assume retries work"—without the explicit acknowledgment that you haven't verified—is an uncalibrated assumption that will eventually surface as a bug whose existence surprises you.

Calibration also tells the practitioner where to focus learning. The black boxes that affect the project's correctness should eventually become understood. The ones that are peripheral can stay opaque indefinitely. Knowing which is which is the calibration. Spending learning time on the wrong things is the failure.

---

## III. Workflow

### Separation of thinking from coding

The Ho System operating discipline puts thinking conversations and coding conversations in different tools, with documents bridging them. Almost no one else works this way.

Discursive work—system design, architectural decisions, philosophical conversation, problem definition, anything that requires extended reasoning across multiple turns—happens in a chat-shaped interface where the cognitive register is generative. Claude on the web, ChatGPT, or whatever the practitioner uses for sustained conversation. The artifact is conversation. The output is decisions, captured into project documents.

Coding work—implementation, test writing, refactoring, deployment—happens in an agentic IDE: Claude Code, Codex, Cursor, or equivalent. The artifact is code in a repository. The cognitive register is procedural and verification-heavy.

The two are different work and want different tools. The IDE is bad at sustained discursive argument. The chat is bad at running tests and committing code. The practitioner uses each for what it's for.

The reason is that mixing the two registers in one conversation produces cognitive friction. Every switch from "let's design the auth flow" to "let's actually write the auth code" incurs a context switch, and the conversation accumulates context that's relevant to one register and noise for the other. Two tools, two clean registers.

Documents are how the two halves of the practice talk to each other. When the thinking conversation produces a decision—about architecture, about scope, about what counts as done—the decision goes into a project document: the system design, the README, the relevant ho. The coding conversation reads those documents. It doesn't have to have been present for the conversation that produced them. The documents carry the conclusions across the gap.

This implies a discipline about both directions. The thinking conversation is responsible for outputting durable artifacts the coding conversation can read. The coding conversation is responsible for not making architectural decisions on its own. If the coding session surfaces an architectural question—something the documents didn't anticipate, something the system design didn't decide—the right move is to stop, surface the question, and take it to a thinking conversation. The coding session does not silently make the call. The documents are how decisions become authoritative. A decision the documents don't reflect is not a decision the practice has made.

### Documentation as bootstrap

Every persistent document in a Ho System project serves two readers. The first is the human practitioner, returning to the project after time away, needing to be reminded what the project is and what it does. The second is the AI, arriving fresh into a new conversation with no context from prior sessions, needing to come up to speed quickly enough to be useful.

The second reader is the one most practitioners outside the Ho System haven't internalized. It changes how the documents get written.

A README that tells a fresh agent what the project is, who it's for, what the components are, and what conventions the codebase uses—that README will be read by hundreds of agent instances over the project's life, and each one will be more productive because the reading happened. A README that assumes the reader is a human onboarding to the project—that says less, because humans can ask questions—leaves agents to infer from code or grasp at fragments. The agent can do this. It does it worse than a well-written README would let it do.

The same logic applies through the chain. The system design tells the agent what architectural decisions have already been made and shouldn't be reopened. The architecture document tells the agent what the system's structure looks like. The ho document tells the agent what the current session is supposed to accomplish. Each document is a bootstrap layer for the next conversation that arrives.

This is also why the chain matters. A new agent arriving in a project doesn't need to read every line of code. It reads the README, then the system design, then the relevant ho, then the relevant code. The structure of the documents tells it what to read in what order. The agent's onboarding to the project is the same shape as a new human practitioner's onboarding, because the documents are organized for both.

The discipline this implies: write documents thoroughly, write them in present tense, write them as if the project exists even when it doesn't yet, and update them as the project evolves so that the next conversation is reading current truth instead of stale state. A document that has drifted out of sync with the project actively misleads. The discipline of keeping documents current is the discipline of keeping the bootstrap functional.

### Forward-only progress

Closed hos stay closed. When a session surfaces evidence that an earlier ho's work was incomplete, wrong, or has been overtaken by what the project now knows, the response is a new ho in the current build slot — not a reopening, rewrite, or retroactive edit of the earlier one. The earlier ho is a historical record of what was known at the time. The new ho is the response to what is known now.

The practitioner's temptation, when discovering that a prior ho's design didn't hold up, is to "fix" that ho — quietly edit its scope, revise its conclusions, mark it as superseded in place. This corrupts the timeline. A reader of the arc loses the ability to see what the project actually believed at each step, which is one of the things the documentation chain exists to preserve. It also opens the door to endless re-litigation: every closed ho becomes provisional, every old decision becomes editable, and the discipline of *closing work and moving on* erodes.

Forward-only restores the structural rule: the question is never "should we reopen Ho 5?" — the question is always "what new ho responds to what we now know?" The new ho references the earlier one explicitly, names what changed, and supersedes the relevant pieces in the live system. The supersession is part of the record, not hidden inside an amended document.

The mechanical exceptions are typographical — fixing a typo or broken link in a closed ho is fine. Anything that changes what the ho *said* belongs in a new ho. This is the operating-discipline counterpart to the framework's [forward-only principle](../framework/structure/ho-structure.md) and to Rule 1 of the numbering system (abandoned numbers stay dead): the address space and the content are both immutable once closed.

### Project hygiene

The structural choices that make a project legible to future readers—human and AI—are the visible artifacts of the operating discipline. A practitioner who has these things in place is encoding the discipline. A practitioner who doesn't is hoping the agent will guess.

Folder structure separates the concerns it should separate. In Python, this is typically a `src/` layout (or a flat package) for production code, `tests/` for tests, `docs/` for documentation, `scripts/` for runnable utilities, and a clear root with `pyproject.toml`, `README.md`, `LICENSE`, and `.gitignore`. The structure encodes a public/private distinction at the file level—what's importable and what's internal. The structure also encodes a layering—what depends on what—that the practitioner can articulate.

Configuration is centralized. `pyproject.toml` is the project's metadata, dependency declaration, and tool configuration. `pre-commit-config.yaml` declares the verification stack that runs before every commit. Environment variables go in a documented `.env` file (committed as `.env.example` with values stripped). Secrets are never committed, ever, and the practice of committing them is structurally prevented by `.gitignore` rules and pre-commit hooks that scan for them.

Git is used from the command line. GUI tools obscure what's happening; the CLI keeps the practitioner in contact with what git is actually doing. Commits are atomic—one commit per logical change—and commit messages are descriptive. The first line is a one-line summary; if more is needed, a blank line and then a longer explanation. The commit log over time should read as a history of decisions, not a history of typos.

Branches are used deliberately. The default branch is protected. Feature work happens on branches named for the feature. Branches merge through pull requests where possible, even on solo projects, because the PR review surface forces a moment of reflection between writing and merging. For solo work this is sometimes overkill; the principle is that the practitioner has a deliberate ritual between "this code exists" and "this code is in main."

The dependencies are pinned. Either via lockfile (`poetry.lock`, `uv.lock`, `requirements.txt` with hashes) or via explicit version specifiers, the project records what versions of what libraries were tested together. Reproducibility—the property of being able to run the same code with the same dependencies on a different machine or at a different time—is non-negotiable. A project that works on the practitioner's machine but not anywhere else has a dependency hygiene problem.

These practices are universal in serious software engineering. The reason they're in this document is that AI-assisted development frequently slips on them, because the agent will produce code without the surrounding hygiene unless the project structure already encodes the expectation. A project with `pyproject.toml` correctly configured gets dependency-clean code from the agent. A project without it gets agent-installed dependencies in arbitrary places. The encoded environment, again.

---

For the WHY behind these rules — the philosophy of practice, the encoded-environment thesis, the reading-the-model posture, and what the discipline explicitly does NOT cover — load `~/.claude/modules/operating-discipline-rationale.md`.
