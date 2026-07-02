---
title: "Changelog"
type: foundation
status: living
tags: [ho-system, changelog, versioning]
---

# Changelog

The framework's version story: all notable **structural** changes to the Ho System —
new documents, full replacements, doctrine changes, renames, and removals. Ordered
newest-first. Prose polish, typo fixes, and broken-link repairs are out of scope.

Each entry names what changed, carries a `kind:` tag, and cites the driving decision
where one exists (`merge-decisions D1`, a ho, etc.).

`kind:` — `added` · `replaced` · `changed` · `renamed` · `removed` · `deprecated`

---

## 2026-07-02 — Fable-audit merge (ho-02, canonical-layer reconciliation)

### Adopt CHANGELOG as the version story; drop per-doc `version:` fields — kind: changed

This file becomes the framework's single version story (merge-decisions **D13**).
Per-document `version:` frontmatter fields are removed as half-maintained and redundant
with this changelog. Documents touched from this point drop the field; the repo-wide
sweep of untouched documents is deferred to the mechanical-cleanup ho (tracked as
IDEA-006). No `1.0 → 2.0`-style version bumps are groomed going forward — a structural
change is recorded here instead.
