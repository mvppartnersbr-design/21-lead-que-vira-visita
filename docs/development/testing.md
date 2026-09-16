# Estratégia de testes

## Pirâmide

A maior parte dos testes deve ser unitária, seguida por integração e poucos fluxos end-to-end críticos.

| Camada | Objetivo |
|---|---|
| Unitário | Score, políticas, schemas, permissões, billing e transições |
| Integração | Banco, filas, providers mock, webhooks e isolamento |
| E2E | Jornadas críticas do usuário |
| Segurança | Acesso cruzado, secrets, opt-out, uploads e abuso |

## Casos unitários

Testar score de qualificação, uso incluído, excedente, teto, arredondamento, cálculo de invoice, idempotência, transição de lead, opt-out, handoff, limite de IA e autorização por papel.

## Casos de integração

Testar recebimento de webhook, criação idempotente de mensagem, atualização de lead, fila de IA, evento de uso, alerta de franquia, criação de tarefa, agendamento, billing webhook e retry.

## Casos end-to-end

1. criar organização;
2. convidar vendedor;
3. importar veículos;
4. receber lead;
5. qualificar;
6. distribuir;
7. assumir conversa;
8. agendar visita;
9. registrar follow-up;
10. atingir 80% de uso;
11. atingir teto;
12. fazer upgrade;
13. cancelar;
14. exportar dados.

## Testes de segurança

Tentar consultar lead de outra organização, modificar billing sem permissão, usar webhook inválido, enviar mensagem após opt-out, inserir XSS, fazer upload de tipo proibido, reenviar evento já processado, acessar credencial pela API e abusar de rate limit.

## Contratos

Providers de WhatsApp, IA e billing devem possuir contract tests. O mock deve respeitar o mesmo contrato externo do adapter real.

## Dados e snapshots

Não versionar dados reais. Fixtures devem ser fictícias, mínimas e reproduzíveis. Não incluir telefones reais, tokens, conversas reais ou documentos.
