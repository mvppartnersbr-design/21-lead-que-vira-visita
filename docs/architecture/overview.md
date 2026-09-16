# Arquitetura geral

## Princípios

A arquitetura privilegia separação de domínio, isolamento multi-tenant, providers substituíveis, jobs idempotentes e evolução incremental. O produto deve funcionar em modo demo sem credenciais externas e migrar para provedores reais por adaptadores.

## Contexto

```text
[WhatsApp/Fontes de lead]
          |
          v
[Webhook/API de entrada]
          |
          v
[Normalização e idempotência]
          |
          v
[Domínio de Leads e Conversas] ---> [PostgreSQL]
          |
          +--> [Fila de IA] --------> [AIProvider]
          |
          +--> [Fila de Follow-up] -> [MessagingProvider]
          |
          +--> [Fila de Uso] -------> [UsageMeter]
          |
          v
[Dashboard / Inbox / Billing]
```

## Monorepo

```text
/apps
  /web       # Next.js, páginas, API e componentes
  /worker    # jobs assíncronos e integrações
/packages
  /db        # schema, migrations e acesso ao banco
  /shared    # tipos, schemas e constantes compartilhadas
  /ui        # componentes visuais reutilizáveis
  /integrations # WhatsApp, IA, billing, estoque e calendário
  /billing   # planos, uso, invoice e regras de consumo
  /config    # configuração tipada
```

## Camadas

### Interface

Responsável por renderização, interação, acessibilidade e estado de apresentação. Não deve decidir autorização nem executar regras comerciais diretamente.

### Aplicação

Coordena casos de uso, valida entrada, chama domínio e registra eventos. Exemplos: `QualifyLead`, `AssignLead`, `ScheduleAppointment`, `RecordUsage` e `UpgradeSubscription`.

### Domínio

Contém entidades, value objects, políticas e invariantes. O domínio não conhece Next.js, Prisma, n8n, Meta ou um fornecedor específico de IA.

### Infraestrutura

Implementa banco, filas, providers, storage, envio, billing e observabilidade.

## Fluxo de uma mensagem

1. O webhook valida autenticidade.
2. A mensagem recebe uma chave de idempotência.
3. O sistema encontra ou cria organização, contato, lead e conversa.
4. O evento é persistido.
5. Um job de processamento é enfileirado.
6. A IA extrai dados com schema validado.
7. O score é calculado por política.
8. O lead é roteado.
9. Um evento de uso é gravado.
10. Uma tarefa ou sugestão de resposta é criada.
11. O vendedor vê o contexto na inbox.

## Fluxo de billing

```text
Operação executada
      |
      v
UsageEvent idempotente
      |
      v
Período corrente da organização
      |
      +--> UsageAlert
      +--> OverageCalculation
      +--> Invoice
      +--> CircuitBreaker de automações caras
```

## Resiliência

Webhooks devem retornar rapidamente. Processamentos lentos entram em fila. Falhas transitórias usam retry com backoff. Falhas permanentes vão para dead-letter queue e geram alerta operacional. Toda operação externa deve possuir timeout, correlation ID e tratamento de duplicidade.

## Multi-tenancy

Cada consulta de negócio deve filtrar por `organization_id`. Serviços de aplicação devem receber o contexto de tenant de uma sessão autenticada ou de um evento validado. O sistema deve ter testes específicos para tentar acessar dados de outra organização.
