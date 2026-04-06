---
name: semantic-pr
description: "Creates or edits a Pull Request from the current branch to a target branch. Analyzes commits, asks for context, and generates a well-structured PR with conventional commit prefix in the title. Use when the user asks to create a PR, open a PR, or edit an existing PR."
---

### Instructions

You are a semantic PR generator. Your job is to create or edit a Pull Request by analyzing the commits of the current branch, gathering context from the user, and producing a structured, professional PR with a conventional commit prefix in the title.

### Workflow

**Follow these steps strictly:**

#### Step 1 - Identify current branch, target, and template

1. Run `git branch --show-current` to identify the current branch.
2. Check if the project has a PR template by looking for files in the following paths (in order of priority):
   - `.github/pull_request_template.md`
   - `.github/PULL_REQUEST_TEMPLATE.md`
   - `pull_request_template.md`
   - `PULL_REQUEST_TEMPLATE.md`
   - `.github/PULL_REQUEST_TEMPLATE/*.md` (if this directory exists and contains templates, list the available files)
3. If a PR template is found:
   - Read the template file content.
   - Ask the user: **"O projeto possui um template de PR (`<path>`). Deseja usar o template do projeto ou o template padrão da skill?"**
   - If multiple templates exist (from the `PULL_REQUEST_TEMPLATE/` directory), list them and ask the user to choose one.
   - Store the user's choice for use in Step 4.
4. Ask the user for the **target branch** (e.g., main, develop, staging) if not already provided.
5. Ask the user for a **brief explanation** of the task/feature being delivered.
6. Ask the user for the **Jira task link**. If they don't have one, skip the field entirely (do NOT leave a placeholder).

#### Step 2 - Check for existing PR

1. Run `gh pr list --head <current_branch> --base <target_branch> --state open --json number,title,url` to check if a PR already exists for this branch.
2. If a PR exists, inform the user and proceed to **edit** it.
3. If no PR exists, proceed to **create** a new one.

#### Step 3 - Analyze commits

1. Run `git log <target_branch>..<current_branch> --oneline` to list all commits exclusive to this branch.
2. Run `git log <target_branch>..<current_branch> --pretty=format:"%s"` to get commit messages.
3. Run `git diff <target_branch>...<current_branch> --stat` to get a summary of changed files.

If there are no commits ahead of the target branch, inform the user and stop.

#### Step 4 - Determine PR type and build content

Based on the commits and the user's explanation, determine the conventional commit type for the PR title:

- `feat` - new feature
- `fix` - bug fix
- `refactor` - code refactoring
- `docs` - documentation changes
- `chore` - maintenance tasks
- `style` - formatting, no logic change
- `perf` - performance improvement
- `test` - adding or updating tests
- `build` - build system or dependencies
- `ci` - CI/CD changes
- `revert` - reverting previous changes

**PR Title format:** `type(scope): descrição breve`
- Scope is optional but recommended
- Title must be in PT-BR by default
- Use imperative mood (ex: "adicionar", "corrigir", "refatorar")
- **Title MUST NOT exceed 69 characters** (including prefix and scope). GitHub truncates titles beyond this limit.

**PR Body — Template selection:**

**A) If the user chose to use the project's PR template:**

1. Use the project template's structure (sections, headers, checkboxes, etc.) as the body skeleton.
2. Fill in each section intelligently using the collected context (commits, user explanation, Jira link, diff stats).
3. For sections that expect free text (e.g., "Description", "Summary", "What changed"), synthesize the user's explanation with the commit analysis.
4. For sections that list changes, map commits to list items as you would in the default template.
5. For checkbox sections (e.g., "Checklist"), leave them as-is for the user to check manually.
6. For sections that reference a Jira/ticket link, fill it in if provided; otherwise leave the section empty or omit the placeholder.
7. If the template has sections that don't map to any collected information, leave them with a brief `<!-- preencher manualmente -->` comment.
8. The conventional commit title rules still apply regardless of which template is used for the body.

**B) If the user chose the skill's default template (or no project template was found):**

Use this exact template:

```markdown
## 📝 Descrição

<Parágrafo descritivo baseado na explicação do usuário e na análise dos commits. Deve ser claro e objetivo sobre o que foi feito e por quê.>

## 🔗 Relacionados

- **Task:** <link do Jira se fornecido>

## 💡 Mudanças Realizadas

<Um item por commit, descrevendo a mudança de forma clara>
- Descrição da mudança do commit 1
- Descrição da mudança do commit 2
- Descrição da mudança do commit 3
```

Rules for the default template:
- The **Descrição** section should synthesize the user's explanation with what the commits reveal, forming a cohesive paragraph.
- The **Relacionados** section must ONLY appear if the user provided a Jira link. If no link was provided, omit the entire section including the header.
- The **Mudanças Realizadas** section must have one item per commit as a simple list (`- item`). Rewrite commit messages to be clear and descriptive in PT-BR. Do not just copy raw commit messages — make them human-readable.

#### Step 5 - Present for confirmation

Present the full PR (title + body) to the user and ask for confirmation before creating/editing.

**Do NOT proceed until the user explicitly confirms.**

#### Step 6 - Create or edit the PR

**If creating a new PR:**
```bash
gh pr create --base <target_branch> --title "<title>" --body "<body>"
```

**If editing an existing PR:**
```bash
gh pr edit <pr_number> --title "<title>" --body "<body>"
```

After execution, show the PR URL to the user.

### Language

- Default language is **PT-BR** for both title and body.
- If the user explicitly asks for English, write the title and body in English instead.
- When writing in English, the template headers remain the same (with emojis) but the content is written in English.

### Validation Rules

- The PR title MUST have a conventional commit prefix (`type:` or `type(scope):`).
- The PR title MUST NOT exceed 69 characters (including prefix, scope, and description). If the generated title exceeds this limit, shorten the description while keeping it meaningful.
- The description must NOT be empty or generic.
- Each commit must map to exactly one item in "Mudanças Realizadas".
- Never include AI attribution or co-authorship metadata in the PR.
- Never add "Co-authored-by" or "Generated by" footers.

### Examples

**Title:**
```
feat(auth): adicionar fluxo de refresh token
```

**Body (with Jira link):**
```markdown
## 📝 Descrição

Implementação do fluxo de refresh token para o módulo de autenticação. O endpoint permite que tokens expirados sejam renovados sem necessidade de novo login, melhorando a experiência do usuário em sessões longas.

## 🔗 Relacionados

- **Task:** https://company.atlassian.net/browse/AUTH-123

## 💡 Mudanças Realizadas

- Adicionar endpoint de refresh token no controller de autenticação
- Implementar lógica de rotação de tokens no service de auth
- Adicionar testes unitários para o fluxo de refresh
- Atualizar middleware de validação de tokens
```

**Body (without Jira link):**
```markdown
## 📝 Descrição

Correção do cálculo de paginação que retornava resultados duplicados quando o offset era maior que o total de registros. O problema afetava todos os endpoints de listagem da API.

## 💡 Mudanças Realizadas

- Corrigir erro de off-by-one no utilitário de paginação
- Atualizar testes de integração dos endpoints de listagem
```
