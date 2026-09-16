# Erros de API

## Princípio

Erros devem ser previsíveis, acionáveis e seguros. Não expor stack trace, tokens, SQL ou informações de outro tenant.

## Códigos

| Código | HTTP | Significado |
|---|---:|---|
| `AUTH_REQUIRED` | 401 | Sessão ausente ou inválida |
| `FORBIDDEN` | 403 | Usuário sem permissão |
| `NOT_FOUND` | 404 | Recurso inexistente no escopo permitido |
| `VALIDATION_ERROR` | 422 | Input inválido |
| `CONFLICT` | 409 | Estado incompatível ou idempotência conflitante |
| `RATE_LIMITED` | 429 | Limite de requisições |
| `PROVIDER_UNAVAILABLE` | 503 | Serviço externo indisponível |
| `USAGE_LIMIT_REACHED` | 402/409 | Limite comercial atingido |
| `INTERNAL_ERROR` | 500 | Erro inesperado |

## Mensagens

A mensagem pública deve explicar a ação possível. Detalhes técnicos vão para logs com request ID.

## Retry

Clientes podem repetir em `PROVIDER_UNAVAILABLE` e falhas transitórias. Não repetir automaticamente em `VALIDATION_ERROR`, `FORBIDDEN`, `USAGE_LIMIT_REACHED` ou conflitos sem nova decisão.

## Idempotência

Webhooks, billing, envio de mensagem, importação e eventos de consumo exigem `Idempotency-Key`. Uma repetição válida deve retornar o resultado anterior quando possível.
