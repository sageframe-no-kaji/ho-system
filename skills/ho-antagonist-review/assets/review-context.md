# Review context

Complete this packet from project evidence before a review. These are configuration fields, not claims about any particular language or project; omit genuinely inapplicable fields and explain material omissions. Resolve essential unknowns with the owner rather than inventing acceptance criteria.

## Identity and scope

- Project and repository location:
- Mode: ho review / final codebase review
- Review cycle ID:
- Pass number and maximum: 1 of 2 by default; maximum 3 if selected
- Current reviewed commit or immutable snapshot:
- Ho ID and opening revision, if applicable:
- Previous reviewed endpoint and repair range, on follow-up:
- In-scope deliverables, directories, files, and integration boundaries:
- Exclusions and reasons:
- Report and cycle-record destinations; outside the repository unless designated otherwise:
- Commit authorization: none unless explicitly provided

## Roles and handoffs

- Active role: reviewer / builder / coordinator
- Coordinator and ledger owner:
- Builder and reviewer identities; host/session identifiers when available:
- Governing instructions: verified Git commit/repository/subdirectory or recoverable snapshot and file-hash manifest:
- Accessible pinned SKILL.md and complete package location:
- Instruction comparison before the latest handoff: time, sources compared, match/drift, chosen version:
- Owner-authorized instruction-version changes and effective handoff/pass, if any:
- Latest handoff file: pass-N-handoff.md beside its report; relative paths based there:
- Handoff mode: manual prompt / authorized agent dispatch
- Existing authorization for repairs, delegation, and re-review; scope and limits:
- Recipient-accessible artifact locations and delivery status:
- Current dispatch: cycle/pass, recipient, pending/running/completed, report path:

## Repair workspace

- Main checkout and repair directory; existing checkout / worktree / snapshot:
- Starting commit plus any uncommitted or untracked content in the reviewed state:
- Content identity of the reproduced starting state and completed repair endpoint:
- Builder response destination and verification evidence location:
- Commit, integration, push, and cleanup permissions, recorded separately:

## Requirements and decisions

- Product requirements or seed:
- System design and binding architectural constraints:
- Ho specification and acceptance criteria, if applicable:
- Mandatory constraints distinguished from preferences and estimates:
- Latest owner rulings and what each supersedes:
- Accepted limitations and evidence boundaries:
- Previous report and finding dispositions, on follow-up:
- Changes the builder was authorized to make:

## Verification

- Exact required commands and their working directories:
- Approved execution environment and relevant toolchain versions:
- Permitted temporary files, caches, scratch copies, and probes:
- Prohibited execution or external effects:
- Manual or external evidence required for acceptance:
- Prior evidence eligible for reuse, with revision and logs:
- Investigation time budget, if the owner has set one:
- Reviewer independence: fresh context / shared builder context disclosed

## Cycle state

- Status: ready / reviewing / awaiting fixes / awaiting decision / stopped / closed
- Reviewer assessment from the last pass:
- Owner acceptance, if any, recorded separately:
- Findings still open and findings closed or accepted:
- New evidence that permits reconsidering an accepted item, if any:
- Passes remaining:
- Permission to arrange another review or repairs, if already given:

Do not store credentials in this packet. A missing prior report or counter is not permission to start at pass 1 again; reconcile the existing record first.
