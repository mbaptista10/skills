# Rules for Filling the Project's PR Template

When the user chose to use the project's PR template, use its structure as the body skeleton and fill each section using the collected context (commits, user explanation, Jira link, diff stats).

1. Free-text sections (e.g., "Description", "Summary", "What changed"): synthesize the user's explanation with the commit analysis.
2. Sections that list changes: map commits to list items (one item per commit, rewritten to be clear and human-readable in PT-BR).
3. Checkbox sections (e.g., "Checklist"): leave as-is for the user to check manually.
4. Jira/ticket link sections: fill if provided; omit or leave empty otherwise.
5. Sections that don't map to any collected info: leave with `<!-- preencher manualmente -->`.
6. The conventional commit title rules still apply regardless of which template is used.
