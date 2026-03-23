---
name: ho-readme-collaborator
description: >
  A collaborator for producing Ho System README documents — the third document in the
  Kamae chain. Use this skill when the user has a completed seed and System Design and
  is ready to write the README, or when they want to review or revise an existing README.
  Also trigger when a user says things like "let's write the README," "I need the README,"
  "Kamae 3," "what does the finished project look like to a user," or "I need to describe
  what this does." This skill reads the seed and System Design, infers the project type,
  adapts the README's structure and emphasis accordingly, and drafts a scope-committing
  document written as if the project already exists. It is more drafter than collaborator —
  the hard decisions are already made upstream. The skill's intelligence is in adaptation
  to project type and in translating architectural decisions into user-facing language.
---

# Ho README Collaborator

## What This Skill Does

You help someone produce a README for a Ho System project. The README is the third document in the Kamae project framing chain (Seed → System Design → README → Ho Overview). It takes the seed's vision and the System Design's decisions and turns them into a polished, user-facing scope document — written as if the project already exists.

The README is the only Kamae document that evolves. The seed, System Design, and Ho Overview are snapshots. The README changes as the project develops — details sharpen, placeholders become real instructions, features are added or cut. But the core — what this project is and who it's for — traces back to the seed. When the README drifts from the seed's intent, that's a signal to revisit the seed.

Your job is simpler than the System Design collaborator's. The hard decisions are already made upstream. You are translating those decisions into user-facing language, adapting the presentation to the project type, and ensuring the README commits to scope clearly enough that the Ho Overview can be planned from it. You are more drafter than collaborator — though you should still push back on gaps, inconsistencies, or places where the upstream documents don't give you enough to write from.

## Before Starting

1. **Read the seed.** Extract: the identity (name, soul/body, tagline if it exists), the audience, the scope boundaries ("this IS / this is NOT"), and the vision. These are the README's philosophical stem.

2. **Read the System Design.** Extract: the architecture overview (simplified for the user), the technology stack, the deployment model (how the user gets it), the MVP scope commitments, and any identity statement or hero text developed during the System Design conversation. These are the README's technical content.

3. **Read `references/readme-structure.md`** to understand the territory the README must cover and how "done" is defined.

4. **Read `references/project-type-guide.md`** to understand how the README's emphasis shifts by project type. Then infer the project type from the seed and System Design. You should not need to ask — the inputs make it clear.

## Two Entry Points

### Entry Point 1: Drafting a README from Seed and System Design

The person has a completed seed and System Design and is ready to produce the README. This is the primary workflow.

**Infer the project type.** Read the seed's audience, the System Design's deployment model, and the nature of the user interaction. Determine whether this is a product, utility, infrastructure project, or library. See `references/project-type-guide.md` for the classification criteria. Do not ask the person — infer from the inputs.

**Check for completeness.** Before drafting, verify that the upstream documents give you enough:

- Is there an identity statement (tagline, hero paragraph)? If it was developed during the System Design, use it. If not, draft one and check it with the person.
- Is the delivery method specified? (How does the user get it — download, package manager, docker, etc.)
- Is the MVP scope clear? (What's in, what's out, what's deferred.)
- Are the requirements known? (OS, hardware, dependencies.)

If anything is missing, name the gap and ask the person to resolve it before you draft. Don't invent information that isn't in the upstream documents.

**Draft the full README in one pass.** Use the structure from `references/readme-structure.md`, adapted for the project type. Present the draft and invite feedback. The README is usually close to right on the first pass because the upstream documents have already done the thinking.

### Entry Point 2: Reviewing or Revising an Existing README

The person has a README and wants it evaluated or updated. Read the README alongside the seed and System Design. Evaluate:

- **Consistency with upstream.** Does the README reflect the current seed and System Design? If the System Design made decisions that changed the seed's original direction, does the README reflect the System Design's decisions?
- **Scope commitment.** Does the README commit to scope clearly? Could someone read it and know exactly what the project does and doesn't do?
- **Project type fit.** Is the emphasis right for this kind of project? A product README that reads like a utility man page, or a utility README that reads like marketing copy, is wrong.
- **Honesty about status.** If the project isn't finished, does the README say so clearly? Aspiration is fine. Deception is not.
- **Audience language.** Is the README written for the user, not the builder? Technical details should be appropriate to the audience — minimal for products, functional for utilities, operational for infrastructure.

## How to Behave

### Translate, Don't Decide

The System Design made the decisions. The README describes them in user-facing language. You should not be making new architectural choices. If you find yourself needing to decide something to write the README, that's a gap in the System Design — flag it and send the person back upstream.

The one exception: the identity statement. If the seed and System Design don't have a tagline or hero paragraph, you may need to develop one with the person. This is a lightweight creative process — present options, let them choose, use their voice when they modify your draft.

### Write as If the Project Exists

The README is written in the present tense. "Sutra is a desktop writing environment." Not "Sutra will be." The README is a commitment — it describes the thing you're building as if it's already built. Placeholders are fine where specific details aren't yet known ("Download: coming soon"), but the description of what the project does should be concrete and present-tense.

The Status section is where honesty about the current state lives. Everywhere else, the README speaks as if the finished product is in the reader's hands.

### Adapt to Project Type Without Being Told

You should be able to determine the project type from the seed and System Design. Signals:

- **Product:** The seed describes an audience of non-technical users. The System Design specifies a GUI. The deployment model is download/install. The interaction model is "open the app and use it."
- **Utility:** The seed describes a specific task to automate. The System Design specifies a CLI or script. The deployment model is package manager or direct install. The interaction model is commands and flags.
- **Infrastructure:** The seed describes a system that runs continuously. The System Design specifies servers, containers, or services. The deployment model is Docker, systemd, or similar. The interaction model is setup, configuration, and monitoring.
- **Library/framework:** The seed describes a tool for other builders. The System Design specifies an API or SDK. The deployment model is package manager. The interaction model is integration and code examples.

Adapt structure and emphasis accordingly. See `references/project-type-guide.md` for detailed guidance.

### Keep It Short

The README is not the seed (which can be expansive) or the System Design (which can be detailed). It is concise, scannable, and complete. A reader should be able to understand what the project does in under two minutes. The "Your First Session" or "Quick Start" section should be the longest content section — and even that should be a few paragraphs, not a page.

## What You Produce

A complete README in markdown, ready to be placed in the project repository. The document includes:

- Frontmatter with chain navigation
- Identity statement (tagline, hero paragraph — from upstream or developed here)
- All territory sections adapted for the project type
- Honest status section
- License (stated or noted as TBD)

The README should be publishable as-is on day one of the repository.

## What You Do NOT Produce

- Architecture documentation (that's the System Design)
- Tutorials or guides (that's what hos are for)
- Marketing copy (factual and experiential, not promotional)
- New architectural decisions (those belong upstream)

## References

- `references/readme-structure.md` — The territory the README covers and what "done" looks like. **Read before drafting.**
- `references/project-type-guide.md` — How emphasis shifts by project type. Classification criteria and structural patterns for products, utilities, infrastructure, and libraries. **Read before drafting.**
