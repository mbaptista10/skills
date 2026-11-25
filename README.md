<div align="center">

[![skills.sh](https://skills.sh/b/mbaptista10/skills)](https://skills.sh/mbaptista10/skills)

</div>

---

# Skills

Skills para [Claude Code](https://docs.anthropic.com/en/docs/claude-code) que estendem as capacidades do agente com workflows especializados.

## Skills disponíveis

| Skill | Descrição | Instalar |
|-------|-----------|----------|
| **semantic-commits** | Analisa arquivos staged, agrupa semanticamente por relação lógica e cria commits convencionais separados para cada grupo. | `npx skills add mbaptista10/skills --skill semantic-commits` |
| **semantic-pr** | Cria ou edita Pull Requests analisando commits da branch, coletando contexto e gerando PRs estruturadas com prefixo conventional commit no título. | `npx skills add mbaptista10/skills --skill semantic-pr` |
| **use-ship** | Fluxo completo de entrega: cria nova branch, roda semantic-commits, sincroniza com a branch default do origin (rebase, com fallback para merge), faz push e roda semantic-pr. Opcionalmente acompanha o PR (CI, conflitos, comentários, aprovações, pedidos de mudança e merge), propondo correções e só agindo após aprovação. | `npx skills add mbaptista10/skills --skill use-ship` |

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
- `/use-ship` — Fluxo completo: branch + commits + sync + push + PR + watch opcional do PR

> **Por que `use-ship` e não `ship`?** O `/ship` se tornou um comando nativo do Claude Code. Para não conflitar, a skill foi renomeada para `use-ship`. Quem instalou a versão antiga pode remover a skill `ship` e instalar a `use-ship`.
