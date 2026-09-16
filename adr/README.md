# Architecture Decision Records

Cada ADR registra uma decisão relevante, o contexto, alternativas, decisão e consequências. Não apagar decisões antigas; marcar como substituídas quando necessário.

## Template

```markdown
# ADR-NNN — Título

## Status

Proposed | Accepted | Deprecated | Superseded

## Contexto

Qual problema exige decisão?

## Alternativas

Quais opções foram consideradas?

## Decisão

O que foi escolhido?

## Consequências

Quais benefícios, custos e riscos surgem?

## Data

YYYY-MM-DD
```

## ADRs atuais

As decisões iniciais estão documentadas em [`docs/architecture/decisions.md`](../docs/architecture/decisions.md). Novas decisões sobre provider, billing, armazenamento, segurança ou multi-tenancy devem receber um ADR próprio antes de alterar o contrato do sistema.
