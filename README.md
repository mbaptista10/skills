# Skills

Skills para [Claude Code](https://docs.anthropic.com/en/docs/claude-code) que estendem as capacidades do agente com workflows especializados.

## Skills disponíveis

| Skill | Descrição | Instalar |
|-------|-----------|----------|
| **semantic-commits** | Analisa arquivos staged, agrupa semanticamente por relação lógica e cria commits convencionais separados para cada grupo. | `npx skills add mbaptista10/skills --skill semantic-commits` |
| **semantic-pr** | Cria ou edita Pull Requests analisando commits da branch, coletando contexto e gerando PRs estruturadas com prefixo conventional commit no título. | `npx skills add mbaptista10/skills --skill semantic-pr` |

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
