# Skills

Skills para [Claude Code](https://docs.anthropic.com/en/docs/claude-code) que estendem as capacidades do agente com workflows especializados.

## Skills disponíveis

| Skill | Descrição | Instalar |
|-------|-----------|----------|
| **semantic-commits** | Analisa arquivos staged, agrupa semanticamente por relação lógica e cria commits convencionais separados para cada grupo. | `npx @anthropic-ai/claude-code-skills install mbaptista10/skills/semantic-commits` |
| **semantic-pr** | Cria ou edita Pull Requests analisando commits da branch, coletando contexto e gerando PRs estruturadas com prefixo conventional commit no título. | `npx @anthropic-ai/claude-code-skills install mbaptista10/skills/semantic-pr` |

## Instalacao

```bash
npx @anthropic-ai/claude-code-skills install mbaptista10/skills/<skill-name>
```

## Uso

Apos instalar, as skills ficam disponiveis como slash commands no Claude Code:

- `/semantic-commits` — Agrupa e commita arquivos staged semanticamente
- `/semantic-pr` — Cria/edita PRs com estrutura padronizada
