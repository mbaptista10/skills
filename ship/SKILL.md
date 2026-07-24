---
name: ship
description: "Orchestrates the full delivery flow from staged files to open PR: creates a new branch, runs semantic-commits, syncs with the origin default branch (rebase, falling back to merge), pushes, and runs semantic-pr. Use when the user wants to ship staged changes end-to-end in one command."
---

### Force mode

If the user passes `--force`, skip the branch-name confirmation in Step 2 and create the derived branch directly. The invoked skills already run in force mode regardless.

### Prerequisite

This skill chains `semantic-commits` and `semantic-pr`. Both must be installed. It operates only on **already staged** files — it never runs `git add`.

### Workflow

#### Step 1 - Gather context and validate

Run in parallel:
- `git status --short`
- `git diff --cached --stat`

If nothing is staged, inform the user that there is nothing to ship and stop.

#### Step 2 - Create the new branch

1. Derive a branch name from the staged changes following `type/short-description` (kebab-case), e.g. `feat/pagamento-pix`, `fix/timeout-fila`. Infer the `type` from the nature of the changes and the description from the affected module/feature.
2. Present the derived name and ask for confirmation (accept a different name from the user). **Skip this confirmation if `--force` was passed.**
3. Create and switch: `git checkout -b <branch>` from the current HEAD.

#### Step 3 - Commit

Invoke `/semantic-commits --force`. Let it group and commit the staged files.

If it produces no commits (e.g. staging became empty), stop and report.

#### Step 4 - Sync with the origin default branch

1. `git fetch origin`.
2. Detect the default branch: read `git symbolic-ref refs/remotes/origin/HEAD --short` (yields `origin/<base>`). If it fails, fall back to `origin/main` if it exists, otherwise `origin/master`.
3. Attempt rebase: `git rebase origin/<base>`.
4. **If the rebase fails or hits conflicts**: run `git rebase --abort`, then attempt `git merge --no-edit origin/<base>`.
5. **If the merge also conflicts**: stop immediately. Report the conflicting files (`git diff --name-only --diff-filter=U`) and ask the user to resolve them manually. **Do NOT push or open a PR with a dirty/conflicted tree.**

Record whether a rebase succeeded — it changes the push in Step 5.

#### Step 5 - Push

- If Step 4 rebased successfully (history rewritten): `git push -u origin <branch> --force-with-lease`.
- Otherwise (fresh branch or merge): `git push -u origin <branch>`.

#### Step 6 - Open the PR

Invoke `/semantic-pr --force`. It creates the PR against the repository default branch and prints the URL.

### Rules

- Never run `git add` — only staged files are shipped.
- Never open a PR when the working tree has unresolved conflicts.
- Prefer rebase; only fall back to merge when rebase cannot complete cleanly.
- Never include AI attribution or co-authorship metadata (inherited by the invoked skills).
