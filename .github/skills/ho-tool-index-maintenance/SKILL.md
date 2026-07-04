---
name: ho-tool-index-maintenance
description: 'Maintain the Ho System INDEX.md and document frontmatter. Use when: adding a new document to the repo index, removing a document from the index, updating a description in the index, updating frontmatter fields (status, tags, stage). Do NOT use for editing framework content, renaming files, or changing the layer hierarchy.'
argument-hint: 'What changed? (e.g. "added guides/new-guide.md" or "update status of 2.3 to stable")'
---

# Ho Tool Index Maintenance

## What This Skill Does

Keeps [INDEX.md](../../../INDEX.md) accurate and frontmatter consistent across all documents in the Ho System repository. It is **scoped to the indexing layer only** — it does not touch document content.

---

## Hard Limits

Stop and ask the human before doing any of these:

- Editing the content of any framework, structure, template, guide, or example document
- Renaming or moving files
- Changing the layer hierarchy (0–9; 7 is retired) or layer definitions
- Changing the frontmatter schema (adding/removing fields)
- Changing the `type` taxonomy values
- Renumbering IDs wholesale
- Creating new documents outside `skills/` or `agent-tasks/`

---

## Frontmatter Schema

```yaml
---
id: "{layer}.{sequence}"       # e.g. "2.3"
title: "Human-readable title"
type: foundation | structure | template | guide | example | seed | agent-task | practitioner | skill
stage: shu | ha | ri | any | n/a
status: draft | stable | deprecated
tags: [ho-system, ...]         # always includes ho-system
---
```

**Type → Layer:**

| type | layer |
|------|-------|
| foundation | 1 (0 for repo meta: index, changelog, contributing) |
| structure | 2 |
| template | 3 |
| guide | 4 |
| example | 5 |
| agent-task | 6 |
| seed | none — seeds live gitignored in `ho-process/` or in `practitioner/archive/` (layer 7 retired) |
| practitioner | 8 |
| skill | 9 (the catalog `skills/ho-skill-overview.md`; individual skills are not indexed) |

---

## Procedures

### Add a document

1. Read the file's `type` frontmatter to determine its layer
2. Assign the next available sequence number in that layer (check INDEX.md for the current highest)
3. Update the file's `id` field to `"{layer}.{sequence}"`
4. Add a row to the correct section table in INDEX.md: `| {id} | [{title}]({relative-path}) | {one-line description} |`
5. Insert in logical reading order — not just appended
6. Read the first paragraph of the document to verify the description is accurate

### Remove a document

1. Remove the row from INDEX.md
2. Leave the ID gap — do not renumber

### Update a description

1. Read the first 20 lines of the document
2. Write a factual one-line description — no adjectives, no quality claims
3. Replace only the description cell in the INDEX.md row

### Update frontmatter fields

Permitted without asking: `status`, `tags`, `stage`
Not permitted without asking: `id`, `type`, `title`

- `status`: draft → stable → deprecated (one direction only unless instructed)

---

## Quality Checks (run after every edit)

1. Every INDEX.md link resolves to an existing file
2. No two documents share the same `id`
3. All `type` values are from the defined taxonomy
4. Section numbers in INDEX.md match the layer prefix of documents within them
5. The "Entry points by intent" block at the top of INDEX.md still points to valid IDs

---

## When Uncertain

Read the document's first section, then check the type → layer table. If still unclear — ask. A wrong entry is worse than a missing one.

INDEX.md is the sole ID authority — derive current and next-available IDs from it directly. (A separate schema-reference cache existed and drifted; retired 2026-07-03, ho-09.)
