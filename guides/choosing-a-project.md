---
id: "4.2"
title: "Choosing Your First Project"
type: guide
stage: n/a
status: draft
tags: [ho-system, project-selection, onboarding]
---

# Choosing Your First Project

The first project teaches you the rhythm — framing, bounded sessions, verification, reflection. Choose one that lets the rhythm be the hard part, not the domain. A project that fights you on both at once teaches you neither.

## What makes a good first project

**It's real.** You want the thing to exist when you're done, and you want to care whether it works. A throwaway exercise doesn't produce the pull that carries you through a full arc, and it doesn't leave you with a record worth keeping. Pick something you'd use, or something someone you know needs.

**It has edges.** You can say, in a sentence, what "done" looks like. "A CLI that renames my media files from a config" has edges. "A personal knowledge system" does not — it's a direction, not a destination. If you can't write the README before the code exists, the scope isn't a project yet.

**It's mostly in territory you can reach.** Not territory you already own — that would teach you nothing — but territory where the parts central to whether the thing *works* are parts you can come to understand. A good first project has one or two genuinely new things in it and a lot of solid ground under your feet.

**It fits an arc.** Enough hos to practice the rhythm — roughly a handful of building sessions past orientation — but not so many that you stall before the first release. If the ho overview runs to twenty hos before anything ships, the project is too big to be a first project. Split it, or pick the sharp inner core and build that.

## What makes a bad first project

- **The moonshot.** Everything is new, nothing is solid, and the finish line is out past the horizon. You'll spend the arc discovering that the architecture was wrong, which is a real lesson but a demoralizing first one.
- **The vague.** No edges, no README you could write today. The project drifts because there's nothing to check drift against.
- **The trivial.** One session, no decisions, nothing to reflect on. You get through it but you don't practice the parts that matter — sequencing, verification, closing a ho honestly.
- **The one whose core is a black box you can't open.** More on this next.

## The black-box discipline, applied to choice

You will treat parts of every project as opaque — a library, a framework, a model, a runtime behavior you use without verifying its internals. That's not a failure; it's how real systems get built. The discipline (from the [[operating-discipline|Operating Discipline]] (practitioner/operating-discipline.md), grounded in [[tiered-understanding|Tiered Understanding]] (framework/structure/tiered-understanding.md)) is to *know which parts are opaque and be deliberate about it* — Tier 1, use without investigating; Tier 2, understand enough to configure and explain; Tier 3, could redesign.

Apply it to project choice like this: **the black boxes are allowed to be peripheral, not central.** A project is a good fit when the parts you're treating as opaque don't decide whether it works, and the parts that decide whether it works are ones you can get to Tier 2 on. It's a bad fit when the project's correctness hinges on a black box you can't open — because when it breaks, and it will, you'll have no model to debug against.

Concretely: building a tool that calls a well-documented API you treat as a black box is fine — the API's internals aren't your correctness problem. Building a tool whose whole value is a computer-vision model you neither understand nor can evaluate is a bad first project — the black box *is* the project, and you can't tell when it's wrong.

## Sizing

Size the project by counting the hos you can imagine, then halve your confidence. First projects run long because you're learning the rhythm at the same time as building. A project that looks like six hos and ships one real capability is a good size. If it looks like two, it's a session, not a project — fold it into a larger one or pick something with more to it. If it looks like fifteen before a first release, carve off the core.

The numbering system absorbs a plan that turns out wrong — hos branch and multiply as reality diverges (see [[ho-structure|Ho Structure]] (framework/structure/ho-structure.md) §3). So you don't need the estimate to be right. You need it to be *roughly arc-shaped*: an orientation ho, a few building hos, something that ships.

## Three worked shapes

**A file-wrangling utility.** A CLI that ingests a directory, reads a config, and renames or reorganizes files by rule. Solid ground: the language, the filesystem, argument parsing. New territory: config schema design, dry-run/commit safety, edge cases in real messy data. Ships as something you actually run. Honest hard part — the edge cases are where the real work is, and they don't show up until you point it at your own mess.

**A self-hosted service with one job.** A small container that does one operational thing — watches a folder and posts a notification, wakes a machine on a schedule, mirrors a dataset. Solid ground: the language, the single job's logic. New territory: deployment, the container boundary, running unattended. Ships as a thing on your homelab. Honest hard part — "runs on my machine" and "runs unattended at 3am" are different projects, and the gap is the lesson.

**A tool that wraps an API you trust.** A utility built on a documented external service — a report generator over a data API, a bulk-editor over a SaaS you use. Solid ground: your language, the API's documented surface (a deliberate Tier 1 black box). New territory: auth, rate limits, failure handling, presenting the result. Ships as something that saves you a recurring chore. Honest hard part — the failure modes live in the network between you and the black box, not in your code.

Each of these has edges, a real deliverable, a small stack of solid ground, and one or two genuinely new things. That's the shape to look for.

## Where to go next

- [[getting-started|Getting Started with the Ho System]] (guides/getting-started.md) — once you've picked, this walks the first arc.
- [[kamae-project-framing|Kamae: Project Framing]] (framework/structure/kamae-project-framing.md) — how the idea becomes a buildable plan.
