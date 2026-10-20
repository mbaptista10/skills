# PR Examples

**Title:**
```
feat(auth): adicionar fluxo de refresh token
```

**Body (with Jira link and impact):**
```markdown
## Contexto

Tokens de acesso expiram em 15 minutos e o cliente precisa refazer login para continuar a sessão.

## Solução

Novo endpoint de refresh com rotação de tokens: cada refresh token é de uso único e invalida o anterior. A validação foi centralizada no middleware de autenticação.

## Alterações

- Endpoint `POST /auth/refresh` no controller de autenticação
- Rotação e revogação de refresh tokens no service de auth
- Middleware de validação aceita o novo formato de token

## Como validar

- `npm test -- auth/refresh`
- Login, aguardar expiração do access token e chamar `/auth/refresh`: deve retornar novo par de tokens
- Reutilizar o refresh token anterior: deve retornar 401

## Impacto

Requer a env `REFRESH_TOKEN_TTL` em todos os ambientes antes do deploy.

Refs: https://company.atlassian.net/browse/AUTH-123
```

**Body (without Jira link and impact):**
```markdown
## Contexto

Endpoints de listagem retornavam registros duplicados quando o offset ultrapassava o total de registros.

## Solução

Correção do cálculo de limite no utilitário de paginação, compartilhado por todos os endpoints de listagem.

## Alterações

- Correção de off-by-one no utilitário de paginação
- Testes de integração cobrindo offset maior que o total

## Como validar

- `npm test -- pagination`
- `GET /items?offset=<total+1>` deve retornar lista vazia
```
