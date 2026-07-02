---
created: 2026-07-02
type: agent-task
status: ready
parent: private/audit/merge-decisions.md
project: ho-system
---

# Agent Task — Adopt the operating-discipline split; fix the env-setup skill's stale copy

## Goal

Make the **slim-rules + rationale split** the canonical repo-side form of the operating
discipline, and regenerate the env-setup skill's bundled copy from it — so a practitioner
onboarded by that skill receives the forward-only principle, which the current bundled
copy omits entirely. This is a file-sync and supersession task; no rule content is
authored or rewritten.

## Context

The architecture is decided in `private/audit/merge-decisions.md` **D6**: the slim-rules
+ rationale split (the live `~/.claude/modules/operating-discipline.md` + `operating-
discipline-rationale.md`) is canonical, having replaced the token-heavy monolith. This
task executes that decision against the repo. **The live `~/.claude/modules/` files are
the source of truth** — they are what actually runs and were deliberately built as the
split. The repo publishes what the practitioner runs; the env-setup skill installs the
repo's copy into a new practitioner's `~/.claude/`.

Current divergence:
- `~/.claude/modules/operating-discipline.md` — 185-line slim rules, **has** the
  Forward-only section. Canonical source.
- `~/.claude/modules/operating-discipline-rationale.md` — 152-line rationale. Canonical
  source. **No repo counterpart exists.**
- `practitioner/operating-discipline.md` — 299-line monolith (has forward-only, but is
  the superseded pre-split form).
- `skills/ho-setup-personal-environment-collaborator/references/operating-discipline.md`
  — 289-line stale snapshot, **missing the Forward-only section** (the bug).

Bidirectional supersession (merge-decisions D7) applies: the archived monolith points
forward to its replacement.

## Problem

The env-setup skill's bundled `references/operating-discipline.md` is an older snapshot
that predates the forward-only principle and does not contain it. Any practitioner set up
by the skill today receives an operating discipline missing the framework's newest
load-bearing principle. This is a correctness bug (stale-ref cleanup item A1; framework-
debt #3), not a documentation nicety.

## Files

- Read-only: `~/.claude/modules/operating-discipline.md` (canonical slim rules — source)
- Read-only: `~/.claude/modules/operating-discipline-rationale.md` (canonical rationale — source)
- Modify: `practitioner/operating-discipline.md` (replace monolith with the slim rules)
- Create: `practitioner/operating-discipline-rationale.md` (from the canonical rationale)
- Create: `private/archive/operating-discipline-monolith-pre-split.md` (the archived monolith)
- Modify: `skills/ho-setup-personal-environment-collaborator/references/operating-discipline.md` (regenerate from canonical slim)
- Create: `skills/ho-setup-personal-environment-collaborator/references/operating-discipline-rationale.md` (bundle the rationale)

## Required Changes

Execute in this order (step 1 must precede step 2 — do not lose the monolith).

1. **Archive the pre-split monolith.** Copy the current
   `practitioner/operating-discipline.md` verbatim to
   `private/archive/operating-discipline-monolith-pre-split.md`, then prepend a single
   note line at the very top (above existing content):

   `> Superseded 2026-07-02 by the slim-rules + rationale split (merge-decisions.md D6). Canonical now: practitioner/operating-discipline.md (slim) + practitioner/operating-discipline-rationale.md. Preserved as historical record.`

2. **Replace the repo canonical rules with the slim form.** Overwrite
   `practitioner/operating-discipline.md` with the exact content of
   `~/.claude/modules/operating-discipline.md`. **Verbatim** — no repo-specific edits, so
   the env-setup skill installs identical content.

3. **Create the repo canonical rationale.** Create
   `practitioner/operating-discipline-rationale.md` with the exact content of
   `~/.claude/modules/operating-discipline-rationale.md`.

4. **Regenerate the skill's bundled rules copy.** Overwrite
   `skills/ho-setup-personal-environment-collaborator/references/operating-discipline.md`
   with the same slim canonical content from step 2. This fixes the bug — the copy now
   contains the Forward-only section.

5. **Add the rationale to the skill bundle.** Create
   `skills/ho-setup-personal-environment-collaborator/references/operating-discipline-rationale.md`
   with the canonical rationale from step 3, so the skill installs the full split.

## Do Not

- Do not edit the *content* of the slim rules or the rationale while copying. They must
  match the live `~/.claude/modules/` sources byte-for-byte — this is a sync, not a
  rewrite. (The single archive note in step 1 is the only authored line, and it goes only
  in the archived file.)
- Do not touch `languages-swift.md` (live-vs-bundled drift is stale-ref item 22 — a
  separate task).
- Do not fix any other stale references or framework debt. Scope is the operating-
  discipline split only; the rest belongs to the mechanical-cleanup ho.
- Do not delete the monolith before it is archived (step 1 precedes step 2).

## Stop Condition

Before overwriting in step 2, diff the current repo monolith
(`practitioner/operating-discipline.md`) against the live slim source. Adopting the split
is meant to be a **reorganization** (rules slimmed, philosophy moved to the rationale) —
not a content change. If the diff shows the slim form **drops a rule or section present
in the monolith that is not preserved in the rationale** (genuine content loss, beyond
the forward-only reorg), **stop and surface the diff.** Whether to drop framework content
is the practitioner's decision, not the executor's.

## Acceptance

- [ ] `private/archive/operating-discipline-monolith-pre-split.md` exists, carries the
      pre-split monolith content, and has the supersession note at the top.
- [ ] `practitioner/operating-discipline.md` is byte-identical to
      `~/.claude/modules/operating-discipline.md`.
- [ ] `practitioner/operating-discipline-rationale.md` is byte-identical to
      `~/.claude/modules/operating-discipline-rationale.md`.
- [ ] `practitioner/operating-discipline.md` contains a "Forward-only" section.
- [ ] `skills/ho-setup-personal-environment-collaborator/references/operating-discipline.md`
      is byte-identical to the new `practitioner/operating-discipline.md` and contains a
      "Forward-only" section.
- [ ] `skills/ho-setup-personal-environment-collaborator/references/operating-discipline-rationale.md`
      exists and is byte-identical to the rationale.
- [ ] `git status --short` shows only the six intended paths changed/added.

## Verification

```bash
# The bug fix: forward-only now present in repo canonical AND skill copy
grep -c "Forward-only" practitioner/operating-discipline.md
grep -c "Forward-only" skills/ho-setup-personal-environment-collaborator/references/operating-discipline.md

# Clean adoption: repo canonical == live modules
diff ~/.claude/modules/operating-discipline.md practitioner/operating-discipline.md && echo "rules: identical"
diff ~/.claude/modules/operating-discipline-rationale.md practitioner/operating-discipline-rationale.md && echo "rationale: identical"

# Skill bundle regenerated from canonical
diff practitioner/operating-discipline.md skills/ho-setup-personal-environment-collaborator/references/operating-discipline.md && echo "skill rules: identical"
diff practitioner/operating-discipline-rationale.md skills/ho-setup-personal-environment-collaborator/references/operating-discipline-rationale.md && echo "skill rationale: identical"

# Monolith archived with its note
test -f private/archive/operating-discipline-monolith-pre-split.md && head -1 private/archive/operating-discipline-monolith-pre-split.md

# Nothing unintended touched
git status --short
```

## Commit

Single commit. No `Co-Authored-By` trailer, no generator tag. Message format:

```
practitioner: adopt slim-rules + rationale operating-discipline split

Supersede the 299-line monolith with the slim rules + separate rationale
(merge-decisions D6). Regenerate the env-setup skill's bundled copy from the
canonical split — it was a stale snapshot missing the forward-only section
entirely (stale-ref A1 / debt #3). Monolith archived to private/archive/.
```
