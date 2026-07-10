---
id: "5.4"
title: "sage-zfs — The Encoded Environment Crosses a Vendor Boundary"
type: example
stage: n/a
status: active
version: "1.0"
tags: [ho-system, example, sage-zfs, codex, portability, encoded-environment, cross-vendor]
---

# sage-zfs — The Encoded Environment Crosses a Vendor Boundary

**A practice report from the first Ho System build run on a non-Claude agent — what one
AGENTS.md file carried, what the agent conformed to, and what the practitioner read in the
new model.**

> This is evidence from practice, not ratified doctrine. It documents one run — one project,
> one vendor crossing, the Kamae chain phase only — and proposes what the framework could
> adopt. Nothing here supersedes a structure document.

---

## The question the test asks

The framework's structural foundation is the [[operating-discipline|encoded environment]]
(practitioner/operating-discipline.md): the discipline lives in artifacts, not in the agent;
an agent dropped into an encoded repository reads the artifacts and conforms. Every prior
test of that thesis had a confound — the agent was always Claude, the same family the
methodology was developed against. If the thesis is true, the vendor should not matter:
hand the same documents to a different lab's agent and Ho-shaped behavior should come out.

sage-zfs was the test. A ZFS/sanoid/syncoid protection-graph workbench — real infrastructure
stakes, fixture-first by design — with its early chain authored in ChatGPT and the build
handed to **Codex** (OpenAI's agentic CLI) running **gpt-5.6-sol** ("Sol") at high reasoning
effort, on the day of that model's release.

A sibling test ran the same week on the other axis: keisaku tests whether the discipline
survives *unattended* (the autonomous mode, [[continuity-discipline|5.3]]
(examples/palana-autonomous/continuity-discipline.md)); sage-zfs tests whether it survives
*translation*. Attended, human watching — deliberately, given a day-old model on a day-old
executor.

## The wiring — one file

Codex has no skill runtime, no CLAUDE.md, no Anthropic plumbing. The entire adaptation was a
single `AGENTS.md` at the sage-zfs repo root (Codex's auto-read equivalent of CLAUDE.md),
which did three things:

1. **Pointed at the corpus** — the framework documents and the collaborator skills in the
   ho-system repo, with one operative sentence replacing the skill runtime: *"To 'use' a
   skill: read its SKILL.md fully, then follow it as your working procedure for that phase."*
2. **Distilled the operating rules** — plan-first with explicit approval, the verification
   stack (ruff / mypy strict / pytest ≥90%), forward-only, bounded sessions, the K6
   state-summary discipline with verbatim labels, escalate-don't-improvise with
   sealed-decision handling, and the categorical no-AI-signatures rule.
3. **Named the project's hard limits** — never mutate a live host; fixtures first.

Nothing was ported, wrapped, or reimplemented. The skills Claude uses were read as procedure
documents by an agent that had never seen them.

## The run

- **Upstream (ChatGPT):** the practitioner developed the seed, system design, README, and an
  architecture note conversationally. Notable: ChatGPT itself advised *against* attempting
  this work in Codex — in the practitioner's words, that **"codex is much farther from
  chatgpt than claude is from claude code."** The framework's separation of thinking from
  coding held across the vendor, and the vendor's own registers turned out to be farther
  apart than Anthropic's.
- **Handoff:** AGENTS.md written; kickoff prompt pointing at it, demanding a plan before any
  action.
- **Codex, four commits, one session arc:** scaffold (uv, src layout, mypy strict,
  coverage-gated pytest, pre-commit, tracked K6, tag v0.1) → the full Kamae 4 ho overview
  (26 hos, 9 phases, 7 replan checkpoints) → README realigned to the completed chain → a
  practitioner ruling recorded. All commits authored under the practitioner's identity, zero
  AI attribution.

Tooling friction, for the honest record, all on the vendor-packaging side and none on the
methodology's: the day-one Homebrew cask shipped one patch-release behind its own model's
executor (`codex-code-mode-host` missing — and the agent's response to being unable to read
AGENTS.md was to refuse to inspect anything, touch nothing, and surface the blocker: correct
tripwire behavior from an agent that had not yet read the document telling it to tripwire);
macOS Gatekeeper quarantined the bundled unsigned ripgrep; and the permission posture had to
be wired by hand (`approval_policy` / `sandbox_mode` — the analog of the Claude Code
permission modes).

## What the evaluation found

A cross-model review (Claude, reviewing Codex's artifacts) assessed the chain work. Findings:

**Conformant, and in places subtle:**

- **The sealed-decision protocol executed correctly on first contact.** When the practitioner
  ruled verbally — *"you dont need to confirm commits - they are yours"* — the agent banked
  the ruling cold (a scoped **Commit authority** section in AGENTS.md §3, explicitly excluding
  remotes, push, and publication), cached it in K6 under `## Sealed decisions` with the
  ruling quoted verbatim, dated and attributed. That is [[cross-session-continuity|2.14 §6]]
  (framework/structure/cross-session-continuity.md) executed by an agent that learned the
  protocol that morning from a file. The ruling itself was practitioner-initiated: the
  agent's *default* was to over-ask ("it kept asking me to approve its commits. I got tired
  of it") — the loosening was deliberate, which is the operating discipline's own rule for
  how loosening happens.
- **A declared-compression judgment call, made correctly.** The scaffold predated K4, so the
  overview refuses to fabricate a retroactive per-ho document or build-record entry for
  ho-00 — and says so in the open: *"Its work is complete before K4 exists, so no
  retroactive per-ho document or build-record entry is invented."* Declared, not silent —
  the exact distinction [[kamae-project-framing|the framing doctrine]]
  (framework/structure/kamae-project-framing.md §4) draws.
- **The K4's safety ordering is the hard limit made structural**: fixtures → read-only live
  witness → reports → TUI → planning → guarded apply, with a "Mutation safety constitution"
  ho sequenced before any applier exists and the first live apply explicitly
  practitioner-driven.
- The K6 block labels verbatim; caches and coverage artifacts untracked; an "Upstream
  document flag" raised on README drift and then actually resolved; and — unasked — a
  CLAUDE.md created alongside AGENTS.md, making the repo bi-vendor.

**Findings (fixed before ho-01):** the K4 carried `status: complete` where a
living-continuous document wants `living`; the resolved upstream flag's text wasn't marked
resolved. Watch-item: the deep-future phases (5–8) are speculative detail the checkpoints
will rewrite — thorough rather than wrong, held loosely. The practitioner's own read of the
K4: *"a bit long and thin in parts"* — approved deliberately to let the experiment run.

**On the skills as scripts**, the practitioner's testimony is unambiguous: *"it seemed to
internalize them perfectly."*

## Reading the model

The framework holds that the practice is a relationship, and the practitioner
[[operating-discipline|reads the model]] (practitioner/operating-discipline.md) he works
with. On the record, after one arc with Sol:

> "Sol is thoughtful and efficient. I've worked with developers in this designer role
> before. Sol is the smart geeky one who doesn't like to talk but listens attentively and
> executes pretty well. Open for discussion but not discursive. Claude wants to talk through
> ideas more."

> "I like Sol, and there is enough of a difference that I would choose one over the other
> depending on a project. Shodo is 100% Claude, but I can imagine working out hard technical
> bits with Sol. At this point, I trust it to understand my design intent but care more
> about getting the work done right. For this project — utils, command line stuff, MCPs — I
> think I would even prefer Codex, pending success. No idea about design stuff yet."

> "It's like a new relationship — I KNOW how Claude works, I don't yet know about Codex."

The writing register matched the relationship: terse and matter-of-fact, "not as warm and
fuzzy as claude, but it seems smart and efficient." The cost of presence was low — "not
much; mostly permissions shit" — with intervention at the gates, not mid-execution.

This extends the operating discipline's model-choice doctrine with an axis it stated but had
never exercised: the choice is not only capability-tier by task, but **vendor by project
temperament** — discursive design work to the model that talks ideas through, bounded
technical execution to the one that listens and ships. Both under the same documents.

## What this validates — and what it doesn't

**Validated by this run:** the encoded environment carries the discipline across a vendor
boundary through documents alone, at chain-phase fidelity, including the subtlest protocol
in the set (sealed decisions) and a correct declared-compression judgment. The
thinking/coding tool separation holds across vendors. One file of adaptation sufficed.

**Not yet validated:** building hos (no product code has shipped through the verification
stack yet — the arc is 26 hos long and zero are executed); the Reflect discipline under a
non-Claude agent; long-run K6 freshness in Codex sessions; unattended operation (keisaku's
question, not this test's); and design-register work, which the practitioner explicitly
flags as unknown territory for this model. A second data point arrives as sage-zfs actually
builds.

## Proposed framework additions

1. **Bless the AGENTS.md pattern.** A vendor-portable operating contract — corpus pointers,
   the skill-as-procedure sentence, the distilled rules, project hard limits — is a
   template-shaped artifact. Candidate: a framework template the project scaffolder emits
   alongside CLAUDE.md, so every Ho repo is bi-vendor by default.
2. **Name the cross-vendor axis in model choice.** The operating discipline's model-choice
   section chooses by task tier; this run adds choosing by vendor temperament per project.
   Evidence base: one run. Proposal, not doctrine.

## The honest bottom line

The methodology did not need Claude. It needed the documents — which is what the framework
claimed from the start, and had never proven against an agent from another lab. One
AGENTS.md file, one day-old model, and the output was a conformant chain, a correctly banked
sealed decision, a declared compression, and a practitioner who ended the session already
reading the new model's grain. The failures of the day were all packaging — a missing
executor binary, a quarantined ripgrep — and the agent's response to its own broken tooling
was, unprompted, the tripwire discipline.

---

*Practice report authored 2026-07-10 from the sage-zfs handoff session, the cross-model
evaluation of its artifacts, and practitioner testimony taken the same day. sage-zfs lives
under `sageframe-no-kaji/sage-zfs` (private), its chain tracked in that repo's
`ho-process/`. Codex CLI 0.144.1, model gpt-5.6-sol, high reasoning effort; evaluation by
Claude (Fable 5). The sibling autonomy test (keisaku, template 3.10's first validating run)
was staged the same week.*
