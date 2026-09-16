# Eventos de domínio

## Convenções

Eventos são imutáveis, versionados e carregam `event_id`, `event_type`, `event_version`, `organization_id`, `occurred_at`, `actor` e `payload` validado.

## Eventos de operação

```text
lead.created
lead.updated
lead.assigned
lead.scored
lead.stage_changed
lead.next_action_created
conversation.created
message.received
message.sent
message.delivery_updated
appointment.created
appointment.confirmed
appointment.canceled
handoff.requested
```

## Eventos de uso

```text
usage.event_recorded
usage.threshold_reached
usage.overage_created
usage.period_closed
```

## Eventos de billing

```text
subscription.created
subscription.updated
subscription.upgraded
subscription.downgraded
subscription.canceled
invoice.created
invoice.paid
invoice.payment_failed
invoice.voided
```

## Idempotência

Consumers devem armazenar `event_id` ou chave equivalente. Um evento repetido não pode duplicar lead, mensagem, tarefa, cobrança ou consumo.

## Outbox

Quando consistência for necessária, persistir alteração de domínio e evento em transação no outbox. Um worker publica ou processa o evento com retry e dead-letter queue.

## Privacidade

Payloads devem conter apenas dados necessários. Não incluir token, chave, documento ou conteúdo integral de conversa em eventos operacionais se um identificador ou resumo redigido for suficiente.
