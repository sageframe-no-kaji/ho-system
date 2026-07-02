---
id: "3.9"
title: "Design Ho Template"
type: template
stage: ri
status: draft
tags: [ho-system, template, design, ho]
---

# Design Ho Template

For the in-chain wrapper cases of the visual-design modality (see Design Work,
framework 2.11): **tuner landings**, and **design-session batches that DO want a ho
document**. The design *sessions* themselves usually run outside the chain with no ho
document (their record is the register and the session artifacts); this template is for
the numbered hos that consume or land those decisions inside the chain, where
forward-only and commit-traceability apply.

Landings are `shape: ri` (problem → solution → changes → results fits "move sliders,
land values, lock commit"); session-batch wrappers may be `shape: ha`. The skeleton is
complete; the section prose fills in per project.

---

```markdown
---
created:
status: draft
type: ho-document
project:
ho: ""
kamae: 5
shape: ri            # landings are ri; session-batch wrappers may be ha
builds-on:
  - design/visual-register.html
  - <the design sessions this ho consumes or produces>
---

# ho-NN — <question or landing being resolved>

<3–5 sentence narrative: which design questions this session resolves or lands,
and what downstream work unblocks.>

## Questions in scope
<one per line, in constraint order; later questions reference earlier winners>

## Constraints (hard)
<palette, reproducibility requirement, medium, out of scope — the pre-written
session prompt's constraint block, or the tuner list for a landing>

## Frozen at session end
<parameter: value — numbers, not descriptions. These update the register.>

## Parked
<one line each; items belonging to later questions>

## Landing record (tuner landings only)
<values landed, against what corpus/scale, commit that locks them>

## Register propagations
<any frozen-earlier values this session revised, each with its named reason>
```

---

_This template is part of the Ho System framework. It is the in-chain companion to
[[design-work|Design Work]] (framework/structure/design-work.md); the eight-step
modality and the living-register discipline live there._
