---
id: "skill-index"
title: "Ho Index Maintenance Skill"
type: foundation
stage: n/a
status: stable
version: "1.0"
tags: [ho-system, skill, index, ai-collaboration]
---

# Skill: Ho System Index Maintenance

## Purpose

This skill defines how an AI assistant should maintain [INDEX.md](../INDEX.md) and the frontmatter system for the Ho System repository. It is intentionally **narrow** — it covers only the indexing layer, not the content of any document.

---

## Hard Boundaries (Do Not Cross Without Human Instruction)

This skill does **not** authorize:

- Editing the content of any framework, structure, template, guide, or example document
- Renaming, moving, or deleting files
- Changing the seven-layer hierarchy (layers 1–7 and their definitions)
- Changing the frontmatter schema (adding/removing fields)
- Changing the `type` taxonomy values
- Renumbering IDs wholesale (e.g. "I'll reorder the whole layer")
- Creating new documents outside the `skills/` folder

If a request implies any of the above, **stop and ask the human** before proceeding.

---

## The Schema (Reference)

Every non-README `.md` file in the repository carries this frontmatter block:

```yaml
---
id: "{layer}.{sequence}"       # e.g. "2.3" — layer 2, third document
title: "Human-readable title"
type: foundation | structure | template | guide | example | seed | agent-task
stage: shu | ha | ri | any | n/a
status: draft | stable | deprecated
version: "major.minor"         # e.g. "1.0"
tags: [ho-system, ...]         # always includes ho-system
---
```

**ID rules:**
- Layer prefix matches the INDEX.md section number (1–7, plus 0 for the index itself)
- Sequence is assigned in reading/logical order within the layer
- IDs are unique across the entire repository
- Gaps are allowed (e.g. 2.1, 2.2, 2.4 — if 2.3 was removed)
- Do not reuse a retired ID

**Type → Layer mapping:**

| type | lives in layer |
|------|---------------|
| foundation | 1 (or 0 for index) |
| structure | 2 |
| template | 3 |
| guide | 4 |
| example | 5 |
| agent-task | 6 (task specs) or 7 (working docs) |
| seed | 7 |

---

## Permitted Operations

### 1. Add a new document to the index

**Trigger:** A new `.md` file has been created in the repository that isn't in INDEX.md.

**Steps:**
1. Determine the correct layer from the file's `type` frontmatter field
2. Assign the next available sequence number within that layer
3. Update the file's frontmatter `id` field to match
4. Add a row to the correct section table in INDEX.md
5. Keep the row format: `| {id} | [{title}]({relative-path}) | {one-line description} |`
6. Place the row in logical reading order, not just appended at the end

**Check before finishing:** Does the new row's description match what the document actually does? Read the first paragraph of the document to verify.

### 2. Remove a document from the index

**Trigger:** A file has been deleted from the repository.

**Steps:**
1. Remove the corresponding row from INDEX.md
2. Do not renumber remaining IDs — leave the gap
3. Note the retired ID in a comment only if instructed to do so

### 3. Update a document's description in the index

**Trigger:** The content of a document has changed significantly and its INDEX.md description is now inaccurate.

**Steps:**
1. Read the first 20 lines of the document to understand the current scope
2. Write a new one-line description — factual, no adjectives, no claims about quality
3. Replace only the description cell in the INDEX.md row

### 4. Update a document's frontmatter

**Trigger:** A document's status, version, stage, or tags need updating.

**Permitted changes:**
- `status`: draft → stable → deprecated
- `version`: increment minor for content changes, major for structural rewrites
- `stage`: only change if the document's intended audience has genuinely changed
- `tags`: add or remove tags that reflect actual content

**Not permitted without human instruction:** changing `id`, `type`, or `title`.

---

## Quality Checks (Run After Any Edit)

1. Every row in INDEX.md links to a file that actually exists
2. No two documents share the same `id`
3. All `type` values are from the defined taxonomy
4. INDEX.md section numbers match the layer IDs of documents within them
5. The "Entry points by intent" section at the top of INDEX.md still points to valid IDs

---

## What to Do When Uncertain

If the correct layer, ID, or description for a document isn't obvious:

1. Read the document's first section
2. Check the type → layer mapping above
3. If still unclear, **ask the human** — do not guess at type or placement

The index is a navigation tool. A wrong entry is worse than a missing one.
