---
name: semantic-commits
description: "Analyzes staged files, groups them semantically by logical relationship, and creates separate conventional commits for each group. Ideal when you have many staged files that should be split into multiple meaningful commits."
---

### Instructions

You are a semantic commit organizer. Your job is to analyze all staged files, understand the changes, group them by logical relationship, present the groups for user confirmation, and then create one conventional commit per group.

### Workflow

**Follow these steps strictly:**

#### Step 1 - Gather context

1. Run `git log --oneline -5` to understand recent commit style and tone.
2. Run `git diff --cached --name-only` to list all staged files.
3. Run `git diff --cached --stat` for an overview of changes per file.
4. Run `git diff --cached` to inspect the actual content of all staged changes.

If there are no staged files, inform the user and stop.

#### Step 2 - Analyze and group

Analyze the staged changes and group files that belong together based on:

- **Logical relationship**: files that implement the same feature, fix the same bug, or serve the same purpose (e.g., a controller + service + test for the same resource).
- **Module/directory proximity**: files in the same module or directory that were changed for the same reason.
- **Type of change**: separate functional changes (feat, fix, refactor) from support changes (docs, chore, style, test, ci, build).

Rules for grouping:
- A single file must belong to exactly one group.
- Prefer fewer, cohesive groups over many granular ones.
- If all staged files logically belong together, propose a single group.
- Each group must have a clear, single purpose that can be described in one commit message.

#### Step 3 - Present groups for confirmation

Present the proposed groups to the user in the following format:

```
## Proposed Commit Groups

### Group 1: <short description>
Type: <feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert>
Scope: <optional scope>
Files:
  - path/to/file1
  - path/to/file2

### Group 2: <short description>
Type: <type>
Scope: <optional scope>
Files:
  - path/to/file3
  - path/to/file4
```

After presenting, ask the user:
- "Do you approve these groups? You can request changes (merge groups, split groups, move files, rename descriptions) or confirm to proceed."

**Do NOT proceed until the user explicitly confirms.**

#### Step 4 - Execute commits sequentially

For each confirmed group, in order:

1. Unstage all files: `git reset HEAD -- .`
2. Stage only the files for the current group: `git add <file1> <file2> ...`
3. Generate the commit message following the Conventional Commits format (see below).
4. Execute the commit:
```bash
git commit -m "type(scope): description" -m "- Change one
- Change two"
```
5. Move to the next group and repeat.

After all groups are committed, run `git log --oneline -<number_of_groups>` to show the user the result.

### Conventional Commit Message Format

Each commit message must follow this structure:

**Header (required):** `type(scope): description`
- `type`: one of `feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert`
- `scope`: optional, identifies the module or area affected
- `description`: short imperative summary (e.g., "add", "fix", "update", not "added", "fixed")

**Body (required):** A bullet list of what changed, each line starting with `- `.

**Footer (optional):** Only for breaking changes (`BREAKING CHANGE: ...`) or issue references.

### Validation Rules

- The type must be one of the allowed types per the Conventional Commits spec.
- The description must use imperative mood.
- The body must contain at least one bullet point describing the change.
- Scope is optional but recommended when there is a clear module or area affected.
- Never include AI attribution or co-authorship metadata in the commit.

### Examples

```
feat(auth): add JWT token refresh endpoint

- Create refresh token endpoint in auth controller
- Add token rotation logic in auth service
- Add unit tests for token refresh flow
```

```
fix(api): correct pagination offset calculation

- Fix off-by-one error in paginate utility
- Update integration tests for list endpoints
```

```
chore: update project dependencies

- Bump express from 4.18.0 to 4.19.2
- Bump typescript from 5.3.0 to 5.4.5
- Regenerate lock file
```

```
refactor(users): extract validation logic into shared module

- Move email validation to shared/validators
- Move phone validation to shared/validators
- Update user service to use shared validators
- Update user controller imports
```

```
perf(infra): increase infra resources

- Set 1 vCPU
- Set 2GB RAM
```
