---
id: "7.3"
title: "Seed Template Checklist"
type: agent-task
stage: n/a
status: draft
version: "1.0"
tags: [ho-system, seed, checklist, kamae]
---

# Framework Update Task List

## Context

The Seed Template surfaces a structural hole: the Ho System's Kamae phase treats the idea as a given and starts at structuring. The Seed absorbs the Concept document and becomes the first document in the Kamae chain. The Precedential Thinking Substack essay provides the intellectual foundation for why the seed phase exists. These changes cascade through the framework documents in the order below.

---

## The Order

### 1. Ho_Seed_Template.md — Rewrite ✅

**Status:** Complete. Output: `Ho_Seed_Template_v2.md`

**What:** Absorb the Concept role, add missing sections (Constraints, Builder's Current Skill Level), remove the "precedes Kamae / does not replace" framing, and declare the Seed as the first document in the Kamae chain (`Seed → System Design → README → Ho Overview`).

**Also:** Fix the tone. The template currently reads like a workshop handout — over-coached, facilitation voice, "What enough looks like" x11. It should read like a practitioner talking to a practitioner. The precedential thinking sections are closest to the right register; the rest needs to match. Reference the Substack essay's voice, not its content (the essay is public writing; the template is framework documentation).

**Resources:** Precedential_Thinking_-_Substack.md (for voice and intellectual grounding), kamae-project-framing.md §2.1 (for what the Concept covered that the Seed currently doesn't).

---

### 2. kamae-project-framing.md — Structural revision ✅

**Status:** Complete. Output: `kamae-project-framing.md`

**What:** Replace the Concept (§2.1) with the Seed. Update the chain diagram to `Seed → System Design → README → Ho Overview`. Revise §1 (What Kamae Is) to acknowledge that Kamae begins with structured ideation, not just structuring an existing idea. Revise §4 (When to Do What) to account for the Seed as a phase with its own time budget. Update the Kanyō evidence section (§7) to note that the pilot's seed was implicit/internalized. Update §3 (The Chain) diagram and narrative.

---

### 3. shu-ho-template.md — Reference update ✅

**Status:** Complete. Output: `shu-ho-template-edits.md` (3 find-and-replace edits)

**What:** Update the Pre-Ho Documents list. Item 1 ("Seed") currently says "Raw ideation... brainstorming output — messy is fine." Replace with a description that points to the Seed Template and describes it as structured capture of precedential thinking, not messy brainstorming. Verify no other references to "Concept" as a document name.

---

### 4. ha-ho-template.md — Reference update ✅ No changes needed

**Status:** Verified. No Pre-Ho Documents section, no chain reference, no "Concept" or "Architecture Overview" references.

**What:** Check whether it references the pre-ho document chain or the Concept by name. If so, align with the updated terminology (Seed replaces Concept). Likely a minor edit.

---

### 5. ri-ho-template.md — Reference check ✅ No changes needed

**Status:** Verified. No chain reference, no "Concept" or "Architecture Overview" references. Minimal by design.

**What:** Same check as ha-ho-template. Ri-stage hos are minimal and may not reference the pre-ho chain at all, but verify.

---

### 6. the-ho-system.md — Wayfinding update + chain logic elevation ✅

**Status:** Complete. Output: `the-ho-system-edits.md` (4 find-and-replace edits)

**What:** Add the Seed Template to the Document Map ("Start Here" or "Understand the System" table). Update the Key Concepts section to include precedential thinking as a named practice. Update the Kamae description (currently in the Project Arc expandable) to reflect the new chain starting with the Seed.

**Also — elevate the chain logic.** The Kamae chain's evaluative structure — the seed as philosophical foundation, System Design as architectural commitment, README as concrete scope, Ho Overview as buildable sequence — should be visible here, not buried in the Seed Template or the Kamae spec. This is one of the most important ideas in the framework: each document increases commitment, and the seed persists as the evaluative reference against which all downstream decisions are judged. The parti concept. This belongs in the root-level explanation of how the Ho System works.

---

### 7. template-selection-guide.md — Terminology check ✅ No changes needed

**Status:** Verified. References Kamae as a phase but does not name individual chain documents. No "Concept" references.

**What:** Search for references to "Concept" as a document name or the old four-document chain order. Align any found references with the new terminology.

---

### 8. design-seed.md (the governing Design Seed Document) — Reference addition 📝

**Status:** Edit note provided in `the-ho-system-edits.md`. Could not access full current document through project knowledge to produce exact find-and-replace. Glossary additions and chain reference updates specified.

**What:** Add a reference to the Project Seed Template and its position in the framework. This is the governing document, so the edit should be minimal — a mention in the appropriate section, not a rewrite. The Design Seed establishes principles; the Project Seed Template operationalizes the ideation phase.

---

### 9. ho-foundations-evidence.md — Flag for future revision 🔮

**Status:** Flagged. No action this pass.

**What:** Do not rewrite now. Flag that the intellectual case should eventually incorporate the precedential thinking argument and the ideation pedagogy content as part of the Ho System's theoretical grounding. This is a future workstream that depends on the ideation/design teaching material being formalized — the stuff from 15 years of practice that hasn't been written into the framework yet.

---

## Not In This Pass

- **Website updates** (ho-system.html, ho-foundations.html) — The website doesn't need to change yet, but when it does, the "how a project starts" story and the precedential thinking frame should be added to the public narrative.
- **Ideation pedagogy formalization** — The deep content on teaching ideation and design thinking. Decades of material. Separate workstream. Will eventually feed into both the Seed Template's facilitation layer (proprietary) and the public intellectual case.
- **Kinhin integration** — The Seed phase will be part of Kinhin's workflow. Architecture decisions for that are downstream of getting the framework documents right. Related idea: Claude skills at each Kamae stage (a seed-stage skill that probes rather than checklists, a System Design skill, etc.). This is a concrete product feature, not just a documentation task.
- **Precedential Thinking Substack essay** — Published (or ready to publish) as public writing. It provides the intellectual argument for why the Seed exists. The framework documents should be consistent with it but don't need to repeat it.

## Discovered During This Pass

- **README.md** (repo root) — The file tree in the README doesn't include `project-seed-template.md` in the templates directory. Quick add when committing the other changes.
- **project-arc.md** — References Kamae as "where arcs begin" but doesn't name chain documents. Probably clean, but worth a quick scan for "Concept" when committing.
- **devlog.md** — Mentions "kamae produces reflection" in a file listing. Should be clean but verify.

---

## Proprietary Boundary Notes

The Seed Template itself is **publishable** — it describes what to think about and what good looks like. What's **proprietary**:

- How to facilitate seed development with learners who've never done precedential thinking
- How to diagnose a thin seed vs. a rich one
- How to coach someone from "I want to build X" to "I've researched the space and here's why X needs to exist"
- The ideation pedagogy itself (the 15 years of design teaching methodology)

The Substack essay is public writing that argues for the importance of precedential thinking without giving away the facilitation methodology. That boundary holds.
