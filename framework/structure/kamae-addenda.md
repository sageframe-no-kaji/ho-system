---
id: "2.10"
title: "Kamae Addenda"
type: structure
stage: n/a
status: stable
tags: [ho-system, kamae, decisions, forward-only]
---

# Kamae Addenda (kamae-N.M)

## Mid-Build Architectural Decisions Without Rewriting History

A committed Kamae document sometimes turns out to be wrong — not because the framing
was sloppy, but because the build surfaced evidence the framing couldn't have had. The
forward-only principle forbids editing the committed document to match the new
understanding. The Kamae addendum is the sanctioned response: a decimal-suffixed
decision document that supersedes a *named part* of the original while the original
stays frozen as the record of what was decided when.

This is ho-numbering's insertion logic (ho-N.5) applied to the thinking chain, and
ho-structure §3.5's forward-only rule applied to architecture.

---

## 1. When to issue an addendum

Issue a kamae-N.M when all three hold:

1. **A committed Kamae decision is being reopened** — not refined, reopened. The
   sharibako test case: kamae-2 committed "no runtime injection" as an architectural
   boundary; a threat-model input that didn't exist in the original analysis (AI
   agents as workspace file-readers) justified reopening it.
2. **The change is architectural, not scope-cosmetic.** Downstream hos will build
   differently because of it. A README wording change or an overview resequencing is
   living-document maintenance, not an addendum.
3. **The original document's other commitments stand.** An addendum supersedes
   *part* of its parent. If the reopening invalidates the document wholesale, the
   project is re-entering Kamae, not amending it — that is a new seed conversation,
   not an addendum.

The addendum records the *reopening justification* as first-class content. Sharibako
kamae-2.1: "The threat-model gap alone justifies the reopening; no other argument is
required." An addendum that doesn't say what changed in the world is just a rewrite
with extra steps.

## 2. Anatomy

Filename: `ho-process/kamae-<N>.<M>-<project>-<slug>.md`, where N is the parent Kamae
number and M increments per addendum (2.1, 2.2, …).

Frontmatter (from the live instances):

```yaml
---
created: 2026-07-01
status: decided
type: decision
project: sharibako
stage: kamae-2.1
kamae-chain: seed → system-design → **injection-decision** → readme → ho-overview
supersedes: kamae-2 §7 (partial — the "no runtime injection" architectural commitment)
builds-on: kamae-1-sharibako-seed, kamae-2-sharibako-system-design
next: reflected in kamae-1 seed (parti), kamae-4 (adds ho-04.5), README, SECURITY.md
---
```

Field notes:

- **`supersedes:` is section-precise.** Not "supersedes kamae-2" — "kamae-2 §7
  (partial — the specific line)." The reader must be able to tell exactly which
  commitments fell and, by implication, which stand. The addendum body repeats this
  as prose: "All other Kamae 2 commitments … stand unchanged."
- **`kamae-chain:` shows the insertion position** — where the addendum sits in the
  reading order, bolded.
- **`next:` names the propagation set** — every downstream document the decision was
  reflected into (see §4).

Body sections (both instances follow this arc):

1. **What the parent originally decided** — quoted, not paraphrased.
2. **What changed** — the new evidence or input that justifies reopening.
3. **The decision** — the revised commitment, stated as precisely as the original.
4. **Why not the alternative shapes** — declined alternatives with reasons preserved
   for future readers. (Both sharibako addenda give this a full section; the declined
   reasoning is half the document's value — it prevents the next reader from
   re-proposing the rejected shape.)
5. **Implementation shape** — enough concretion that the resolving ho can be
   specified (signatures, behavioral contracts), stopping short of the ho's own
   territory.
6. **What stays the same** — the explicit not-superseded list.
7. **Reflected in** — the propagation record (§4).
8. **Open items handed to downstream hos** — which ho owns each loose end.

Closing line, verbatim from the instances and recommended as convention:
_"Decision committed. Downstream documents updated in the same session. This file is
a permanent record; not to be edited except for typographical fixes."_

## 3. How the readership knows what supersedes what

Three links, all mandatory, so the chain is navigable from any entry point:

1. **The addendum → parent:** the `supersedes:` field plus quoted original text.
2. **The parent → addendum:** a reader's note at the top of the frozen parent naming
   each addendum and one line on what it supersedes. Sharibako kamae-2 carries
   exactly this (`superseded-in-part-by:` frontmatter list + a "Reader's note" block),
   with the body "preserved as-authored to record what was decided when." The parent
   edit is one of the narrow legitimate touches to a frozen document — it changes
   navigation, not what the document said.
3. **Downstream consumers → addendum:** hos that build on the revised decision cite
   the addendum in `builds-on:` alongside the parent (sharibako ho-03 cites kamae-2,
   2.1, and 2.2). A ho that cites only kamae-2 is declaring it predates — or is
   unaware of — the revision; tooling can flag the latter.

These three links are this document's instance of the framework-wide
**bidirectional-supersession** rule (ho-structure §3.5, merge-decisions D7): whichever
document a reader lands on — parent, addendum, or downstream ho — they can reach the
others. The parent → addendum reader's note (link 2) is the backward half the general
rule makes mandatory for every living document; it is not a courtesy specific to
addenda, and because the parent is frozen, adding it is one of the narrow legitimate
touches to a frozen document.

## 4. Same-session propagation

The addendum is not done when the decision is written. Both instances updated every
downstream document *in the same session* — seed (living parti gets a revision note),
kamae-4 (new or updated ho entries), README, and public docs — and recorded the full
set in `Reflected in`. The discipline: **a decision the downstream documents don't
reflect is not yet a decision the project has made.** Leaving propagation for later
reintroduces exactly the drift the encoded-environment thesis exists to prevent.

Propagation respects each target's mutability regime (see the artifact-type
registry §6): the seed and overview are edited in place with revision notes; a frozen
system design gets only the reader's-note pointer; new hos are added to the overview
rather than rewriting closed ones.

## 5. Addenda vs the other change mechanisms

| Situation | Mechanism |
|---|---|
| Committed architectural decision reopened by new evidence | **Kamae addendum** (this document) |
| Closed ho's work wrong or overtaken | New ho with `supersedes:` (ho-structure §3.5) |
| Overview needs resequencing, splits, insertions | Edit the overview in place (living document) |
| Seed's intent shifts deliberately | Revise seed in place with dated revision note |
| Whole system design invalidated | Re-enter Kamae; new system design supersedes old wholesale |

---

_This document is part of the Ho System framework. It is the forward-only principle
applied to the Kamae chain: the original stays frozen, the addendum records what
changed and why, and the supersession is part of the record, not hidden inside an
amended document._
