# Billing, planos e uso

## Princípio

A loja compra previsibilidade e operação. O sistema cobra mensalidade, inclui uma franquia de uso e cobra excedente conforme regra publicada.

O cliente não deve precisar entender tokens. A plataforma, entretanto, precisa medir tokens, chamadas, retries e custo técnico para proteger margem.

## Planos seed

| Plano | Leads incluídos | Mensalidade | Excedente | Teto |
|---|---:|---:|---:|---:|
| Essencial | 150 | R$ 2.190 | R$ 7,90 | 225 |
| Comercial | 400 | R$ 3.990 | R$ 6,90 | 600 |
| Pro | 800 | R$ 7.990 | R$ 5,90 | 1.200 |

Os valores são configuráveis. O banco deve ser a fonte de verdade do plano ativo, não o código do frontend.

## Unidade comercial

`lead_processed` é um lead que passou pela qualificação, score, distribuição e registro de próxima ação. Cada plano pode definir limites de mensagens, follow-ups, áudio, imagem e contexto.

## Unidade técnica

| Tipo | Exemplo |
|---|---|
| `ai_classification` | Classificar intenção |
| `ai_extraction` | Extrair veículo e prazo |
| `ai_summary` | Resumir conversa |
| `ai_draft_reply` | Rascunhar resposta |
| `followup_sent` | Enviar follow-up |
| `audio_transcription` | Transcrever áudio |
| `image_analysis` | Analisar imagem |
| `long_context_processing` | Processar histórico extenso |

## Período de uso

Cada assinatura possui período de uso com início, fim, franquia, usado, excedente, custo estimado e estado. O período deve ser recalculável por uma operação auditada, mas o recálculo não pode gerar cobrança duplicada.

## Alertas

- 70%: alerta interno;
- 80%: notificação ao Owner;
- 90%: alerta de upgrade;
- 100%: registrar excedente;
- teto: pausar automações caras ou exigir autorização.

## Excedente

O excedente deve ser previsível, com teto de segurança e visualização no painel. Antes de executar operações caras próximas do limite, o sistema pode exigir aprovação ou escolher modo degradado.

## Billing provider

O domínio deve depender de uma interface:

```ts
interface BillingProvider {
  createCustomer(input: CreateCustomerInput): Promise<CustomerResult>;
  createSubscription(input: CreateSubscriptionInput): Promise<SubscriptionResult>;
  cancelSubscription(input: CancelSubscriptionInput): Promise<void>;
  createInvoice(input: CreateInvoiceInput): Promise<InvoiceResult>;
  handleWebhook(payload: unknown): Promise<BillingEvent[]>;
}
```

O MVP deve usar provider mock e preparar adapter para um provedor real adequado à operação brasileira. A aplicação não deve armazenar dados completos de cartão.

## Estados

Assinatura: `trialing`, `active`, `past_due`, `paused`, `canceled`, `incomplete`.

Invoice: `draft`, `open`, `paid`, `void`, `uncollectible`.

## Inadimplência

Não apagar dados imediatamente. Definir grace period, alertar responsáveis, restringir operações de custo variável e preservar exportação conforme contrato. O comportamento deve ser configurável.

## Implantação

A implantação é separada da mensalidade. Pode ser registrada como `one_time_setup` e cobrada manualmente ou via provider em fase futura.

## Auditoria

Registrar criação, upgrade, downgrade, cancelamento, mudança de preço, mudança de franquia, aplicação de excedente, recálculo, falha de pagamento e alteração manual.

## Fórmulas

```text
custo_total = custos_fixos + franquia × custo_variável_médio
preço_mínimo = custo_total ÷ (1 − margem − impostos_taxas)
excedente = unidades_excedentes × preço_unitário
```

O sistema não deve afirmar margem real sem os custos configurados. Deve marcar projeções como estimativas.
