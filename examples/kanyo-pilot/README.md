---
id: "5.2"
title: "Kanyō Pilot Project"
type: example
stage: n/a
status: stable
version: "1.0"
tags: [ho-system, example, kanyo, pilot]
---

# Kanyō Pilot Project

Production falcon detection system built using the Ho System.

## What Was Built

Kanyō is a production peregrine falcon monitoring system built around live stream capture, computer vision detection, event logging, clip generation, and public/admin interfaces. The example set in this directory documents the project's progression from tool setup through deployment, architecture redesign, debugging, and later-stage operational refinement.

## What This Demonstrates

- Full project arc progression across shu, ha, and ri work
- The transition from prescriptive hos to compact operational records
- Architectural redirection in response to real integration failures
- How per-ho work and bounded agent-task execution can coexist in one project

## The Ho Sequence

- `ho-00-overview.md` — Proto-Kamae project overview before the framework split the chain into distinct documents
- `ho-0_5-tool-mastery.md` through `ho-04-docker-deploy.md` — early structured build-out
- `ho-05-deployment-verification.md` through `ho-06-gui-architecture-planning.md` — transition into self-directed systems thinking
- `ho-05_7-state-redesign.md` through `ho-06_51-cloudflare-tunnnel.md` — ri-shaped operational and maintenance work
- `ho-07-yolo-training.md` — later planning work in a new sub-domain

## Agent Task Examples

These are included as child artifacts of later hos, matching the execution pattern described by `ho-kamae-5-authoring-collaborator`.

- `agent-tasks/Ho-05_7-AT-01.md` — bounded fix for departure clip timing, paired with `ho-05_7-state-redesign.md`
- `agent-tasks/Ho-06_1-AT-01.md` — bounded admin-GUI performance task, paired with `ho-06_1-admin-gui-implementation.md`
- `agent-tasks/Ho-06_13-AT-01.md` — bounded feature task for arrival confirmation, paired with `ho-06_13-arrival-confirmation-system.md`

## Related Evidence

- `learning-process.md` — reflective summary of what the pilot taught
