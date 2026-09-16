# Integrações

## Princípio

Cada integração externa deve ter um contrato interno, um adapter isolado, configuração tipada, health check e provider mock. O domínio não deve conhecer detalhes do fornecedor.

## WhatsApp

Provider oficial ou BSP oficial. Deve suportar envio de texto, templates, marcação de leitura, verificação de webhook, status de entrega e parsing de eventos.

Variáveis esperadas:

```text
WHATSAPP_PROVIDER
WHATSAPP_PHONE_NUMBER_ID
WHATSAPP_BUSINESS_ACCOUNT_ID
WHATSAPP_ACCESS_TOKEN
WHATSAPP_VERIFY_TOKEN
WHATSAPP_APP_SECRET
```

Nenhuma variável real deve aparecer no Git ou em respostas da API.

## IA

O provider de IA deve suportar classificação, extração, resumo, rascunho e handoff. A resposta deve ser validada por schema. O sistema deve registrar modelo, tokens, custo estimado e confiança sem enviar dados desnecessários.

Variáveis esperadas:

```text
AI_PROVIDER
AI_API_KEY
AI_MODEL_CLASSIFICATION
AI_MODEL_GENERATION
AI_MAX_INPUT_TOKENS
AI_MAX_OUTPUT_TOKENS
```

## Billing

O adapter de billing deve suportar cliente, assinatura, invoice, cancelamento e webhook. O modo mock deve simular pagamento, atraso, cancelamento e upgrade.

Variáveis esperadas:

```text
BILLING_PROVIDER
BILLING_API_KEY
BILLING_WEBHOOK_SECRET
BILLING_ENVIRONMENT
```

## Estoque

O primeiro MVP pode usar importação CSV/JSON. Uma integração futura deve consultar estoque atualizado, mas nenhuma resposta de disponibilidade pode ser baseada em cache vencido sem aviso.

## Calendário

Integração futura deve suportar disponibilidade, criação, alteração e cancelamento. O sistema interno continua sendo fonte de auditoria do agendamento.

## n8n

O n8n pode executar workflows de automação, mas não deve ser a fonte de verdade de leads, billing, permissões ou auditoria. O acesso deve ser isolado, protegido e compatível com os termos do fornecedor. O produto deve manter provider mock e workflows idempotentes.

## Falhas

Toda integração deve definir timeout, retry, backoff, circuit breaker, dead-letter queue, status de saúde e mensagem acionável para o operador.
