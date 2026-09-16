# Modelo de dados

## Convenções

Todos os IDs públicos são UUIDs. Todos os timestamps são armazenados em UTC. Entidades de negócio possuem `organization_id`. Exclusões preferem soft delete quando houver necessidade de auditoria ou retenção.

## Entidades principais

| Entidade | Responsabilidade |
|---|---|
| `Organization` | Tenant da loja ou grupo |
| `User` | Usuário e papel dentro do tenant |
| `Vehicle` | Veículo do estoque |
| `Lead` | Oportunidade comercial |
| `TradeInVehicle` | Veículo oferecido em troca |
| `Conversation` | Conversa por canal |
| `Message` | Mensagem individual |
| `LeadEvent` | Mudança de estado ou evento operacional |
| `Task` | Próxima ação |
| `Appointment` | Visita, test-drive ou contato agendado |
| `MessageTemplate` | Modelo aprovado de mensagem |
| `Plan` | Configuração de preço e franquia |
| `Subscription` | Plano contratado pelo tenant |
| `UsagePeriod` | Período de consumo |
| `UsageEvent` | Unidade técnica/comercial consumida |
| `Invoice` | Registro de cobrança |
| `AuditLog` | Trilha de auditoria |

## Estados de lead

```text
new → researching → interested → qualified → hot
                         ↓          ↓         ↓
                      nurture    scheduled → visited → proposal → won/lost
```

Um lead também pode ir para `opted_out`. A transição deve ser registrada como evento.

## Invariantes

1. Lead ativo deve possuir organização.
2. Usuário só pode atuar em organizações autorizadas.
3. Veículo reservado ou vendido não pode ser confirmado como disponível.
4. Lead atribuído deve possuir usuário ou fila válida.
5. Agendamento precisa ter data futura no momento da criação.
6. Evento de uso precisa possuir `idempotency_key` única por organização.
7. Invoice precisa apontar para período e assinatura válidos.
8. `estimated_value_cents` de troca nunca equivale automaticamente ao valor aprovado.
9. Mensagem promocional não pode ser enviada para lead com opt-out.
10. Dados confirmados manualmente não podem ser sobrescritos pela IA sem revisão.

## Índices recomendados

- `organization_id` em todas as entidades de tenant;
- `organization_id, status` em leads;
- `organization_id, next_action_at` em tarefas;
- `organization_id, created_at` em eventos;
- `organization_id, period_start, period_end` em uso;
- `external_message_id` em mensagens;
- `idempotency_key` em eventos de uso;
- `provider_invoice_id` em invoices;
- `assigned_to, status` em leads e tarefas.

## Retenção

A retenção deve ser configurável por organização, respeitando obrigações contratuais e políticas de privacidade. Conversas e anexos devem ter regras de retenção separadas de métricas agregadas. Ao excluir dados pessoais, preserve somente o mínimo necessário para auditoria agregada e cobrança, conforme política aprovada.
