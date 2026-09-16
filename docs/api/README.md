# API

## Convenções

A API usa JSON, UUIDs e datas ISO 8601 em UTC. Rotas protegidas exigem sessão e contexto de organização derivados no servidor.

## Resposta de sucesso

```json
{
  "data": {},
  "meta": {"requestId": "req_..."}
}
```

## Resposta de erro

```json
{
  "error": {
    "code": "LEAD_NOT_FOUND",
    "message": "Lead não encontrado.",
    "requestId": "req_...",
    "details": {}
  }
}
```

Não expor stack trace, SQL, token ou dados de outro tenant.

## Endpoints principais

```text
GET/POST       /api/leads
GET/PATCH      /api/leads/:id
POST           /api/leads/:id/assign
POST           /api/leads/:id/score
POST           /api/leads/:id/next-action
GET            /api/leads/:id/timeline

GET/POST       /api/vehicles
GET/PATCH      /api/vehicles/:id
POST           /api/vehicles/import

GET            /api/conversations
GET            /api/conversations/:id
GET/POST       /api/conversations/:id/messages
POST           /api/conversations/:id/summarize
POST           /api/conversations/:id/draft-reply
POST           /api/conversations/:id/takeover

GET/POST       /api/appointments
GET/POST       /api/tasks
PATCH          /api/tasks/:id

GET            /api/plans
GET            /api/billing/subscription
POST           /api/billing/checkout
POST           /api/billing/upgrade
POST           /api/billing/cancel
GET            /api/billing/invoices
GET            /api/billing/usage/current
GET            /api/billing/usage/history

GET/POST       /api/templates
POST           /api/follow-ups/preview
POST           /api/follow-ups/schedule

GET            /api/dashboard/summary
GET            /api/dashboard/funnel
GET            /api/dashboard/usage

GET/POST       /api/webhooks/whatsapp
POST           /api/webhooks/billing
```

## Idempotência e paginação

Webhooks, billing, importação e uso aceitam `Idempotency-Key`. Coleções grandes utilizam cursor e limite máximo configurado.

## Autorização

Cada caso de uso declara permissão. Exemplos: `lead:read:assigned`, `lead:read:all`, `lead:assign`, `billing:manage`, `billing:read`, `audit:read`, `conversation:send` e `integration:manage`.

## Documentação

Gerar OpenAPI a partir dos schemas ou manter contrato versionado em `openapi.yaml`. Veja também [erros de API](errors.md).
