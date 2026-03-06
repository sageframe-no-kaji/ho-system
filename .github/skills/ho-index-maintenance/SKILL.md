---
name: ho-index-maintenance
description: 'Maintain the Ho System INDEX.md and document frontmatter. Use when: adding a new document to the repo index, removing a document from the index, updating a description in the index, updating frontmatter fields (status, version, tags, stage). Do NOT use for editing framework content, renaming files, or changing the layer hierarchy.'
argument-hint: 'What changed? (e.g. "added guides/new-guide.md" or "update status of 2.3 to stable")'
---

# Ho System Index Maintenance

## What This Skill Does

Keeps [INDEX.md](../../../INDEX.md) accurate and frontmatter consistent across all documents in the Ho System repository. It is **scoped to the indexing layer only** — it does not touch document content.

---

## Hard Limits

Stop and ask the human before doing any of these:

- Editing the content of any framework, structure, template, guide, or example document
- Renaming or moving files
- Changing the seven-layer hierarchy or layer definitions
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
type: foundation | structure | template | guide | example | seed | agent-task
stage: shu | ha | ri | any | n/a
status: draft | stable | deprecated
version: "major.minor"
tags: [ho-system, ...]         # always includes ho-system
---
```

**Type → Layer:**

| type | layer |
|------|-------|
| foundation | 1 (0 for index itself) |
| structure | 2 |
| template | 3 |
| guide | 4 |
| example | 5 |
| agent-task | 6 or 7 |
| seed | 7 |

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

Permitted without asking: `status`, `version`, `tags`, `stage`
Not permitted without asking: `id`, `type`, `title`

- `version`: increment minor for content updates, major for structural rewrites
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

---

## Reference

See [references/schema-reference.md](./references/schema-reference.md) for the full ID registry and all current assigned IDs.
