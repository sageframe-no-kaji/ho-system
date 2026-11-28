# ho-system — project instructions

Work in this repo is on the Ho System methodology itself. The rationale for the operating discipline is load-bearing here — edits to framework structure, operating discipline, or Kamae chain must be reasoned from first principles.

Read the [operating discipline rationale](practitioner/operating-discipline-rationale.md) before changing the framework or Kamae chain.

## Where changes are made

The practitioner's agents load this repository's skills and operating discipline straight from its main checkout, through links. A file saved there is in use by every new session at once, whether or not it has been committed.

So the main checkout stays on `main` and clean, and it is not where work happens:

- Make every change in a linked worktree on a branch, created as a visible sibling of the repository: `git worktree add ../ho-system-<track> -b <branch> origin/main`. Do not nest a worktree inside the repository or a hidden directory.
- Merge by pull request. The change goes live when the main checkout is updated to the merged `main`, and not before.
- Remove the worktree and its branch after the merge.
- The one exception is `.keisaku.json`, a generated state file that Keisaku rewrites in the main checkout. It may be committed there.

`.githooks/pre-commit` refuses other commits in the main checkout. It is active after `git config core.hooksPath .githooks`, run once in a clone. If the hook is not active, the rule still holds.

Some files are copied into a skill so that someone who installs the skill alone receives them: the operating discipline and its rationale into the personal-environment setup skill, and the license into the antagonist review skill. Edit the original, never the copy. `.githooks/check-copies` lists each pair and refuses a commit in which a copy differs from its original, printing the command that brings the copy into step. The pre-commit hook runs it, and so does a check on every pull request.

This applies to a checkout that is linked live. A clone used only for reading or for a separate installation has no such constraint.
