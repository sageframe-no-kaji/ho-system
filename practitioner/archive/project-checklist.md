---
id: "7.1"
title: "Ho System Codification Checklist"
type: seed
stage: n/a
status: draft
version: "1.0"
tags: [ho-system, checklist, codification, private]
---

> Archived 2026-07-03 (ho-05 review). The 2026-02 Phase-1 codification tracker (was INDEX 7.1, `type: seed` — miscategorized; it is a checklist), previously in the gitignored `private/`. Its live function is superseded: completed work is in the framework and git history, and the forward-only backlog is now `ho-process/ideas.md` (D16). Preserved as historical record.

# Ho System Codification Checklist

**Phase 1: Codification — Pilot Evidence to Reusable Framework**

---

## Critical Path Decision — RESOLVED

- [x] **Framing resolved**: The Ho System is BOTH, structured as:
    - **Primary identity**: A collaborative development methodology for systems thinkers building with AI
    - **Essential component**: A pedagogical framework for learning and practicing the methodology
    - **Philosophy**: Knowledge is open-source (methodology published freely), mentorship is where value lives (facilitation, training, consulting)
    - **Market position**: Geared toward a new type of developer—process/architecture-oriented rather than technically-oriented
    - **IP strategy**: Book/publication shows the methodology openly; training/consulting helps teams practice it effectively

---

## Core Framework Work

### 1. Shu-Ha-Ri Progression Specification

**→ [[framework/structure/shu-ha-ri]]**

- [x] Document three ho modes (Shu/Ha/Ri)
- [x] Define characteristics of each stage
- [x] Establish transition criteria between stages
- [x] Create examples from Kanyō for each mode

### 2. Template Variants

**→ [[framework/templates/]]**

- [x] Extract base template from Design Seed Appendix A → [[framework/templates/devlog-template]]
- [x] Create Shu-stage template (full prescriptive structure) → [[framework/templates/shu-ho-template]]
- [x] Create Ha-stage template (flexible collaborative structure) → [[framework/templates/ha-ho-template]]
- [x] Create Ri-stage template (agent task format) → [[framework/templates/ri-ho-template]]
- [x] Write template selection guidance → Add to [[framework/structure/shu-ha-ri]]

### 3. Project Selection Criteria

**→ [[guides/choosing-a-project]]**

- [ ] Extract "what makes a good first ho project" from Kanyō evidence
- [ ] Document what worked vs. what created strain
- [ ] Create recommendations for pedagogical applications
- [ ] Write complete guide

---

## Publication Preparation

### 4. Curate Kanyō Evidence Base

**→ [[examples/kanyo-pilot/]]**

- [ ] Review all Kanyō hos for publication value
- [ ] Select 3-5 exemplar hos to include:
    - [ ] [[examples/kanyo-pilot/ho-01-git-good|Ho 01 (Git Good)]] — shu-stage example
    - [ ] [[examples/kanyo-pilot/ho-02-falcon-vision|Ho 02 (Falcon Vision)]] — scope creep lesson
    - [ ] [[examples/kanyo-pilot/ho-03-live-detection|Ho 03 (Live Detection)]] — shu→ha transition
    - [ ] [[examples/kanyo-pilot/ho-05_6-architecture-redesign|Ho 05.6 (Architecture Redesign)]] — ha-stage example
    - [ ] [[examples/kanyo-pilot/agent-task-004|Agent Task 004]] — ri-stage example
- [ ] Write meta-commentary → [[examples/kanyo-pilot/README]]
- [ ] Move [[learning-process]] → [[examples/learning-process]] with minor edits

### 5. Core Structure Documents

**→ [[framework/structure/]]**

- [ ] Extract and expand "What is a Ho" → [[framework/structure/ho-structure]]
- [ ] Extract and expand "Project Arc" → [[framework/structure/project-arc]]
- [ ] Extract and expand "Tiered Understanding" → [[framework/structure/tiered-understanding]]
- [ ] Write overview README → [[framework/structure/README]]

### 6. Self-Guided Materials

**→ [[guides/]]**

- [ ] Write getting started guide → [[guides/getting-started]]
- [ ] Write AI collaboration guide → [[guides/ai-collaboration]]
- [ ] Write overview README → [[guides/README]]

### 7. IP Boundary Definition

- [ ] Finalize publishable vs. proprietary split (done in principle)
- [ ] Create "What We're Sharing" list (captured in repo structure)
- [ ] Create "What We're Holding" list → [[private/README]]
- [ ] Flag any gray areas requiring discussion

---

## Documentation Deliverables

### 6. Ho Author's Guide (Proprietary)

**→ [[private/authoring/ho-authors-guide]]**

- [ ] Create outline
- [ ] Write "How to Design a Ho Sequence" section
- [ ] Write "Calibrating Structure to Learner Stage" section
- [ ] Write "Common Authoring Pitfalls" section
- [ ] Include worked examples

### 7. Design Seed Revisions

**→ [[framework/design-seed]]**

- [x] Add section 1.4 "The Access-Judgment Gap" to Part I
- [x] Add section 1.5 "Open Knowledge, Facilitated Practice" to Part I
- [ ] Update Abstract to reflect "collaborative development methodology" framing
- [ ] Revise Part V "Evidence — The Kanyō Pilot" with honest lessons learned
- [ ] Update Appendix A template or remove (templates now in separate files)
- [ ] Add references to new structure files throughout

### 8. Repository Infrastructure

**→ Root level files**

- [x] [[README|Root README.md]] (inspired version drafted)
- [ ] LICENSE (CC BY-NC-ND 4.0)
- [ ] [[CONTRIBUTING]] (feedback/evidence submission guidelines)
- [ ] .gitignore (private/, .obsidian/, .DS_Store)
- [ ] Framework README → [[framework/README]]
- [ ] Examples README → [[examples/README]]

---

## Public Content Draft Sprint

**Goal**: Get strong first drafts of all public-facing content

**Estimated Time**: 4-6 hours with Claude Code (Opus for substantial content)

### High Priority (Core Framework)

- [x] `framework/structure/shu-ha-ri.md` (NEW, COMPLEX - 1-2 hours)
- [ ] `framework/templates/shu-ho-template.md` (extract + adapt - 30 min)
- [ ] `framework/templates/ha-ho-template.md` (extract + adapt - 30 min)
- [ ] `framework/templates/ri-ho-template.md` (extract + adapt - 30 min)
- [ ] `framework/design-seed.md` revisions (Abstract, Part V - 45 min)
- [ ] **Agent task template + tracking system** — the artifact you hand to agent-mode AI. Separate from the ri template because agent tasks are a _tool_ used across stages (you could write one during a ha-stage session), even though they're most common in ri.

### Medium Priority (Structure Docs)

- [ ] `framework/structure/ho-structure.md` (extract from design-seed - 30 min)
- [ ] `framework/structure/project-arc.md` (extract from design-seed - 30 min)
- [ ] `framework/structure/tiered-understanding.md` (extract from design-seed - 30 min)

### Lower Priority (Guides)

- [ ] `guides/getting-started.md` (NEW - 45 min)
- [ ] `guides/choosing-a-project.md` (synthesize from analysis - 30 min)
- [ ] `guides/ai-collaboration.md` (extract from learning-process - 30 min)

### Examples (Editorial)

- [ ] `examples/kanyo-pilot/README.md` (NEW - 30 min)
- [ ] Curate + lightly edit 4-5 hos (1 hour)

### Infrastructure

- [ ] All READMEs at various levels (30 min total)
- [ ] LICENSE file (10 min)
- [ ] CONTRIBUTING.md (20 min)

**TOTAL ESTIMATE**: ~10-12 hours of writing **REALISTIC WITH COLLABORATION**: 4-6 hours if we work efficiently

---

## Status Tracking

**Last Updated**: February 9, 2026 **Current Phase**: Framework Definition **Completed**: Framing decision resolved, Part I additions drafted, repo structure designed **Next Priority**: Populate public-facing content

---

## Time Estimates for Public Content

### Quick Wins (Can Extract from Existing) — ~1-2 hours

- [ ] Extract [[framework/structure/ho-structure]] from Design Seed §3.1
- [ ] Extract [[framework/structure/project-arc]] from Design Seed §3.2
- [ ] Extract [[framework/structure/tiered-understanding]] from Design Seed §3.3
- [ ] Extract [[framework/templates/devlog-template]] from Design Seed Appendix C
- [ ] Move [[learning-process]] to [[examples/learning-process]]
- [ ] Move Kanyō hos to [[examples/kanyo-pilot/]]

### Moderate Effort (New Writing, Clear Source Material) — ~2-3 hours

- [ ] Write [[framework/structure/shu-ha-ri]] (we have analysis from earlier)
- [ ] Write [[guides/choosing-a-project]] (we have notes on Kanyō lessons)
- [ ] Create [[framework/templates/shu-ho-template]] (adapt from Design Seed Appendix A)
- [ ] Create [[framework/templates/ha-ho-template]] (based on Ho 05.6 pattern)
- [ ] Create [[framework/templates/ri-ho-template]] (based on Agent Task 004 pattern)
- [ ] Write [[examples/kanyo-pilot/README]] (meta-commentary)

### Substantial Writing (New Content) — ~2-3 hours

- [ ] Write [[guides/getting-started]]
- [ ] Write [[guides/ai-collaboration]] (can pull from learning-process.md)
- [ ] Write all README files ([[framework/README]], [[guides/README]], [[examples/README]])
- [ ] Write [[CONTRIBUTING]]

### Polish Pass — ~1 hour

- [ ] Review all public content for consistency
- [ ] Update cross-references between docs
- [ ] Clean up Design Seed references to new structure

**TOTAL FOR STRONG FIRST DRAFTS: ~6-9 hours of focused work**

---

## Priority Order for Today

1. **Infrastructure** (30 min) — Create directory structure, move files, add LICENSE
2. **Quick extractions** (1-2 hours) — Pull content from Design Seed into structure files
3. **Templates** (1-2 hours) — Create the three ho templates + devlog template
4. **Shu-Ha-Ri doc** (1 hour) — This is foundational for everything else
5. **Guides** (2 hours) — Getting started + choosing a project + AI collaboration
6. **Example curation** (1 hour) — Kanyō README + select hos to include
7. **README files** (30 min) — All the connector READMEs
8. **Polish** (30 min-1 hour) — Cross-references, consistency check

**Target: Strong draft of ALL public content in 6-8 hours of focused work.**
