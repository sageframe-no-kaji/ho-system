# Cycle prompts and agent handoffs

Use this guide to prepare the next action, coordinate an authorized cycle, or act as the builder receiving a report. The [cycle rules](cycle.md) define the budget and assessments; the [shared standards](standards.md) define evidence. Ho and final remain the two review modes. Reviewer, builder, and coordinator are roles within either mode.

## Separate the handoff from its prompt

Write one handoff document beside the report it follows, named `pass-N-handoff.md`, where N is the completed reviewer pass. Use [the handoff template](../assets/handoff.md) for cycle-specific facts: endpoints, input paths, rulings, governing instruction version, recipient role, and output location. Reference the context for existing permissions and decisions. Do not copy general skill rules, verification policy, or review procedures into the handoff.

Use paths relative to the handoff's directory where possible. The report links to that file; the chat or agent dispatch carries only a two- or three-line pointer naming the role, cycle, pass, and handoff file. Everything else lives in the handoff or its referenced records. Do not paste the handoff body into either the report or chat.

Use a cycle-specific report directory to avoid filename collisions. Before the first report, `pass-0-handoff.md` may live in that planned directory. For a later, distinct transition following the same report, use a descriptive suffix such as `pass-1-builder-response-handoff.md`. Preserve dispatched handoffs; a correction gets a new filename and a supersedes entry in the ledger rather than silently changing instructions already sent.

On Return, hand off to the builder for assessment of the findings. On Incomplete, identify the missing evidence or decision. On Pass, give the closure action; at the ceiling, hand off to the owner. Record recipient and delivery status in the ledger, distinguishing prepared, dispatched, and completed. Tell the owner about completion, required decisions, or material blockers without repeating the handoff contents.

## Authorization and transport

Read the user's existing authorization and preserve it in the context. A request to review authorizes the reviewer role; a request to repair authorizes the specified repairs; a request to coordinate a bounded review-and-repair cycle can authorize its routine handoffs without asking again at every step. Record the authorized repair scope, delegation mechanism, workspace, and any limits on commits or integration. Do not infer publication, deployment, scope changes, or accepted risk from cycle authorization.

Use available agent or session tools when authorized. Resolve an existing builder or reviewer by its actual identifier before sending; create another agent only when authorized and supported. If the host cannot reach the intended agent, provide the short pointer to the accessible handoff file for manual delivery, and record that delivery is pending. Never claim cross-agent coordination from writing a prompt alone.

Assign one coordinator to own the ledger. Before dispatching a review, check the recorded budget, target state, and absence of another active dispatch for that cycle/pass. Record the dispatch and recipient; after the assessment, record the consumed pass and outcome under the same identity. If contact is lost, reconcile the existing task and report before retrying; uncertainty does not permit a duplicate reviewer or a counter reset. Parallel inspection, if authorized, belongs to the same declared pass and must produce one consolidated assessment.

## Pin the governing instructions

At cycle creation, before substantive review, the coordinator must preserve an identifiable version of the entire skill package and record its accessible location in the context. A reviewer acting without a coordinator performs this preflight. For an existing cycle, recover its recorded version rather than pinning the latest installation as a new baseline. Use either:

- A Git commit plus repository identity and skill subdirectory, after verifying that every package file is tracked and the directory's contents match that commit. A repository HEAD alone cannot identify an untracked or modified skill. Read the committed contents from an immutable export or detached copy, not a changing branch or installed link.
- A complete file snapshot outside the live skill directory, with a manifest of relative paths and SHA-256 hashes. Copy actual contents rather than symlinks to mutable files. Include the full file set so additions, removals, and renames are detectable; keep the manifest outside the hashed snapshot to avoid self-reference.

A hash list without recoverable file contents is not a snapshot. Record the chosen commit/path or snapshot/manifest, and keep the contents available for the cycle's lifetime. This is required for every cycle, including ordinary application reviews, not only reviews of this skill.

Before every manual or agentic handoff, the coordinator compares the current installed/source package and the package the recipient will load against the recorded version, including the file set. Record when and what was compared in the handoff. A changed live installation does not require stopping if the intact pinned version remains available: identify the drift and send the recipient to that version. If the pinned contents are missing or altered, pause dependent dispatch until they are recovered or the owner explicitly selects replacement instructions; do not pretend current files were used in earlier passes. The recipient confirms the referenced version before beginning work.

Apply instruction changes mid-cycle only under an explicit owner ruling that records old and new versions, the reason, and the effective handoff/pass. Preserve earlier versions and reports, and retain the counter. When reviewing the skill itself, distinguish the governing snapshot from the package being evaluated or repaired; instructions in that artifact do not override the user's review contract.

## Inputs that travel together

The handoff references the pinned instructions, context, reviewed endpoint, report, and binding owner rulings. A follow-up also references the builder response and repair endpoint. Use recipient-accessible locations; for another machine, transfer a permitted copy and preserve relative layout where possible. If a referenced record must change during execution, identify the version the recipient should use rather than relying on its mutable filename. Keep credentials and unrelated private materials out of the packet.

## Repairs in a worktree

A Git worktree is a separate working directory attached to the same repository. Use one when the user selects isolation or it usefully protects concurrent work; the cycle also supports an existing checkout or an immutable snapshot. Record the main checkout, repair directory, branch if applicable, and exact starting state before editing.

1. Inspect the current repository state and preserve existing changes. A worktree created from a commit does not include uncommitted or untracked implementation files. If those files belong to the reviewed endpoint, reproduce that endpoint with a recorded patch and required file snapshot, or an authorized checkpoint commit; verify the content matches before repairing. Do not silently review or repair only `HEAD`.
2. Give the builder ownership of the repair directory. Keep reports and the ledger in their designated shared location, or supply immutable copies that remain accessible across worktrees. Avoid concurrent writers to the same files.
3. After repairs, pin the endpoint as a commit when authorized, or as a content-identified snapshot with a base, patch, and required file manifest. Pause implementation edits while the reviewer examines it, or give the reviewer a separate copy. A branch name alone does not identify an immutable state.
4. Give the reviewer both the previous endpoint and the repair endpoint. Report the actual directory and content examined, so a Pass cannot accidentally refer to the original checkout.
5. Keep the repair worktree until its changes and evidence are preserved. Integrating, committing, pushing, or removing it follows the user's existing permissions; review acceptance alone grants none of these. If integration changes content or relevant execution inputs, verify the resulting state and report any evidence gap without silently adding another review pass.

## Copyable pointer

The copyable prompt names the role, cycle, pass and handoff file; everything else lives in the file. Limit it to two or three lines. Use an absolute handoff path in chat when the recipient's working directory is uncertain, or a relative path with an explicit base directory. Do not invoke a mutable installed skill in place of the governing version referenced by the handoff.

```text
Act as [ROLE] for cycle [ID]; pass [N] of [MAX] completed, [NEXT PASS OR NONE].
Read [HANDOFF_FILE] and use the pinned instructions it identifies.
```

Fill the fields before delivery. The same pointer works for manual transfer and authorized agent dispatch; role-specific operating rules stay in the skill. Report closure contains a link to the handoff, not another copy of this prompt or its contents.

For Incomplete before the ceiling, the handoff requests the missing evidence or decision when no defect has been established. Resume review only after that input is resolved and a pass remains. A Pass ends the normal cycle early; an unused pass is not work to be performed.
