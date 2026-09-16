# Deploy e rollback

## Ambientes

Manter desenvolvimento, staging e produção isolados em banco, storage, Redis, providers e webhooks.

## Pipeline

1. Instalar dependências com lockfile.
2. Executar lint e typecheck.
3. Executar testes unitários e de integração.
4. Construir artefatos.
5. Executar migrations compatíveis.
6. Fazer deploy web e worker.
7. Verificar health e readiness.
8. Executar smoke tests.
9. Observar erros e filas.

## Migrations

Migrations devem ser backward-compatible sempre que possível. Primeiro adicionar estrutura, depois migrar dados, depois remover campos antigos em release separada.

## Rollback

Definir rollback de aplicação e de configuração. Não reverter banco automaticamente sem avaliar eventos já processados. Em caso de billing ou webhook, preservar idempotência e reconciliar eventos antes de reprocessar.

## Release checklist

- [ ] Tests passed.
- [ ] Migration reviewed.
- [ ] Secrets configured.
- [ ] Webhooks apontam para ambiente correto.
- [ ] Provider mock desligado em produção.
- [ ] Backups recentes.
- [ ] Plano de rollback definido.
- [ ] Monitoramento ativo.
