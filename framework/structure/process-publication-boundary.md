---
id: "2.15"
title: "Process Publication Boundary"
type: structure
stage: n/a
status: stable
tags: [ho-system, structure, privacy, publication, process-record, synchronization]
---

# Process Publication Boundary

## Keeping an Exact Build Memory and a Public Build Record

---

## 1. The Boundary

Ho process documents can require facts that should not ship with a public
project: real machine names, filesystem paths, account identifiers, network
topology, exact fleet shape, unvarnished incidents, and the practitioner's
working language. Removing those facts from the working record damages Kamae 6
and weakens executable agent handoffs. Publishing the exact record exposes an
operational map that a reader does not need.

The solution is a **dual-record posture**:

- `ho-process-private/` is the canonical nested private repository. Agents read
  and edit this record because it preserves the exact facts needed to continue
  the build.
- `ho-process/` is the generated public derivative. Readers use this record to
  inspect decisions, sequence, evidence, and revisions without receiving the
  practitioner's operational map.

The two trees are not peers. The private record is source and the public record
is output, so synchronization has one direction and no merge problem.

---

## 2. Facts Are Not Secrets

The private record may contain operational facts when the work depends on them:
host aliases, exact paths, topology, observed fixture state, failure details,
and commands that reproduce a finding. These facts belong there because a
future session otherwise has to rediscover them or may act on an invented
substitute.

Credentials belong in neither record. Passwords, private keys, API tokens,
session cookies, recovery codes, and authentication headers stay in the system
Keychain or a secret manager. A private Git remote reduces audience; it does
not turn Git history into a credential store.

---

## 3. The Four Postures

Choose the posture from repository visibility and publication intent:

1. **Private project repository.** Track `ho-process/` in the project. The
   project and its process record share one private history.
2. **Public project, private process.** Ignore `ho-process-private/` in the
   public project and version it as a nested private repository. Publish no
   process tree.
3. **Public project, public process showcase.** Use the dual-record posture:
   canonical `ho-process-private/`, generated `ho-process/`, a revision
   manifest, an exporter, a public privacy checker, and a private pre-push gate.
4. **Unversioned floor.** Keep the canonical process tree outside Git. This is
   permitted when no private remote is available, but it loses process history
   and weakens recovery.

The public-process showcase is an explicit election because it adds a second
artifact with a maintenance obligation. Do not create it merely because the
project repository is public.

---

## 4. The Source Contract

In the dual-record posture, project instructions name
`ho-process-private/` as the process root. Every returning agent reads Kamae 6
there first, writes hos and agent tasks there, and treats `ho-process/` as
read-only generated output. The generated tree may contain a sanitized Kamae 6
for documentary continuity, but that file is an exhibit rather than the live
handoff.

The private repository carries its own `AGENTS.md` or equivalent instruction
file stating three rules: preserve exact operational facts, store no
credentials, and publish only through the exporter. The public repository
ignores the nested private root so an ordinary `git add -A` cannot absorb it.

---

## 5. The Export Transaction

A publication run is deterministic from one committed private revision:

1. Refuse a dirty private working tree.
2. Read the canonical files from the committed private revision.
3. Apply explicit, reviewed replacements for known private identifiers.
4. Refuse binary files, invalid text, credential patterns, personal home paths,
   private network addresses, and every known identifier that survives.
5. Write a machine-readable manifest containing the private source revision and
   redaction-policy version. Do not include a timestamp, because identical
   source and policy must produce identical bytes.
6. Compare the generated tree with `ho-process/`; `--check` reports drift and
   `--apply` replaces the derivative.
7. Review the public diff before committing it.

Redaction is explicit rather than heuristic. A new identifier class stops the
publication until the practitioner chooses its public representation; the
exporter does not silently delete a paragraph or invent a topology.

---

## 6. The Synchronization Gates

The private repository's pre-push hook enforces publication order. It refuses
unless the public export matches private `HEAD`, the parent public repository
is clean, and its current commit matches its configured upstream-tracking
reference. The resulting order is private commit, public export, public review,
public commit, public push, then private push.

The public repository enforces the other half independently. Its pre-commit and
CI checks scan `ho-process/` for generic credential and privacy classes, so a
public edit or a broken exporter cannot bypass the boundary merely because the
private repository is absent in CI. The source-revision manifest makes drift
inspectable from either side without exposing the private remote.

The pre-push upstream comparison uses the local remote-tracking reference. Run
`git fetch` before a release or closeout so the gate is comparing against a
current view of the remote.

---

## 7. Change and Closeout

During the build, edit only the canonical private record. Any process change
that should be public passes through the export transaction in the same working
session, while a private-only incident remains canonical and is generalized or
excluded by an explicit policy change. The public history records each new
derivative instead of rewriting prior public commits.

At closeout, the practitioner may keep the dual record, stop publishing further
updates, or publish a final derivative. Removing private values from the current
public tree does not remove them from earlier commits, tags, forks, release
archives, or caches, so a history rewrite is a separate incident response used
only when the exposure warrants its disruption.

---

## 8. Related Framework Documents

- [[cross-session-continuity|Cross-Session Continuity]]
  (framework/structure/cross-session-continuity.md) — Kamae 6 privacy,
  canonical authority, and the hot/cold memory relationship.
- [[kamae-project-framing|Kamae: Project Framing]]
  (framework/structure/kamae-project-framing.md) — process-root and filename
  conventions.
- [[verification-practices|Verification Practices]]
  (framework/structure/verification-practices.md) — independent gates and
  evidence requirements.

---

_This document is part of the Ho System framework. It specifies the dual-record
posture for publishing a sanitized Ho process without weakening the canonical
operational memory that agents use to continue the build._
