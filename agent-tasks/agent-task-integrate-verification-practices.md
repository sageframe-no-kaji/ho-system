# TASK: INTEGRATE VERIFICATION PRACTICES INTO HO SYSTEM FRAMEWORK

## GOAL

Integrate the new `verification-practices.md` document into the Ho System framework repo so that verification is treated as a first-class structural element — referenced from all relevant documents, reflected in all templates, and visible in the framework's navigation structure.

After this task, a reader encountering verification in ANY Ho System document should be able to follow a trail to the full verification practices document. No template should treat tests or verification as a checklist afterthought.

## PLACEMENT

The document goes at: `framework/structure/verification-practices.md`

This places it alongside the other structural documents:
```
framework/structure/
├── ho-structure.md
├── project-arc.md
├── tiered-understanding.md
├── shu-ha-ri.md
├── devlog.md
├── kamae-project-framing.md
└── verification-practices.md    ← NEW
```

## DO NOT CHANGE

- The content of `verification-practices.md` itself (it is already written)
- The structure or meaning of any existing document — only ADD references and strengthen existing verification language
- Template section ordering or naming conventions
- The shu-ha-ri stage definitions or transition criteria

## REQUIRED CHANGES

### 1. `the-ho-system.md` (main entry point)

Add `verification-practices.md` to the "Read the Framework" table, positioned after `devlog.md`:

```
| [[verification-practices|Verification Practices]] (framework/structure/verification-practices.md) | The four-layer verification stack. Test coverage, self-review, cross-agent verification, human review. |
```

Add to the Reading Order section:
- Under "If you're starting a project": mention verification practices alongside Ho Structure
- Under "If you're writing hos for someone else": include verification practices as essential reading

### 2. `framework/structure/ho-structure.md`

In the section on the five invariants (or equivalent foundational properties of a ho), add or strengthen the principle that verification is structural. If there is a section on deliverables or completion, add a note that a deliverable is not complete until verified — and link to verification-practices.md for the full stack.

### 3. `framework/structure/shu-ha-ri.md`

In each stage section (2.1 Shu, 2.2 Ha, 2.3 Ri), add a brief paragraph or note on how verification practices manifest at that stage. Reference the verification-practices.md stage tables. Specifically:

- **Shu (§2.1):** Add under "Ho Characteristics" that verification is built into the template structure — test commands in verify steps, tests in completion checklists. Link to Layer 1 and Layer 2 in verification-practices.md.
- **Ha (§2.2):** Add that the practitioner develops their own verification judgment — choosing which layers to apply based on task risk. The Agent Task Log tracks verification decisions. Link to Layer 2 and Layer 3.
- **Ri (§2.3):** Add that the full verification stack (including cross-agent verification) becomes standard workflow. The danger at ri is efficiency erosion — skipping verification on "simple" tasks. Link to the full workflow diagram in verification-practices.md.

In the transition signals section (§3):
- **Shu → Ha:** Add that the learner begins questioning whether the template's built-in verification is sufficient, and starts designing their own verification approaches.
- **Ha → Ri:** Add that cross-agent verification becomes habitual, not occasional.

### 4. `framework/structure/devlog.md`

Add a section (or extend the existing structure) on verification records in devlogs. What to capture: which verification layers were applied, what was caught, what was not verified and why. Link to §5 of verification-practices.md.

### 5. `framework/templates/shu-ho-template.md`

In the author notes for the Completion Checklist section, strengthen the language around testing AND linting. Currently it says:

> Always include:
> - Tests passing (cross-project)
> - Quality tools passing (cross-project)

Change the author note to emphasize that "tests passing" means "tests exist that would fail if the feature were broken" — not merely that existing tests still pass. Strengthen "quality tools passing" to explicitly name the code-lint-test cycle (code → lint → test → fix → re-test → re-lint → commit) as the non-negotiable development rhythm. Add a note linking to verification-practices.md Layers 1 and 1b for the full testing and linting philosophy.

In the AI Collaboration section (or wherever agent/AI guidance lives), add a note about recursive self-review: after the AI produces implementation, direct it to review its own output against the spec before the learner reviews it. Also note: AI-generated code should be run through the lint pipeline BEFORE the learner reads it — code that doesn't pass automated quality gates doesn't deserve human attention yet. Link to Layers 1b and 2.

### 6. `framework/templates/ha-ho-template.md`

In the Agent Task Log section author notes, add that the log should include verification method used. Add guidance that significant delegated work should include cross-agent verification. Link to Layers 2-3 of verification-practices.md.

In Phase 3 (Reflect), add a prompt for verification reflection: "What verification did you apply? What did it catch? Where were you under- or over-verifying?"

### 7. `framework/templates/ri-ho-template.md`

Add a verification field to the template structure — after "Files Changed" or equivalent, add a "Verification" section where the practitioner records what verification was applied. This should be brief (one line for trivial tasks, a few lines for significant ones).

### 8. `framework/templates/agent-task-spec.md`

In the "Writing Good Agent Tasks" section, add a subsection on verification protocol — what the practitioner should do AFTER the agent completes the task. Reference the four-layer stack. Specifically:

- Always run the test suite
- Direct self-review for anything non-trivial
- Cross-agent verification for critical path changes
- Human architectural review for all accepted work

In the Surgical Spec template, after ACCEPTANCE CHECKS, add a VERIFICATION PROTOCOL note reminding the practitioner which layers to apply.

In the Broad Spec template, add a similar note — verification of broad specs focuses on "does the feature work as intended" rather than line-by-line correctness.

### 9. `framework/templates/template-selection-guide.md`

In the Quick Reference table, add a "Verification" row:

```
| **Verification** | Lint + tests + template checks | Lint + tests + self-review + emerging cross-agent | Full stack (lint + tests → self-review → cross-agent → human review) | Lint + tests + self-review |
```

### 10. `ho-foundations-evidence.md` (if it exists)

In the section on the Access-Judgment Gap or AI-dependent development, add a note that verification practices are the Ho System's structural answer to the overconfidence problem documented in the METR and CodeRabbit research. The four-layer stack is not theoretical hygiene — it is the mechanism that makes "architectural authority" real rather than aspirational.

## INVARIANTS TO PRESERVE

1. All existing cross-references between documents remain valid
2. Template structures maintain their current section ordering and naming
3. Author notes (📐 blocks) maintain their existing format
4. The distinction between cross-project structure and project-specific content in templates is preserved
5. No template becomes longer than necessary — additions should be surgical, not expansive
6. Wiki-link format `[[slug|Display Name]](relative-path.md)` is maintained for all new links

## ACCEPTANCE CHECKS

- [ ] `verification-practices.md` is placed at `framework/structure/verification-practices.md`
- [ ] `the-ho-system.md` references verification-practices in its framework table and reading order
- [ ] All three stage templates (shu, ha, ri) reference verification practices appropriately for their stage
- [ ] `agent-task-spec.md` includes post-completion verification protocol
- [ ] `shu-ha-ri.md` includes verification notes in all three stage sections and both transition sections
- [ ] `devlog.md` addresses verification records
- [ ] `template-selection-guide.md` quick reference includes verification row
- [ ] All new wiki-links use correct format and point to valid relative paths
- [ ] No existing document has had its meaning, structure, or section ordering changed — only additions
- [ ] Running a grep for "verification-practices" across the framework shows references from at least 8 documents

## QUALITY

- Run any available linting or link-checking tools
- Verify all new wiki-links resolve to actual files
- Ensure no broken markdown formatting (tables, headers, code blocks)
- Read each modified document in full to confirm the additions flow naturally with existing content

## VERIFICATION PROTOCOL

After completing this task:

1. **Self-review:** Re-read this spec. For each of the 10 numbered changes, confirm it was implemented. For each acceptance check, confirm it passes.
2. **Cross-agent verification (recommended):** Have an evaluation model review the diffs against this spec, focusing on whether the additions are proportionate (not too verbose) and whether they maintain the voice and register of each document they're added to.
