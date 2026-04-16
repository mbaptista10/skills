# PR Examples

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
