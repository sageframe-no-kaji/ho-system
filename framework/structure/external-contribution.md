---
id: "2.12"
title: "External-Project Contribution"
type: structure
stage: n/a
status: draft
tags: [ho-system, external, contribution, pull-request]
---

# External-Project Contribution

## Running Ho Against Someone Else's Codebase

A practitioner contributing to an upstream project keeps the Ho System's *session
discipline* and surrenders its *project authority*. The host repo owns the
architecture, the verification stack, the commit style, and the definition of done.
The Ho System owns how the practitioner thinks, scopes, delegates, and records. This
document says which is which, and how to keep Ho artifacts out of the upstream PR.

---

## 1. What compresses: the Kamae chain, usually to zero

An external contribution does not get a seed, system design, README, or overview.
**The upstream issue substitutes for all four** — it states the problem (seed), the
host codebase states the architecture (system design), the host README states scope,
and the PR's own bounded goal is the sequence. The supacode case ran two hos against
GitHub issue #442 with no Kamae documents; the ho frontmatter carries the
substitution:

```yaml
branch: osc11-per-pane-backgrounds
issue: https://github.com/supabitapp/supacode/issues/442
```

If a contribution grows past what an issue can anchor — multi-PR arcs, architectural
proposals — write the missing thinking as an RFC *in the host project's format*, not
as a Kamae document in their tree.

## 2. What survives: the session discipline, intact

From the case, kept without modification:

- **Document shape.** Frontmatter, Context, in-scope/out-of-scope, Think, Execute,
  Acceptance, Reflect. The ho-02 recovered from supacode is structurally
  indistinguishable from a home-project ho.
- **Planning before execution.** Think resolves the load-bearing facts before the
  agent prompt exists (ho-02's Think carries three "facts the executing agent must
  hold").
- **Escalation.** An explicit Escalation section: conditions under which the agent
  stops and surfaces rather than adapting (architectural assumption fails manual
  verification; input methods regress; a C accessor doesn't behave as documented).
- **Forbidden moves.** The out-of-scope list hardened into named prohibitions,
  including host-specific ones ("No `print()` or `os.Logger`. Use `SupaLogger`").

## 3. What is renegotiated, term by term

| Term | Home-project rule | External rule | Case evidence |
|---|---|---|---|
| Verification stack | Practitioner's stack (ruff/mypy/pytest, 90% floor) | **Inherited from the host, run verbatim, after every step** | `make format && make lint && make test && make build-app`, "Fix anything red before moving on" |
| Coverage floor | 90%, pre-commit enforced | **Dropped.** The host's test conventions govern; new invariants get tests, no floor is asserted | ho-02 adds bridge/view tests for its invariants, no coverage stanza |
| Commit style | `ho-NN:` prefix, one commit per AT | **Matched to the host log.** Read their recent history and imitate it | "message format the recent log uses (`Verb subject under 72 chars`, no scope prefix)"; no Co-Authored-By trailers in PR commits |
| Verification gates | Automated (hooks, CI) | **Per-step manual gates where the host has no automation** — named checks, required not optional | ho-02 Step 2: five manual checks, "This is the gate for whether the split is viable" |
| Staging | Practitioner discretion | **Strict:** never `git add .` / `-A`; stage only files the ho names | verbatim forbidden move |
| Forward-only | Closed hos stay closed | **Enforced harder:** editing the prior ho is the *first listed* forbidden move; supersession recorded in the successor's frontmatter | `supersedes: ho-01-... (Decision 2 host-layer approach; Decision 3 opacity proxy; …)` |

The forward-only evidence deserves its line: ho-01's Think was wrong on three counts
(layer ordering, OSC kind clobber, blur proxy). ho-02 exists purely to name and
correct them, with decision-precise `supersedes:`. Under the social pressure of
someone else's repo — where rewriting your own branch history is normal — the
practice held the record instead. That is the pattern working where it costs
something.

## 4. Workarounds are the host's, not yours

When the host environment breaks, follow the host's documented workaround; do not
invent one inside the ho ("If `make build-app` fails for a Tahoe-SDK reason, follow
`CLAUDE.md`'s 'Building on macOS 26.4+' section — do not invent a workaround"). A
missing workaround is an escalation, and possibly an upstream issue — which is a
contribution in itself.

## 5. Agent-file compatibility

Host projects standardize on `AGENTS.md` or their own agent-instruction file. The
compatibility move from the case: `CLAUDE.md` as a symlink to `AGENTS.md`, so Claude
Code and other agent hosts read the same file with zero divergence risk. Applies in
the practitioner's fork; never add agent files to the upstream PR.

## 6. The ho-process privacy problem — doctrine

**The problem.** Ho artifacts are the practitioner's working record, not PR content.
In the case, `ho-process/` was committed in-tree (with a `.gitignore` entry added,
then reverted, then the docs tracked deliberately), and keeping it out of the PR
ultimately required `git filter-branch` — history rewriting, the most fragile tool in
git, on a branch destined for upstream review. The add-revert-track-filter sequence
is visible in the host repo's reflog and is the friction this doctrine removes.

**The rule: Ho artifacts never enter the contribution repo's object database.** Not
"gitignored" — *never committed*, because anything committed on the PR branch is one
`rebase` mistake away from upstream history, and scrubbing requires history rewriting
after the fact.

**The mechanism — sidecar directory:** keep `ho-process/` in a sibling vault
directory outside the fork's working tree:

```
~/Vaults/sageframe-no-kaji-dev/
├── supacode/                      # the fork — upstream's tree, nothing else
└── supacode-ho/                   # the sidecar — practitioner's record
    └── ho-process/
        ├── hos/
        └── agent-tasks/
```

- The sidecar is its own git repo (the record deserves version control) on the
  practitioner's account, private.
- Hos reference the fork by path; the executing agent is pointed at the fork as
  working directory and the sidecar ho as its spec. Nothing in the fork points back
  (a gitignored local `CLAUDE.local.md` may carry the pointer for the agent's
  benefit — gitignore protects *convenience files*; it is not the privacy mechanism
  for the record itself).
- The fork's `.gitignore` is upstream's file; adding personal entries to it is
  itself a diff pollution. Use `.git/info/exclude` for any local-only convenience
  paths — it lives outside the tree entirely.

**Why not in-tree-but-gitignored:** the case tried the adjacent version and produced
the filter-branch cleanup. Gitignore does not protect against `git add -f`, does not
protect files committed before the entry existed, and leaves the practitioner one
habit-slip from publishing their private record. The sidecar makes the failure mode
structurally impossible.

**Trade-off named honestly:** the sidecar breaks the "ho documents are committed
alongside the code they describe" invariant (ho-structure §6). For external work
that invariant is the *bug* — the whole point is that the record and the
contribution have different owners and different audiences. Traceability is
preserved by commit hashes recorded in the sidecar hos, exactly as ri-shape hos
already do.

## 7. What goes upstream

The PR contains: the code, the tests, commit messages in the host's style, and a PR
description written in the host's register (linking the issue, not the ho). The ho
record stays in the sidecar. If the host community would benefit from the reasoning,
distill it into the PR description or a comment — deliberately, as publication, not
as leakage.

---

_This document is part of the Ho System framework. Case study: the supacode OSC 11
contribution (two hos, one supersession, one filter-branch lesson). The discipline
travels; the authority does not._
