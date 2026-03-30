---
name: project-questions
description: "Atua como code reviewer e tech lead para analisar toda a codebase de um projeto, gerando um QUESTIONS.md com perguntas arquiteturais, de refatoração, segurança e performance organizadas por categoria. Após o usuário responder as perguntas, gera um IMPROVEMENTS.md com o plano de melhorias. Esta skill deve ser usada quando o usuário quiser uma revisão completa do projeto ou quando invocar /project-questions."
---

# Project Questions

Skill para revisão completa de codebase atuando como code reviewer e tech lead profissional. Todo o output deve ser em **pt-BR**.

## Fluxo de Trabalho

A skill opera em duas fases distintas.

### Detecção de Fase

- Se **não existe** `QUESTIONS.md` na raiz do projeto → executar **Fase 1**
- Se **existe** `QUESTIONS.md` com respostas preenchidas (campos `**Resposta:**` não vazios) → executar **Fase 2**
- Se **existe** `QUESTIONS.md` sem respostas → informar o usuário que precisa responder as perguntas primeiro

### Fase 1: Geração do QUESTIONS.md

1. **Descoberta do projeto** — Identificar a stack, framework, linguagem e estrutura do projeto:
   - Ler arquivos de configuração raiz (`package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`, `composer.json`, `Gemfile`, `pom.xml`, `build.gradle`, etc.)
   - Ler arquivos de configuração do framework (`.env.example`, `docker-compose.yml`, `Dockerfile`, `Makefile`, etc.)
   - Mapear a estrutura de diretórios com `ls` recursivo (limitado a 3 níveis de profundidade)

2. **Análise profunda** — Revisar cada arquivo relevante do projeto. Utilizar subagentes em paralelo (Agent tool com subagent_type=Explore) para acelerar a análise quando o projeto for grande. Cobrir:
   - Todos os arquivos de código fonte, não apenas os principais
   - Rotas/endpoints (se API)
   - Páginas/componentes (se frontend)
   - Models/schemas/migrations (se houver banco de dados)
   - Middlewares, guards, interceptors
   - Configurações de CI/CD, Docker, infra
   - Testes existentes (ou a falta deles)

3. **Geração do QUESTIONS.md** — Criar o arquivo na raiz do projeto com a estrutura abaixo. Usar apenas as categorias que tiverem perguntas; omitir categorias vazias.

```markdown
# QUESTIONS.md

> Revisão técnica completa do projeto. Responda cada pergunta diretamente abaixo dela.
> Após responder, execute `/project-questions` novamente para gerar o plano de melhorias.

## Arquitetura

### Q1: [Pergunta sobre decisão arquitetural]
**Resposta:**


### Q2: [Próxima pergunta]
**Resposta:**


## Seguranca

### QN: [Pergunta sobre vulnerabilidade ou prática insegura]
**Resposta:**


## Performance

### QN: [Pergunta sobre gargalo ou otimização]
**Resposta:**


## Refatoracao

### QN: [Pergunta sobre código duplicado, complexidade, etc.]
**Resposta:**


## Banco de Dados

### QN: [Pergunta sobre schema, queries, migrations]
**Resposta:**


## Testes

### QN: [Pergunta sobre cobertura, qualidade dos testes]
**Resposta:**


## Infraestrutura e DevOps

### QN: [Pergunta sobre deploy, CI/CD, Docker]
**Resposta:**


## Observacoes Gerais

### QN: [Outras observações relevantes]
**Resposta:**

```

#### Diretrizes para as perguntas

- Numerar as perguntas sequencialmente (Q1, Q2, Q3...) independente da categoria
- Cada pergunta deve ser independente e autocontida
- Incluir contexto: mencionar o arquivo, linha ou trecho de código relevante
- Ser específico: "Por que o endpoint `/api/users` não valida o campo `email`?" em vez de "Falta validação"
- Cobrir todos os aspectos: arquitetura, segurança, performance, refatoração, banco de dados, testes, infra
- Não ter medo de gerar muitas perguntas — quanto mais completo, melhor
- Apontar bugs encontrados como perguntas: "O handler em `src/api/users.go:45` não trata o erro de conexão com o banco. Isso é intencional ou é um bug?"
- Sempre que possível, sugerir o que parece ser o problema ou melhoria na própria pergunta para facilitar a resposta do usuário

### Fase 2: Geração do IMPROVEMENTS.md

Quando o usuário retornar com o `QUESTIONS.md` respondido (ou instruir para ler as respostas):

1. **Ler o QUESTIONS.md respondido** — Processar todas as respostas do usuário
2. **Classificar as respostas** — Identificar o que é bug, o que é comportamento intencional, o que precisa de melhoria
3. **Gerar o IMPROVEMENTS.md** na raiz do projeto com a seguinte estrutura:

```markdown
# IMPROVEMENTS.md

> Plano de melhorias baseado na revisão técnica do projeto.
> Cada item referencia a pergunta original do QUESTIONS.md.

## Prioridade Alta (Bugs e Seguranca)

### I1: [Título da melhoria] (ref: Q3)
**Problema:** [Descrição do problema identificado]
**Solucao:** [Descrição técnica da solução]
**Arquivos afetados:**
- `path/to/file.ts` — [o que mudar]


## Prioridade Media (Performance e Refatoracao)

### IN: [Título] (ref: QN)
**Problema:** [...]
**Solucao:** [...]
**Arquivos afetados:**
- [...]


## Prioridade Baixa (Melhorias e Boas Praticas)

### IN: [Título] (ref: QN)
**Problema:** [...]
**Solucao:** [...]
**Arquivos afetados:**
- [...]


## Itens Descartados

| Pergunta | Motivo |
|----------|--------|
| Q5 | Comportamento intencional conforme resposta do usuário |

```

#### Diretrizes para o plano de melhorias

- Referenciar sempre a pergunta original (ref: QN)
- Priorizar: bugs e segurança > performance > refatoração > boas práticas
- Ser específico nos arquivos afetados e no que precisa mudar em cada um
- Itens que o usuário indicou como comportamento intencional vão para "Itens Descartados"
- Numerar melhorias sequencialmente (I1, I2, I3...)
- Após gerar o arquivo, perguntar ao usuário se deseja iniciar a implementação das melhorias
