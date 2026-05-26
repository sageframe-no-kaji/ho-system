---
created: 2026-05-25
type: agent-task
status: ready
project: ho-system
---

**Goal**

Integrate the new `ho-task-decomposition.md` structural document into the Ho System framework repo. The document is already written and ready to place. After this task, a reader encountering the ho-task relationship in any framework document can follow a trail to the canonical specification; the existing documents that previously treated decomposition ambiguously now defer to this one.

**Context**

The Ho System framework currently presents inline execution as the default for ho documents, with agent task extraction described as one option among several. Mature practice has converged on the opposite: extraction is canonical, inline execution is the exception reserved for trivial moves. The new document `ho-task-decomposition.md` names this pattern as canonical, defines the three relationship types (constitutive children, in-session delegations, standalone tasks), specifies the parent-child binding via frontmatter, and uses Shodō ho-02 as the worked example.

This task integrates that document into the framework's reference layer. It does not rewrite the documents that currently carry the older framing — that work is deferred to a subsequent ho. This task is the placement and cross-referencing pass only.

**Files**

- Create: `framework/structure/ho-task-decomposition.md` (content provided separately; place verbatim)
- Modify: `INDEX.md` (add row to Structure section)
- Modify: `framework/the-ho-system.md` (add to framework reading table and reading order)
- Modify: `framework/templates/agent-task-spec.md` (add reference to ho-task-decomposition in the Relationship to Ho Documents section)
- Modify: `framework/templates/template-selection-guide.md` (add reference in the related-documents footer)
- Modify: `framework/templates/ha-ho-template.md` (add reference where the template addresses Execute phase decomposition)
- Modify: `framework/templates/ri-ho-template.md` (add reference where the template addresses the ri-vs-agent-task choice)
- Modify: `framework/structure/ho-structure.md` (add to the Related Framework Documents list at the bottom)
- Modify: `.github/skills/ho-index-maintenance/references/schema-reference.md` (add row 2.8 to the ID registry; update "Next available per layer" for layer 2 from 2.8 to 2.9)

**Required Changes**

1. **Place the new document** at `framework/structure/ho-task-decomposition.md`. The content is provided as a separate file; copy verbatim, do not edit. The document's frontmatter already declares `id: "2.8"`, which slots it after `verification-practices.md` (2.7) in the Structure layer.

2. **Update `INDEX.md`.** Add a row to the Structure section table, immediately after the verification-practices row:

   ```
   | 2.8 | [Ho-Task Decomposition](framework/structure/ho-task-decomposition.md) | How hos and agent tasks compose one architectural thought |
   ```

3. **Update `framework/the-ho-system.md`.** In the framework reading table (the "Read the Framework" or equivalent table), add a row for ho-task-decomposition. Position it after verification-practices to match its placement in the Structure section. Use the wiki-link format consistent with surrounding rows:

   ```
   | [[ho-task-decomposition|Ho-Task Decomposition]] (framework/structure/ho-task-decomposition.md) | How hos and agent tasks compose one architectural thought. The parent-child relationship, the three relationship types, and when to extract. |
   ```

   In the Reading Order section, add ho-task-decomposition to the "If you're writing hos for someone else" path (or equivalent path that mentions agent tasks). One-line mention is sufficient — defer to the document itself.

4. **Update `framework/templates/agent-task-spec.md`.** In the "Relationship to Ho Documents" section, add a one-paragraph reference at the top of the section:

   ```
   The canonical specification for how agent tasks relate to ho documents is in [[ho-task-decomposition|Ho-Task Decomposition]] (framework/structure/ho-task-decomposition.md). That document names three relationship types (constitutive children, in-session delegations, standalone tasks), specifies the parent-child binding, and establishes extraction as canonical. The notes below describe how each type manifests at each shu-ha-ri stage.
   ```

   Do not edit the existing stage-by-stage paragraphs below this addition. They remain valid as stage-specific notes; the new paragraph establishes the overall reference.

5. **Update `framework/templates/template-selection-guide.md`.** In the related-documents footer at the bottom of the file, add ho-task-decomposition to the Structure references:

   ```
   _Structure: [[shu-ha-ri|Shu-Ha-Ri Progression]] (framework/structure/shu-ha-ri.md) · [[kamae-project-framing|Kamae: Project Framing]] (framework/structure/kamae-project-framing.md) · [[ho-task-decomposition|Ho-Task Decomposition]] (framework/structure/ho-task-decomposition.md)_
   ```

6. **Update `framework/templates/ha-ho-template.md`.** Locate the section that describes the Execute phase (Phase 2). Add a single sentence at the appropriate point — likely near the beginning of the Execute section's author notes — pointing to the decomposition document:

   ```
   When Execute decomposes into agent tasks, see [[ho-task-decomposition|Ho-Task Decomposition]] (framework/structure/ho-task-decomposition.md) for the canonical pattern: Execute functions as a structural index over the child tasks, not as a section where the practitioner writes code inline.
   ```

   Do not restructure the template. One reference, placed where the topic naturally arises.

7. **Update `framework/templates/ri-ho-template.md`.** Locate the section that describes the choice between a ri ho and a standalone agent task (likely the "When to Use Ri vs. Agent Task" table or its surrounding prose). Add a one-line reference:

   ```
   See [[ho-task-decomposition|Ho-Task Decomposition]] (framework/structure/ho-task-decomposition.md) §7 for the test that distinguishes a ri ho from a standalone task.
   ```

8. **Update `framework/structure/ho-structure.md`.** In the Related Framework Documents list at the bottom of the file, add ho-task-decomposition:

   ```
   - [[ho-task-decomposition|Ho-Task Decomposition]] (framework/structure/ho-task-decomposition.md) — How hos and agent tasks compose one architectural thought
   ```

   Insert in logical reading order (after agent-task-spec or template-selection-guide, depending on the existing order).

9. **Update `.github/skills/ho-index-maintenance/references/schema-reference.md`.** Add a row to the ID registry table for 2.8:

   ```
   | 2.8 | framework/structure/ho-task-decomposition.md | structure | any | draft |
   ```

   Insert in the correct position (after the 2.7 verification-practices row, before the 3.1 template-selection-guide row).

   Then update the "Next available per layer" table — change the layer 2 next-available from `2.8` to `2.9`.

**Acceptance**

- [ ] `framework/structure/ho-task-decomposition.md` exists, content matches the provided source verbatim.
- [ ] `INDEX.md` has a row for 2.8 in the Structure section table, in the correct position (after 2.7).
- [ ] `framework/the-ho-system.md` references ho-task-decomposition in both the framework reading table and the Reading Order section.
- [ ] `framework/templates/agent-task-spec.md` has the new reference paragraph at the top of "Relationship to Ho Documents"; existing paragraphs in that section are unchanged.
- [ ] `framework/templates/template-selection-guide.md` footer lists ho-task-decomposition in the Structure references.
- [ ] `framework/templates/ha-ho-template.md` has a one-sentence reference in the Execute phase section.
- [ ] `framework/templates/ri-ho-template.md` has a one-line reference in the ri-vs-agent-task section.
- [ ] `framework/structure/ho-structure.md` Related Framework Documents list includes ho-task-decomposition.
- [ ] `.github/skills/ho-index-maintenance/references/schema-reference.md` ID registry includes 2.8; "Next available per layer" for layer 2 is updated to 2.9.
- [ ] No existing document has had its meaning, structure, or section ordering changed — only the additions specified above.
- [ ] All new wiki-links resolve to the actual file at `framework/structure/ho-task-decomposition.md`.

**Verification**

```bash
# From repo root.

# 1. New document is in place and matches source.
test -f framework/structure/ho-task-decomposition.md
head -10 framework/structure/ho-task-decomposition.md | grep -q 'id: "2.8"'
head -10 framework/structure/ho-task-decomposition.md | grep -q 'title: "Ho-Task Decomposition"'

# 2. INDEX.md row exists.
grep -F '[Ho-Task Decomposition](framework/structure/ho-task-decomposition.md)' INDEX.md

# 3. the-ho-system.md references the new doc.
grep -F 'ho-task-decomposition' framework/the-ho-system.md

# 4. Agent task spec references the new doc.
grep -F 'ho-task-decomposition' framework/templates/agent-task-spec.md

# 5. Template selection guide footer references the new doc.
grep -F 'ho-task-decomposition' framework/templates/template-selection-guide.md

# 6. Ha template references the new doc.
grep -F 'ho-task-decomposition' framework/templates/ha-ho-template.md

# 7. Ri template references the new doc.
grep -F 'ho-task-decomposition' framework/templates/ri-ho-template.md

# 8. Ho structure related-docs references the new doc.
grep -F 'ho-task-decomposition' framework/structure/ho-structure.md

# 9. Schema reference registry includes 2.8 and layer-2 next is 2.9.
grep -F '| 2.8 | framework/structure/ho-task-decomposition.md' .github/skills/ho-index-maintenance/references/schema-reference.md
grep -E '^\| 2 \| 2\.9' .github/skills/ho-index-maintenance/references/schema-reference.md

# 10. Count cross-references — at least 7 files should reference the new doc.
grep -lrF 'ho-task-decomposition' --include='*.md' . | wc -l
# Expected: 8 or more (the doc itself plus seven referencing files plus schema-reference)
```

**Do Not**

- Do not edit the content of `ho-task-decomposition.md`. Place it verbatim.
- Do not rewrite the existing documents to reflect the new framing. This task is placement and cross-referencing only. The semantic rewrite of `agent-task-spec.md`, `ha-ho-template.md`, and the others is a subsequent ho with its own decisions about voice, scope, and tradeoffs.
- Do not change the section ordering or naming conventions of any existing document.
- Do not change the frontmatter schema or the `type` taxonomy values. The document declares `type: structure` and `id: "2.8"`; both fit the existing schema.
- Do not renumber any existing IDs.

**Commit**

Single commit. Message format:

```
docs(framework): integrate ho-task-decomposition into the structural layer

Place framework/structure/ho-task-decomposition.md (2.8) and add references
from INDEX.md, the-ho-system.md, agent-task-spec.md, template-selection-guide.md,
ha-ho-template.md, ri-ho-template.md, ho-structure.md, and the index-maintenance
schema reference.

This is placement and cross-referencing only. Semantic rewrites of the documents
currently carrying the older "extraction is optional" framing are deferred to a
subsequent ho.
```
