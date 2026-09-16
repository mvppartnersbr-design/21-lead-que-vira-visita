# PROMPT MESTRE PARA O ANTIGRAVITY — V2
# Produto: Lead que Vira Visita

Você é uma equipe sênior de produto, engenharia full-stack, arquitetura SaaS, UX/UI, segurança, dados, automação conversacional, billing e operações comerciais. Construa um MVP funcional, seguro, multi-tenant e preparado para produção do produto **Lead que Vira Visita**.

O produto ajuda lojas de veículos seminovos a transformar leads recebidos por WhatsApp, Instagram, site, portais de anúncios e campanhas em oportunidades organizadas, visitas, test-drives e follow-ups. A plataforma deve centralizar leads, qualificar intenção, distribuir oportunidades, registrar próximas ações, agendar visitas e controlar o consumo de IA/automação por organização.

Não construa somente uma interface bonita. Construa uma operação comercial utilizável, auditável, mensurável e segura.

---

## 1. Visão do produto

O problema é:

> Lojas de seminovos recebem leads, mas muitos ficam sem resposta, são distribuídos de forma desigual, recebem informações inconsistentes, não possuem próxima ação ou consomem tempo excessivo da equipe.

O produto deve permitir:

1. centralizar leads e conversas;
2. associar cada lead a um veículo ou preferência;
3. coletar intenção, prazo, modalidade de compra e possível troca;
4. calcular score de prioridade comercial de 0 a 100;
5. distribuir leads por vendedor, equipe ou regra;
6. acompanhar o funil;
7. registrar responsável e próxima ação;
8. agendar visita, test-drive ou ligação;
9. fazer follow-up autorizado;
10. gerar resumo contextual para o vendedor;
11. controlar estoque e informação não confirmada;
12. medir uso de IA, mensagens, leads e automações;
13. aplicar franquias, excedentes, limites e alertas de consumo;
14. manter histórico, auditoria, consentimentos e opt-out.

Tese do produto:

> **A loja não precisa apenas de mais mensagens automáticas. Precisa de uma operação que saiba qual lead merece atenção agora, por quê, por quem e qual é o próximo passo.**

---

## 2. Produto e modelo comercial

O produto é um SaaS B2B verticalizado, inicialmente para lojas independentes e pequenos grupos de seminovos.

### 2.1 Planos comerciais padrão configuráveis

Crie uma tabela de planos seed, editável pelo Owner da plataforma. Não hardcode preços diretamente no frontend.

| Plano | Franquia mensal | Preço de referência | Excedente | Teto de referência |
|---|---:|---:|---:|---:|
| Essencial | 150 leads processados | R$ 2.190/mês | R$ 7,90/unidade | 225 unidades |
| Comercial | 400 leads processados | R$ 3.990/mês | R$ 6,90/unidade | 600 unidades |
| Pro | 800 leads processados | R$ 7.990/mês | R$ 5,90/unidade | 1.200 unidades |

Esses valores são defaults comerciais e precisam ser configuráveis. O sistema deve permitir alteração por organização, contrato, região, moeda e campanha.

A implantação inicial fica separada da assinatura:

- Essencial: referência de R$ 3.000 a R$ 4.500;
- Comercial: referência de R$ 5.000 a R$ 8.000;
- Pro: referência de R$ 10.000 a R$ 20.000.

Não implemente cobrança de implantação automaticamente no MVP, a menos que o ambiente forneça um gateway seguro e isso seja explicitamente ativado.

### 2.2 Unidade de consumo

A unidade comercial padrão é `lead_processed`:

> Um novo lead que passou pelo fluxo de qualificação, extração, score, distribuição e registro de próxima ação.

Uma unidade de `lead_processed` inclui, por padrão:

- até 20 mensagens recebidas no período de qualificação;
- extração estruturada de dados;
- score;
- resumo de conversa;
- até duas sugestões de resposta;
- criação ou atualização de próxima ação;
- atualização do funil.

Além disso, crie unidades técnicas separadas:

- `ai_classification`;
- `ai_extraction`;
- `ai_summary`;
- `ai_draft_reply`;
- `followup_sent`;
- `audio_transcription`;
- `image_analysis`;
- `long_context_processing`;
- `workflow_execution`.

O cliente visualiza a franquia comercial. A plataforma registra também o consumo técnico real, incluindo tokens, modelo, provedor, retries e custo estimado.

### 2.3 Regras de consumo

- O consumo deve ser atribuído a uma `organization_id`.
- Toda unidade deve ser registrada de forma idempotente.
- Uma mesma mensagem não pode gerar cobrança duplicada por retry.
- Processamento de áudio, imagem e contexto longo deve consumir unidades adicionais configuráveis.
- O sistema deve permitir definir quais operações estão incluídas em cada plano.
- O sistema deve calcular `estimated_cost` e `billable_amount`.
- O sistema deve separar custo técnico de preço comercial.
- A franquia nunca pode ser negativa.
- O cliente nunca deve receber cobrança sem evento de consumo rastreável.

### 2.4 Alertas e limites

Defaults:

- 70%: alerta interno;
- 80%: notificar Owner e Manager;
- 90%: notificação forte e sugestão de upgrade;
- 100%: marcar franquia como excedida;
- 120% ou teto configurado: exigir upgrade ou bloquear automações caras.

O sistema não deve bloquear totalmente a captação básica. Ao atingir o teto, deve poder manter:

- recebimento de lead;
- registro da conversa;
- encaminhamento básico ao vendedor;
- resposta manual.

Pode pausar:

- resumo longo;
- análise de áudio;
- análise de imagem;
- reativação em massa;
- follow-up automatizado;
- enriquecimento não essencial.

---

## 3. Escopo do MVP

### Incluído

- autenticação;
- criação de organização;
- multi-tenant;
- papéis e permissões;
- cadastro e importação de veículos;
- leads e fontes;
- conversas e inbox;
- funil;
- score;
- distribuição;
- troca;
- tarefas;
- agenda;
- templates;
- follow-up;
- WhatsApp oficial via adapter;
- provider mock;
- IA estruturada com aprovação humana;
- dashboards;
- uso e billing operacional;
- franquias e limites;
- planos configuráveis;
- invoices e registros de cobrança;
- auditoria;
- logs;
- testes;
- documentação.

### Fora do MVP

Não implemente sem decisão explícita:

- aprovação de crédito;
- decisão financeira automatizada;
- parcela definitiva;
- valor definitivo de troca;
- desconto automático;
- assinatura de contrato;
- pagamentos de veículos;
- emissão fiscal;
- scraping de portais;
- WhatsApp não oficial;
- agente de voz;
- chatbot autônomo irrestrito;
- marketplace público;
- uso de dados para crédito ou discriminação;
- billing white-label do n8n;
- planos ilimitados.

---

## 4. Usuários e autorização

Modele tudo com `organization_id`.

Papéis:

- `platform_admin`: administração do SaaS, planos, organizações e suporte;
- `owner`: configura a loja, billing, integrações, usuários e dados;
- `admin`: administra operação, sem poder excluir a organização;
- `manager`: visualiza equipe, leads, distribuição e indicadores;
- `salesperson`: acessa seus leads, tarefas, agenda e conversas permitidas;
- `analyst`: acessa dados agregados;
- `support`: acesso temporário e auditado.

O backend deve validar autorização em toda query e mutação. Nunca dependa somente de ocultar botões no frontend.

Para suporte da plataforma, implemente impersonation somente se necessário, com:

- consentimento ou política interna;
- prazo de expiração;
- motivo obrigatório;
- log de entrada e saída;
- visualização clara de que o suporte está atuando em nome de uma organização.

---

## 5. Stack técnica

Use TypeScript strict e arquitetura modular.

### Frontend

- Next.js App Router;
- TypeScript;
- Tailwind CSS;
- shadcn/ui;
- Radix UI;
- TanStack Query;
- React Hook Form;
- Zod;
- Zustand somente para estado local simples;
- Lucide React;
- Recharts;
- date-fns;
- Playwright.

### Backend

Use Next.js Route Handlers para o MVP, com domínio separado de infraestrutura. Regras de negócio não podem ficar em componentes React.

Estrutura:

```text
/apps
  /web
  /worker
/packages
  /db
  /shared
  /ui
  /integrations
  /billing
  /config
```

### Banco

- PostgreSQL;
- Prisma ou Drizzle, escolher um;
- migrations versionadas;
- UUIDs;
- UTC;
- soft delete;
- índices por organização e status;
- constraints;
- transações;
- Row Level Security se o ambiente suportar de modo confiável.

### Jobs

- Redis;
- BullMQ ou equivalente;
- retries com backoff;
- dead-letter queue;
- idempotency keys;
- workers separados para webhooks, IA, billing, follow-up e sincronização.

### Storage

S3-compatible privado, URLs assinadas com expiração e verificação de tipo/tamanho de arquivo.

### Deploy

Forneça Docker Compose para desenvolvimento. Separe ambientes de desenvolvimento, staging e produção. Não coloque segredos no repositório.

---

## 6. Integração com WhatsApp

Use somente WhatsApp Business Platform/API oficial da Meta ou BSP oficial documentado. Não use WhatsApp Web automatizado, Puppeteer ou bibliotecas que simulem usuário.

Interface:

```ts
interface MessagingProvider {
  sendText(input: SendTextInput): Promise<SendMessageResult>;
  sendTemplate(input: SendTemplateInput): Promise<SendMessageResult>;
  markAsRead(messageId: string): Promise<void>;
  verifyWebhook(input: WebhookVerificationInput): Promise<boolean>;
  parseInboundWebhook(payload: unknown): ParsedInboundEvent[];
}
```

Implementar:

1. `MockMessagingProvider`;
2. `MetaWhatsAppProvider` por variáveis de ambiente.

Registrar mensagens, templates, consentimento, opt-out, status de entrega, leitura, origem, agente, usuário, timestamp e external ID.

Validar assinatura de webhook, idempotência, replay, retry e retorno rápido `200 OK`.

---

## 7. IA e guardrails

Crie interface:

```ts
interface AIProvider {
  classifyLead(input: ClassifyLeadInput): Promise<LeadClassification>;
  extractLeadData(input: ExtractLeadDataInput): Promise<ExtractedLeadData>;
  summarizeConversation(input: SummarizeConversationInput): Promise<ConversationSummary>;
  draftReply(input: DraftReplyInput): Promise<DraftReplyResult>;
  detectHandoff(input: HandoffDetectionInput): Promise<HandoffDecision>;
}
```

Toda saída deve passar por Zod/JSON Schema.

A IA pode:

- extrair veículo;
- identificar prazo;
- identificar modalidade de compra;
- identificar troca;
- sugerir score;
- resumir conversa;
- sugerir resposta baseada em dados aprovados;
- detectar intenção de visita;
- detectar opt-out;
- apontar informação faltante;
- classificar lead.

A IA não pode:

- inventar estoque, preço, quilometragem ou versão;
- prometer aprovação de crédito;
- calcular parcela definitiva sem sistema aprovado;
- definir valor final de troca;
- negociar desconto sem autorização;
- afirmar reserva sem confirmação;
- usar dados sensíveis para priorização;
- enviar resposta se a confiança estiver abaixo do limite;
- ignorar pedido de humano, privacidade ou exclusão.

Handoff obrigatório quando houver desconto, financiamento, troca, reclamação, dúvida jurídica, baixa confiança, estoque não confirmado, solicitação de humano, visita confirmada ou assunto fora do escopo.

---

## 8. Sistema de qualificação

Score de 0 a 100.

### Interesse específico — 0 a 25

- categoria genérica: 8;
- modelo: 15;
- anúncio/versão: 20;
- perguntas concretas: 25.

### Prazo — 0 a 25

- não informa: 0;
- mais de 90 dias: 5;
- 31–90: 10;
- 8–30: 18;
- até 7 dias: 25.

### Capacidade de avançar — 0 a 25

- não sabe modalidade: 0;
- sabe se será à vista, financiamento ou troca: 8;
- entrada/faixa disponível: 12;
- informa troca: 17;
- aprovação, entrada ou documentação preparada: 25.

### Compromisso — 0 a 25

- nenhuma ação: 0;
- aceita receber informação: 5;
- aceita falar com vendedor: 10;
- escolhe horário: 18;
- confirma visita/test-drive: 25.

Faixas:

- 0–29: pesquisa;
- 30–54: interessado;
- 55–74: qualificado;
- 75–100: quente.

Confirmação de visita ou test-drive cria tarefa imediata, independentemente do score.

---

## 9. Modelo de dados

Crie entidades:

`Organization`, `User`, `Vehicle`, `Lead`, `TradeInVehicle`, `Conversation`, `Message`, `LeadEvent`, `Task`, `Appointment`, `MessageTemplate`, `AuditLog`.

Adicione entidades comerciais:

### Plan

```text
id
code
name
active
monthly_price_cents
included_leads
overage_unit_price_cents
hard_limit_leads
features_json
limits_json
created_at
updated_at
```

### Subscription

```text
id
organization_id
plan_id
status
provider
provider_customer_id
provider_subscription_id
current_period_start
current_period_end
cancel_at_period_end
trial_ends_at
created_at
updated_at
```

Status: `trialing`, `active`, `past_due`, `paused`, `canceled`, `incomplete`.

### UsagePeriod

```text
id
organization_id
period_start
period_end
included_units
used_units
overage_units
estimated_cost_cents
billable_overage_cents
status
created_at
updated_at
```

### UsageEvent

```text
id
organization_id
lead_id
conversation_id
workflow_execution_id
idempotency_key
usage_type
commercial_unit_type
units
provider
model
input_tokens
output_tokens
estimated_cost_cents
billable_amount_cents
metadata_json
created_at
```

### Invoice

```text
id
organization_id
subscription_id
period_start
period_end
base_amount_cents
overage_amount_cents
total_amount_cents
currency
status
provider_invoice_id
issued_at
due_at
paid_at
created_at
updated_at
```

### BillingAlert

```text
id
organization_id
usage_period_id
threshold
sent_at
channel
created_at
```

### IntegrationCredential

```text
id
organization_id
provider
encrypted_config
status
last_tested_at
expires_at
created_at
updated_at
```

Credenciais devem ser criptografadas em repouso e nunca retornadas em API.

---

## 10. Billing e medição

Crie `BillingProvider` abstrato:

```ts
interface BillingProvider {
  createCustomer(input: CreateCustomerInput): Promise<CustomerResult>;
  createSubscription(input: CreateSubscriptionInput): Promise<SubscriptionResult>;
  cancelSubscription(input: CancelSubscriptionInput): Promise<void>;
  createInvoice(input: CreateInvoiceInput): Promise<InvoiceResult>;
  handleWebhook(payload: unknown): Promise<BillingEvent[]>;
}
```

Implemente `MockBillingProvider` primeiro. Prepare adaptador para um provedor adequado ao mercado brasileiro, como Stripe, Asaas ou Mercado Pago, sem acoplar o domínio ao fornecedor.

Regras:

- não cobrar apenas olhando logs brutos do n8n;
- registrar consumo no momento do processamento;
- usar idempotência;
- não duplicar cobrança em retry;
- separar preço base, franquia e excedente;
- permitir teto de cobrança;
- alertar 70%, 80%, 90% e 100%;
- permitir upgrade;
- suportar downgrade apenas no ciclo seguinte;
- não excluir dados por inadimplência imediatamente;
- manter modo de captura básica se o plano entrar em limite;
- permitir exportação dos dados ao cancelar;
- registrar alterações de plano e preço em auditoria.

Fórmula interna:

```text
custo_total = custos_fixos_alocados + (franquia × custo_variável_médio)
preço_mínimo = custo_total ÷ (1 − margem − impostos_taxas)
```

Não exponha tokens ao cliente. Mostre unidades comerciais e, opcionalmente, estimativa de custo técnico agregada.

---

## 11. APIs

### Auth

```text
POST /api/auth/register
POST /api/auth/login
POST /api/auth/logout
GET  /api/auth/session
```

### Organization

```text
GET   /api/organization
PATCH /api/organization
GET   /api/organization/members
POST  /api/organization/members/invite
```

### Leads

```text
GET   /api/leads
POST  /api/leads
GET   /api/leads/:id
PATCH /api/leads/:id
POST  /api/leads/:id/assign
POST  /api/leads/:id/score
POST  /api/leads/:id/handoff
POST  /api/leads/:id/next-action
GET   /api/leads/:id/timeline
```

### Conversations

```text
GET  /api/conversations
GET  /api/conversations/:id
GET  /api/conversations/:id/messages
POST /api/conversations/:id/messages
POST /api/conversations/:id/summarize
POST /api/conversations/:id/draft-reply
POST /api/conversations/:id/takeover
```

### Vehicles

```text
GET   /api/vehicles
POST  /api/vehicles
PATCH /api/vehicles/:id
POST  /api/vehicles/import
POST  /api/vehicles/sync
```

### Appointments e tasks

```text
GET   /api/appointments
POST  /api/appointments
PATCH /api/appointments/:id
GET   /api/tasks
POST  /api/tasks
PATCH /api/tasks/:id
POST  /api/tasks/:id/complete
```

### Plans e billing

```text
GET   /api/plans
GET   /api/billing/subscription
POST  /api/billing/checkout
POST  /api/billing/upgrade
POST  /api/billing/downgrade
POST  /api/billing/cancel
GET   /api/billing/invoices
GET   /api/billing/usage/current
GET   /api/billing/usage/history
GET   /api/billing/usage/events
POST  /api/billing/usage/recalculate
POST  /api/billing/test-payment
```

Apenas Owner/Platform Admin pode alterar billing. `recalculate` deve ser protegido, idempotente e auditado.

### Templates e follow-up

```text
GET   /api/templates
POST  /api/templates
PATCH /api/templates/:id
POST  /api/follow-ups/preview
POST  /api/follow-ups/schedule
POST  /api/follow-ups/:id/cancel
```

### Dashboards

```text
GET /api/dashboard/summary
GET /api/dashboard/funnel
GET /api/dashboard/response-time
GET /api/dashboard/lead-sources
GET /api/dashboard/sales-team
GET /api/dashboard/follow-ups
GET /api/dashboard/usage
```

### Webhooks

```text
GET  /api/webhooks/whatsapp
POST /api/webhooks/whatsapp
POST /api/webhooks/billing
POST /api/webhooks/provider-events
```

---

## 12. Rotas de frontend

```text
/login
/onboarding
/dashboard
/inbox
/inbox/:conversationId
/leads
/leads/:id
/vehicles
/vehicles/:id
/appointments
/tasks
/templates
/analytics
/usage
/billing
/billing/plans
/billing/invoices
/team
/integrations
/settings
/audit-log
```

### Dashboard

Mostrar leads novos, leads sem responsável, leads quentes, visitas próximas, follow-ups vencidos, tempo de resposta, funil, fontes e consumo da franquia.

### Usage/Billing

Mostrar:

- plano atual;
- franquia;
- usado;
- excedente;
- percentual consumido;
- projeção do período;
- custo comercial estimado;
- alertas;
- histórico de unidades;
- botão de upgrade;
- explicação da unidade `lead_processed`;
- limites e ações pausadas.

Não mostrar tokens brutos como unidade principal para o cliente.

### Inbox

Desktop em três colunas:

1. lista de conversas;
2. conversa;
3. contexto do lead.

Mostrar score, breakdown, veículo, troca, responsável, próxima ação, resumo, alertas, confiança da IA e botão de assumir conversa.

---

## 13. Diretrizes de front design

A interface deve parecer um cockpit de vendas, não uma landing page futurista de IA.

Visual:

- claro, profissional e confiável;
- fundo branco/cinza claro;
- azul profundo/petróleo;
- verde para sucesso;
- âmbar para atenção;
- vermelho apenas para risco/erro;
- Inter ou equivalente;
- bordas discretas;
- sombras leves;
- densidade operacional adequada;
- mobile-first;
- desktop para gestão.

Princípios:

- toda tela responde “o que devo fazer agora?”;
- destacar próxima ação;
- não depender apenas de cor;
- estados vazio, loading e erro obrigatórios;
- preservar filtros;
- confirmar ações destrutivas;
- mostrar origem e atualização dos dados;
- mostrar quando estoque não está confirmado;
- mostrar quando plano está perto do limite;
- não esconder problemas de billing ou integração.

Componentes:

`AppShell`, `Sidebar`, `Topbar`, `LeadScoreBadge`, `UsageMeter`, `PlanBadge`, `ConversationList`, `ConversationPanel`, `LeadContextPanel`, `VehicleCard`, `DataTable`, `Timeline`, `TaskCard`, `AppointmentCalendar`, `AuditDrawer`, `AIConfidenceIndicator`, `BillingSummary`, `UpgradeDialog`.

---

## 14. Segurança, LGPD e privacidade

Implemente:

- autorização server-side;
- isolamento por tenant;
- menor privilégio;
- rate limiting;
- proteção contra brute force;
- cookies HttpOnly/Secure/SameSite;
- CSP, HSTS e headers seguros;
- validação Zod;
- sanitização XSS;
- CORS restritivo;
- proteção CSRF quando aplicável;
- uploads com MIME e tamanho verificados;
- URLs privadas assinadas;
- criptografia de credenciais;
- rotação de chaves;
- logs estruturados sem segredos;
- redaction de telefone, e-mail, CPF e documentos;
- retenção configurável;
- opt-out;
- solicitação de acesso, correção e exclusão;
- exportação de dados;
- backup e restauração testados.

Nunca envie ao provedor de IA dados desnecessários. Nunca use dados sensíveis ou proxies para score comercial. O score mede prioridade de atendimento, não crédito.

A propriedade dos dados da loja deve continuar com a loja. A aplicação deve ter política de exportação e encerramento.

---

## 15. Observabilidade

Implemente:

- Pino ou logs estruturados;
- request ID;
- correlation ID;
- métricas de webhooks;
- métricas de jobs;
- métricas de uso;
- métricas de custo estimado;
- falhas de integração;
- execução de follow-up;
- alertas de fila parada;
- alertas de custo anormal;
- health check;
- readiness check;
- rastreamento de exceções.

Nunca registre conteúdo completo de mensagens em log técnico por padrão.

---

## 16. Testes

### Unitários

Testar score, regras de plano, cálculo de uso, limites, excedente, autorização, opt-out, transição de estágio, idempotência, handoff e bloqueio de afirmações não confirmadas.

### Integração

Testar webhook WhatsApp, billing webhook, criação de lead, distribuição, score, agendamento, uso, alerta de franquia, upgrade, cancelamento, isolamento multi-tenant e provider mock.

### End-to-end

Cobrir:

1. criação de organização;
2. convite de usuário;
3. importação de veículo;
4. entrada de lead;
5. score;
6. distribuição;
7. conversa;
8. visita;
9. follow-up;
10. opt-out;
11. franquia em 80%, 100% e teto;
12. upgrade;
13. acesso proibido entre tenants;
14. cancelamento e exportação.

---

## 17. Critérios de aceite

O MVP só está pronto quando:

- uma organização pode ser criada;
- usuários possuem papéis corretos;
- tenants estão isolados;
- veículos e leads podem ser cadastrados;
- score possui breakdown;
- leads podem ser distribuídos;
- inbox possui histórico;
- visita/test-drive pode ser agendado;
- follow-up possui aprovação;
- IA retorna schema válido;
- IA não inventa dados;
- WhatsApp mock funciona;
- webhook é validado e idempotente;
- plano atual aparece;
- consumo é registrado por evento;
- franquia e excedente são calculados corretamente;
- alertas são enviados nos thresholds;
- teto limita automações caras sem bloquear captação básica;
- invoice pode ser simulada no ambiente demo;
- billing é auditado;
- nenhuma chave aparece no frontend;
- logs não vazam dados sensíveis;
- testes passam;
- documentação permite rodar localmente sem credenciais reais.

---

## 18. Dados demo

Criar organização fictícia `AutoPrime Seminovos` com:

- 8 veículos disponíveis;
- 2 reservados;
- 1 vendido;
- 12 leads em diferentes estágios;
- 4 vendedores;
- conversas;
- visitas;
- tarefas vencidas;
- exemplos de curioso, interessado, qualificado e quente;
- opt-out;
- estoque não confirmado;
- handoff humano;
- plano Comercial;
- consumo em 82% da franquia;
- evento de excedente;
- invoice fictícia não financeira.

Tudo deve ser claramente fictício.

---

## 19. Ordem de implementação

### Fase 1 — Fundação

Monorepo, TypeScript, lint, banco, migrations, autenticação, organização, papéis e isolamento.

### Fase 2 — Domínio comercial

Veículos, leads, funil, score, tarefas, agenda, eventos e conversas.

### Fase 3 — Interface operacional

Dashboard, inbox, lead detail, veículos, agenda, equipe e templates.

### Fase 4 — IA controlada

Provider mock, extração, resumo, draft, score e handoff.

### Fase 5 — Medição e billing

Planos, assinatura, usage events, períodos, alertas, excedentes, teto, invoices e mock billing provider.

### Fase 6 — WhatsApp oficial

Webhook, adapter Meta, templates, status, opt-out e retry.

### Fase 7 — Qualidade

Auditoria, segurança, observabilidade, testes, estados de erro e documentação.

Não avance sem cumprir os critérios da fase anterior.

---

## 20. Instruções finais para o Antigravity

Antes de codificar:

1. inspecione o repositório e o ambiente;
2. identifique o que já existe;
3. apresente arquitetura e árvore de arquivos;
4. liste variáveis de ambiente;
5. liste integrações que exigem credenciais;
6. não invente APIs disponíveis;
7. proponha plano por vertical slices;
8. comece pelo domínio e segurança;
9. use providers mock para desenvolvimento;
10. execute testes após cada etapa.

Durante o desenvolvimento:

- não use WhatsApp não oficial;
- não exponha segredos;
- não deixe autorização só no frontend;
- não implemente billing sem idempotência;
- não conte consumo olhando apenas logs brutos;
- não cobre evento duplicado;
- não use plano ilimitado;
- não permita que um cliente veja uso ou dados de outro;
- não misture custo técnico com preço comercial;
- não faça IA tomar decisões financeiras;
- não aceite estoque desatualizado como verdade;
- não esconda erros;
- não crie tela sem loading, empty e error state;
- não use dados reais no demo;
- não considere pronto só porque a interface renderiza.

Ao final, entregue:

1. árvore de arquivos;
2. instruções de instalação;
3. `.env.example`;
4. instruções de migração;
5. seeds demo;
6. credenciais locais sem segredos reais;
7. OpenAPI;
8. decisões arquiteturais;
9. riscos conhecidos;
10. testes executados;
11. critérios atendidos e pendentes;
12. próximos passos de produção.

Construa uma plataforma cuja função central seja clara: ajudar a loja a saber **qual lead merece atenção agora, por quê, por quem, qual é o próximo passo e quanto consumo operacional ainda está disponível no plano**.
