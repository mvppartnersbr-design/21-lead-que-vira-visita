# Runbook operacional

## Ambientes

Manter desenvolvimento, staging e produção separados. Credenciais, bancos, filas, buckets e webhooks devem ser diferentes por ambiente.

## Deploy

1. Executar lint, typecheck e testes.
2. Construir imagens versionadas.
3. Rodar migrations de forma controlada.
4. Fazer deploy da aplicação e workers.
5. Verificar health e readiness.
6. Conferir filas e webhooks.
7. Executar smoke test.
8. Observar erros e métricas.

## Observabilidade

Registrar logs estruturados com request ID e tenant redigido. Monitorar:

- erro HTTP;
- tempo de resposta;
- falha de webhook;
- jobs pendentes;
- dead-letter queue;
- atraso de follow-up;
- falha de billing;
- custo estimado por tenant;
- consumo acima da projeção;
- erro de provider de IA;
- erro de autenticação.

## Backups

O PostgreSQL precisa de backup automatizado e restauração testada. Storage deve possuir política de retenção e versionamento quando apropriado. Backups não devem ficar acessíveis pelo mesmo segredo da aplicação.

## Incidentes

Classificar em P0 indisponibilidade ampla ou risco de vazamento, P1 falha de operação importante e P2 degradação limitada. Para qualquer incidente:

1. identificar;
2. conter;
3. preservar evidências;
4. comunicar responsáveis;
5. corrigir;
6. validar restauração;
7. registrar postmortem;
8. criar ação preventiva.

## Circuit breaker

Se IA, WhatsApp ou billing falharem, o sistema deve degradar de forma controlada. A captação e a operação manual devem continuar quando possível. Nunca enviar mensagens duplicadas por causa de retry.

## Billing em falha

Se o webhook de pagamento falhar, registrar evento pendente, tentar novamente e alertar plataforma. Não cancelar assinatura automaticamente por um único erro de webhook.

## Checklist de produção

- [ ] Backup restaurado em teste.
- [ ] Secrets em secret manager.
- [ ] Webhooks validados.
- [ ] Rate limits ativos.
- [ ] Tenants isolados.
- [ ] Logs sem dados desnecessários.
- [ ] Health checks ativos.
- [ ] Alertas configurados.
- [ ] Provider mock desligado em produção.
- [ ] Opt-out testado.
- [ ] Billing idempotente.
- [ ] Processo de rollback documentado.
- [ ] Contatos de incidente definidos.
