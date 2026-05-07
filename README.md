# Skills

Skills para [Claude Code](https://docs.anthropic.com/en/docs/claude-code) que estendem as capacidades do agente com workflows especializados.

[![skills.sh](https://skills.sh/b/mbaptista10/skills)](https://skills.sh/mbaptista10/skills)

## Skills disponíveis

| Skill | Descrição | Instalar |
|-------|-----------|----------|
| **semantic-commits** | Analisa arquivos staged, agrupa semanticamente por relação lógica e cria commits convencionais separados para cada grupo. | `npx skills add mbaptista10/skills --skill semantic-commits` |
| **semantic-pr** | Cria ou edita Pull Requests analisando commits da branch, coletando contexto e gerando PRs estruturadas com prefixo conventional commit no título. | `npx skills add mbaptista10/skills --skill semantic-pr` |
| **project-questions** | Analisa toda a codebase como code reviewer e tech lead, gera um QUESTIONS.md com perguntas organizadas por categoria e, após respostas, gera um IMPROVEMENTS.md com o plano de melhorias. | `npx skills add mbaptista10/skills --skill project-questions` |

## Instalacao

Instalar todas as skills:
```bash
npx skills add mbaptista10/skills
```

Instalar uma skill especifica:
```bash
npx skills add mbaptista10/skills --skill <skill-name>
```

## Uso

Apos instalar, as skills ficam disponiveis como slash commands no Claude Code:

- `/semantic-commits` — Agrupa e commita arquivos staged semanticamente
- `/semantic-pr` — Cria/edita PRs com estrutura padronizada
- `/project-questions` — Revisão completa do projeto com perguntas e plano de melhorias
