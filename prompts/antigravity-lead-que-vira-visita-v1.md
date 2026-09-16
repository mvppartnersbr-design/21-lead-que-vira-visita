# PROMPT MESTRE PARA O ANTIGRAVITY
# Produto: Lead que Vira Visita

Você é uma equipe sênior de produto, engenharia, UX/UI, segurança, dados e automação conversacional. Construa um MVP funcional, seguro, multi-tenant e preparado para evolução do produto **Lead que Vira Visita**.

O produto é uma plataforma para lojas de veículos seminovos que recebem leads por WhatsApp, Instagram, site, portais de anúncios e campanhas. A plataforma deve ajudar a loja a transformar conversas em oportunidades organizadas, visitas, test-drives e follow-ups, sem permitir que a IA invente estoque, preço, financiamento, aprovação de crédito ou condições comerciais.

Não construa apenas uma interface bonita. Construa uma operação de vendas utilizável, auditável e segura.

---

## 1. Objetivo do produto

O produto deve resolver este problema:

> Lojas de seminovos recebem leads, mas muitos contatos ficam sem resposta, são distribuídos de forma desigual, recebem informações inconsistentes ou não têm uma próxima ação definida.

O produto deve permitir que a loja:

1. centralize e registre leads;
2. associe cada lead a um veículo ou preferência de compra;
3. colete dados de intenção, prazo, modalidade de pagamento e eventual veículo de troca;
4. calcule um score de prioridade comercial;
5. distribua o lead para o vendedor correto;
6. acompanhe o estágio do funil;
7. registre a próxima ação;
8. agende visita ou test-drive;
9. faça follow-up de forma autorizada e controlada;
10. entregue ao vendedor um resumo contextualizado;
11. acompanhe indicadores operacionais;
12. mantenha histórico, auditoria e consentimentos.

A tese do produto é:

> **A loja não precisa apenas de mais mensagens automáticas. Precisa de uma operação que saiba quem é o lead, em que etapa ele está, quem é o responsável e qual é o próximo passo.**

---

## 2. Escopo do MVP

### 2.1 Incluído no MVP

Implemente:

- autenticação e criação de organização;
- suporte a múltiplas organizações isoladas;
- usuários e papéis;
- cadastro e importação de veículos;
- cadastro de leads;
- caixa de entrada interna de conversas;
- cadastro de fontes de lead;
- funil de vendas;
- score de qualificação de 0 a 100;
- regras configuráveis de pontuação;
- distribuição manual e automática de leads;
- cadastro de veículo de troca;
- registro de tarefas e próxima ação;
- agenda de visitas e test-drives;
- templates de mensagens;
- follow-up manual e agendado;
- integração preparada para WhatsApp Cloud API oficial da Meta;
- webhook de mensagens recebidas;
- adaptador mock para desenvolvimento sem credenciais reais;
- agente de IA com saída estruturada e aprovação humana;
- resumo automático da conversa;
- classificação de intenção;
- detecção de necessidade de transferência para humano;
- dashboard operacional;
- trilha de auditoria;
- logs e tratamento de erros;
- documentação de configuração;
- testes unitários, de integração e end-to-end.

### 2.2 Fora do MVP

Não implemente agora:

- aprovação de crédito;
- decisão financeira automatizada;
- cálculo definitivo de financiamento;
- avaliação definitiva de veículo de troca;
- negociação automática de descontos;
- assinatura digital de contratos;
- pagamentos;
- emissão de nota fiscal;
- consulta jurídica ou documental;
- publicação automática em todos os portais;
- agente de voz;
- chatbot totalmente autônomo sem aprovação humana;
- marketplace público de veículos;
- cobrança ou planos de assinatura;
- scraping de portais;
- bibliotecas não oficiais para WhatsApp;
- envio em massa sem consentimento e sem controles de opt-out.

Se alguma dessas funcionalidades aparecer em uma tela, marque como “fase futura” e não a implemente no MVP.

---

## 3. Usuários e permissões

Modele a aplicação como multi-tenant. Toda informação de negócio deve pertencer a uma `organization_id` e jamais pode vazar entre organizações.

### 3.1 Papéis

#### Owner

Pode configurar a organização, convidar usuários, alterar regras, gerenciar integrações, visualizar todos os leads, exportar dados e consultar auditoria.

#### Admin

Pode gerenciar operação, usuários, fontes, veículos, funil, templates e dashboards, mas não deve alterar dados críticos de faturamento ou excluir a organização.

#### Manager

Pode visualizar todos os leads da equipe, redistribuir oportunidades, analisar desempenho, revisar conversas e alterar tarefas.

#### Salesperson

Visualiza os leads atribuídos a si, suas tarefas, seus agendamentos e conversas permitidas. Não pode consultar leads privados de outros vendedores sem permissão.

#### Analyst

Pode visualizar dashboards e dados agregados, mas não deve editar conversas nem dados pessoais desnecessários.

#### Support

Pode ajudar em operação e suporte, com acesso controlado, temporário e auditado.

Implemente autorização no backend. Não confie apenas na ocultação de botões no frontend.

---

## 4. Stack técnica obrigatória/recomendada

Use uma arquitetura TypeScript moderna, tipada e modular.

### 4.1 Monorepo

Use `pnpm` com a seguinte estrutura:

```text
/apps
  /web
  /worker
/packages
  /db
  /shared
  /config
  /ui
  /integrations
```

### 4.2 Frontend

- Next.js com App Router;
- TypeScript em modo strict;
- Tailwind CSS;
- shadcn/ui;
- Radix UI quando necessário;
- TanStack Query para dados remotos e cache;
- React Hook Form;
- Zod para validação de formulários;
- Zustand somente para estado local simples;
- Lucide React para ícones;
- Recharts para gráficos;
- date-fns para datas;
- Playwright para testes end-to-end.

### 4.3 Backend

Use Next.js Route Handlers para o MVP, com separação clara entre domínio, aplicação e infraestrutura. Não coloque regras de negócio complexas diretamente nos componentes React.

Estruture assim:

```text
/apps/web/src
  /app
  /components
  /features
  /lib
  /server
    /domain
    /application
    /infrastructure
    /auth
    /api
  /styles
```

Se a integração de webhooks e jobs exigir processo separado, use o `/apps/worker` em Node.js com TypeScript.

### 4.4 Banco de dados

- PostgreSQL;
- Prisma ORM ou Drizzle ORM; escolha um e use-o consistentemente;
- migrações versionadas;
- UUIDs como identificadores públicos;
- timestamps em UTC;
- soft delete para entidades de negócio;
- índices para `organization_id`, `status`, `assigned_to`, `created_at`, `next_action_at` e `source`;
- constraints para evitar inconsistências;
- transações para mudanças de estágio, distribuição e agendamento.

Preferência: usar PostgreSQL gerenciado quando o ambiente fornecer essa possibilidade. Caso contrário, fornecer Docker Compose para desenvolvimento local.

### 4.5 Filas e tarefas assíncronas

Use Redis com BullMQ, ou equivalente disponível no ambiente, para:

- processamento de webhooks;
- resumo de conversas;
- cálculo de score assíncrono;
- envio de follow-ups agendados;
- sincronização de veículos;
- notificações;
- retries com backoff;
- tarefas idempotentes.

Nenhum webhook deve depender de processamento longo antes de retornar `200 OK`.

### 4.6 Armazenamento

Use storage S3-compatible para fotos de veículos, anexos e documentos não sensíveis. Os arquivos devem ser privados por padrão e acessados por URLs assinadas com expiração.

### 4.7 Autenticação

Use uma solução madura de autenticação compatível com o ambiente, preferencialmente Auth.js/Better Auth ou o provedor nativo do ambiente. Implemente:

- login por e-mail e senha ou magic link;
- recuperação de senha;
- sessões seguras;
- rotação de sessão;
- MFA preparado para fase posterior;
- convite de membros;
- encerramento de sessões;
- proteção contra brute force;
- rate limiting;
- verificação de e-mail quando aplicável.

---

## 5. Integração com WhatsApp

Use somente a **WhatsApp Business Platform/API oficial da Meta** ou um provedor oficial documentado. Não use WhatsApp Web automatizado, Puppeteer, Playwright ou bibliotecas que simulem usuário para envio de mensagens.

Crie uma interface de integração:

```ts
interface MessagingProvider {
  sendText(input: SendTextInput): Promise<SendMessageResult>;
  sendTemplate(input: SendTemplateInput): Promise<SendMessageResult>;
  markAsRead(messageId: string): Promise<void>;
  getConversationStatus(conversationId: string): Promise<ConversationStatus>;
  verifyWebhook(input: WebhookVerificationInput): Promise<boolean>;
  parseInboundWebhook(payload: unknown): ParsedInboundEvent[];
}
```

Implemente dois adaptadores:

1. `MockMessagingProvider`, para desenvolvimento e testes;
2. `MetaWhatsAppProvider`, com credenciais via variáveis de ambiente.

Nunca coloque token da Meta, chave de IA ou segredo de webhook no frontend.

### 5.1 Regras de mensagem

O sistema deve registrar:

- mensagem recebida;
- mensagem enviada;
- remetente;
- destinatário;
- canal;
- template utilizado;
- status de entrega;
- status de leitura quando disponível;
- consentimento aplicável;
- opt-out;
- usuário ou agente responsável pela geração;
- timestamp;
- mensagem original e versão enviada, quando permitido.

O sistema deve impedir disparos promocionais para contatos que solicitaram não receber mensagens.

---

## 6. Integração com IA

Crie um `AIProvider` abstrato para permitir troca de modelo:

```ts
interface AIProvider {
  classifyLead(input: ClassifyLeadInput): Promise<LeadClassification>;
  extractLeadData(input: ExtractLeadDataInput): Promise<ExtractedLeadData>;
  summarizeConversation(input: SummarizeConversationInput): Promise<ConversationSummary>;
  draftReply(input: DraftReplyInput): Promise<DraftReplyResult>;
  detectHandoff(input: HandoffDetectionInput): Promise<HandoffDecision>;
}
```

Use saída estruturada validada por Zod ou JSON Schema. Se a saída não passar na validação, não envie a mensagem e encaminhe para revisão humana.

### 6.1 A IA pode

- identificar o veículo de interesse mencionado;
- extrair intenção, prazo, modalidade de compra e existência de troca;
- sugerir perguntas de qualificação;
- calcular ou sugerir componentes do score;
- resumir a conversa;
- sugerir resposta baseada em dados aprovados;
- reconhecer intenção de visitar ou fazer test-drive;
- detectar dúvidas que exigem vendedor;
- identificar possível opt-out;
- apontar informação faltante;
- classificar o lead em pesquisa, interessado, qualificado ou quente.

### 6.2 A IA não pode

- inventar estoque;
- confirmar disponibilidade sem consultar fonte atualizada;
- inventar preço, quilometragem, versão ou condição;
- prometer aprovação de crédito;
- calcular parcela definitiva sem sistema aprovado;
- definir valor final de troca;
- negociar desconto sem autorização;
- apresentar hipótese como fato;
- afirmar que o carro está reservado sem confirmação;
- ocultar que uma informação precisa ser confirmada por humano;
- enviar resposta quando a confiança estiver abaixo do limite configurado;
- tomar decisão discriminatória baseada em dados sensíveis ou proxies.

### 6.3 Regras de handoff

Encaminhe imediatamente a um humano quando:

- o lead pergunta sobre desconto;
- o lead pede financiamento ou aprovação;
- o lead quer avaliar troca;
- o estoque não estiver sincronizado;
- houver reclamação;
- houver dúvida jurídica ou documental;
- o lead estiver irritado;
- a mensagem tiver baixa confiança de interpretação;
- o lead confirmar visita/test-drive;
- o lead pedir explicitamente uma pessoa;
- houver pedido de exclusão ou privacidade;
- o conteúdo fugir do escopo.

---

## 7. Modelo de dados

Crie pelo menos as seguintes entidades.

### Organization

```text
id
name
slug
status
timezone
created_at
updated_at
```

### User

```text
id
organization_id
name
email
phone
role
status
last_login_at
created_at
updated_at
```

### Vehicle

```text
id
organization_id
stock_code
make
model
version
year_manufacture
year_model
mileage
color
fuel
transmission
price_cents
status
source
public_description
photos
last_synced_at
metadata_json
created_at
updated_at
```

Status possíveis: `available`, `reserved`, `sold`, `inactive`, `unknown`.

### Lead

```text
id
organization_id
name
phone
email
source
source_campaign
source_external_id
status
score
score_breakdown_json
intent_level
purchase_timeline
payment_method
has_trade_in
assigned_to
vehicle_id
last_contact_at
next_action_at
next_action_type
consent_status
opt_out_at
created_at
updated_at
```

Status possíveis: `new`, `researching`, `interested`, `qualified`, `hot`, `scheduled`, `visited`, `proposal`, `won`, `lost`, `nurture`, `opted_out`.

### TradeInVehicle

```text
id
lead_id
make
model
version
year
mileage
condition_notes
photos
estimated_value_cents
estimated_by
status
created_at
updated_at
```

Nunca trate `estimated_value_cents` como valor final. O campo deve mostrar se é apenas uma estimativa inicial ou uma avaliação confirmada.

### Conversation

```text
id
organization_id
lead_id
channel
status
assigned_to
last_message_at
ai_enabled
human_handoff_reason
created_at
updated_at
```

### Message

```text
id
conversation_id
external_message_id
direction
sender_type
content
content_type
ai_generated
ai_confidence
provider_status
sent_at
read_at
created_at
```

### LeadEvent

```text
id
lead_id
type
actor_type
actor_id
payload_json
created_at
```

Use eventos para manter histórico de score, mudança de estágio, atribuição, agendamento, envio de template e handoff.

### Task

```text
id
organization_id
lead_id
assigned_to
type
title
due_at
status
completed_at
created_at
updated_at
```

### Appointment

```text
id
organization_id
lead_id
vehicle_id
type
scheduled_at
status
location
notes
created_by
created_at
updated_at
```

Tipos: `visit`, `test_drive`, `call`, `video_call`.

### MessageTemplate

```text
id
organization_id
name
channel
category
content
variables_json
approval_status
active
created_by
created_at
updated_at
```

### AuditLog

```text
id
organization_id
actor_id
actor_type
action
entity_type
entity_id
before_json
after_json
ip_hash
user_agent_hash
created_at
```

---

## 8. Sistema de qualificação

Implemente score de 0 a 100.

### Interesse específico — 0 a 25

- categoria genérica: 8;
- modelo específico: 15;
- anúncio/versão específica: 20;
- perguntas concretas sobre o veículo: 25.

### Prazo de compra — 0 a 25

- não informa ou apenas pesquisa: 0;
- mais de 90 dias: 5;
- 31 a 90 dias: 10;
- 8 a 30 dias: 18;
- até 7 dias: 25.

### Capacidade de avançar — 0 a 25

- não sabe modalidade: 0;
- sabe se será à vista, financiamento ou troca: 8;
- possui faixa de entrada ou valor disponível: 12;
- informa veículo de troca: 17;
- possui entrada definida, aprovação ou documentação preparada: 25.

### Compromisso com o próximo passo — 0 a 25

- não aceita ação: 0;
- aceita receber informações: 5;
- aceita falar com vendedor em horário definido: 10;
- escolhe horário de visita: 18;
- confirma visita/test-drive: 25.

### Faixas

- `0–29`: pesquisa inicial;
- `30–54`: interessado;
- `55–74`: oportunidade qualificada;
- `75–100`: lead quente.

Regra de prioridade: confirmação de visita ou test-drive sempre cria tarefa imediata para o vendedor, mesmo que o score esteja incompleto.

O score é de prioridade comercial. Não é score de crédito, não decide aprovação e não deve usar atributos sensíveis.

---

## 9. API REST

Crie documentação OpenAPI para as rotas.

### Auth

```text
POST   /api/auth/register
POST   /api/auth/login
POST   /api/auth/logout
POST   /api/auth/forgot-password
POST   /api/auth/reset-password
GET    /api/auth/session
```

### Organization

```text
GET    /api/organization
PATCH  /api/organization
GET    /api/organization/members
POST   /api/organization/members/invite
PATCH  /api/organization/members/:id
DELETE /api/organization/members/:id
```

### Leads

```text
GET    /api/leads
POST   /api/leads
GET    /api/leads/:id
PATCH  /api/leads/:id
DELETE /api/leads/:id
POST   /api/leads/:id/assign
POST   /api/leads/:id/score
POST   /api/leads/:id/qualify
POST   /api/leads/:id/handoff
POST   /api/leads/:id/next-action
GET    /api/leads/:id/timeline
```

### Conversations

```text
GET    /api/conversations
GET    /api/conversations/:id
GET    /api/conversations/:id/messages
POST   /api/conversations/:id/messages
POST   /api/conversations/:id/summarize
POST   /api/conversations/:id/draft-reply
POST   /api/conversations/:id/takeover
POST   /api/conversations/:id/close
```

### Vehicles

```text
GET    /api/vehicles
POST   /api/vehicles
GET    /api/vehicles/:id
PATCH  /api/vehicles/:id
POST   /api/vehicles/import
POST   /api/vehicles/sync
POST   /api/vehicles/:id/reserve
POST   /api/vehicles/:id/mark-sold
```

### Trade-in

```text
POST   /api/leads/:id/trade-in
GET    /api/leads/:id/trade-in
PATCH  /api/leads/:id/trade-in
POST   /api/leads/:id/trade-in/request-evaluation
```

### Appointments

```text
GET    /api/appointments
POST   /api/appointments
GET    /api/appointments/:id
PATCH  /api/appointments/:id
POST   /api/appointments/:id/confirm
POST   /api/appointments/:id/cancel
```

### Tasks e follow-up

```text
GET    /api/tasks
POST   /api/tasks
PATCH  /api/tasks/:id
POST   /api/tasks/:id/complete
POST   /api/follow-ups/preview
POST   /api/follow-ups/schedule
POST   /api/follow-ups/:id/cancel
```

### Templates

```text
GET    /api/templates
POST   /api/templates
PATCH  /api/templates/:id
POST   /api/templates/:id/submit-approval
```

### Dashboard

```text
GET    /api/dashboard/summary
GET    /api/dashboard/funnel
GET    /api/dashboard/response-time
GET    /api/dashboard/lead-sources
GET    /api/dashboard/sales-team
GET    /api/dashboard/follow-ups
```

### Webhooks

```text
GET    /api/webhooks/whatsapp
POST   /api/webhooks/whatsapp
POST   /api/webhooks/provider-events
```

Webhook requirements:

- validar assinatura;
- validar token de verificação;
- responder rapidamente;
- usar idempotência por `external_message_id`;
- enfileirar processamento;
- registrar erro sem vazar payload sensível;
- impedir replay quando houver timestamp/assinatura;
- nunca confiar em campos não validados.

---

## 10. Rotas de frontend

Crie as seguintes rotas protegidas:

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
/team
/integrations
/settings
/audit-log
```

### Dashboard

Mostrar:

- leads novos hoje;
- leads sem responsável;
- leads quentes;
- visitas e test-drives próximos;
- follow-ups vencidos;
- tempo médio de primeira resposta;
- distribuição por fonte;
- funil por estágio;
- alertas de estoque desatualizado;
- alertas de falha em integração.

Não inventar metas ou resultados. Se não houver dados suficientes, mostrar estado vazio com instrução.

### Inbox

Criar layout de três colunas em desktop:

1. lista de conversas;
2. conversa atual;
3. painel de contexto do lead.

O painel de contexto deve mostrar:

- nome e telefone;
- veículo de interesse;
- score;
- breakdown do score;
- prazo de compra;
- modalidade de compra;
- troca;
- responsável;
- próxima ação;
- tarefas;
- agendamentos;
- resumo gerado pela IA;
- alertas de informação não confirmada;
- botão de assumir conversa;
- botão de encaminhar;
- botão de agendar.

### Lead detail

Mostrar timeline completa, dados de contato, veículo, troca, score, tarefas, conversas, agendamentos, eventos e auditoria permitida.

### Veículos

Criar tabela e visualização em cards, com busca e filtros por:

- marca;
- modelo;
- preço;
- ano;
- quilometragem;
- câmbio;
- combustível;
- status;
- data de atualização.

A interface deve mostrar claramente quando o dado está desatualizado ou não confirmado.

---

## 11. Diretrizes de UX e front design

A interface deve parecer um **cockpit operacional de vendas**, não uma landing page de IA.

### Direção visual

- base clara e profissional;
- fundo branco ou cinza muito claro;
- azul petróleo ou azul profundo para confiança;
- verde discreto para estados positivos;
- âmbar para atenção;
- vermelho apenas para erro, risco ou urgência;
- alto contraste;
- tipografia Inter ou equivalente sans-serif;
- bordas discretas;
- sombras suaves;
- raio moderado, sem excesso de cards flutuantes;
- densidade de informação adequada para uso diário;
- mobile-first para operação no celular;
- desktop otimizado para gestores e vendedores.

### Princípios de interação

- toda tela deve responder: “o que devo fazer agora?”;
- ações importantes devem ser visíveis sem procurar menus;
- destaque pendências e próximos passos;
- não usar cor como único indicador;
- sempre mostrar estado vazio útil;
- confirmar ações destrutivas;
- usar toasts para ações rápidas;
- usar skeleton loading;
- tratar erros com mensagem acionável;
- evitar modais longos;
- preservar filtros e contexto ao voltar para uma lista;
- permitir atalhos de teclado no desktop quando útil.

### Componentes essenciais

- AppShell;
- Sidebar;
- Topbar;
- CommandBar;
- LeadScoreBadge;
- FunnelStageBadge;
- ConversationList;
- ConversationPanel;
- LeadContextPanel;
- VehicleCard;
- VehicleTable;
- TaskCard;
- AppointmentCalendar;
- Timeline;
- DataTable;
- FilterBar;
- EmptyState;
- ErrorState;
- ConfirmDialog;
- PermissionGate;
- AuditDrawer;
- AIConfidenceIndicator.

---

## 12. Segurança e privacidade

Trate a aplicação como sistema que processa dados pessoais e conversas comerciais.

### 12.1 Controle de acesso

- autorização server-side;
- isolamento por organização;
- checagem de papel e escopo em todas as mutações;
- princípio do menor privilégio;
- suporte a escopo por equipe e vendedor;
- logs de acesso a dados sensíveis;
- sessões revogáveis;
- rate limiting por IP, usuário e organização.

### 12.2 Segredos

- usar `.env.example` sem valores reais;
- nunca commitar segredos;
- validar variáveis no startup;
- separar ambientes;
- usar secret manager no deploy;
- mascarar tokens nos logs;
- nunca enviar segredo para o navegador;
- rotacionar credenciais quando necessário.

### 12.3 Proteção de dados

- coletar apenas dados necessários;
- registrar base/consentimento quando aplicável;
- permitir opt-out;
- permitir solicitação de acesso, correção e exclusão conforme política da organização;
- aplicar retenção configurável;
- anonimizar dados usados em testes;
- não usar conversas reais em desenvolvimento sem autorização;
- não enviar dados pessoais desnecessários ao provedor de IA;
- criar política de redaction de telefone, e-mail, CPF e documentos em logs.

### 12.4 Segurança de aplicação

- validação Zod em entrada e saída;
- proteção contra SQL injection via ORM;
- proteção XSS;
- sanitização de conteúdo renderizado;
- CSRF quando aplicável;
- CORS restritivo;
- CSP;
- HSTS;
- cookies `HttpOnly`, `Secure`, `SameSite`;
- upload com limite de tamanho e tipo;
- verificação de MIME real;
- proteção contra SSRF;
- proteção contra prompt injection;
- dependências auditadas;
- headers de segurança;
- backup e plano de restauração.

### 12.5 Segurança específica da IA

Nunca trate texto recebido do lead como instrução confiável para alterar regras do sistema. O conteúdo do usuário é dado, não comando de sistema.

A IA deve trabalhar com:

- prompt de sistema imutável no backend;
- dados de estoque selecionados pelo sistema;
- ferramentas permitidas por allowlist;
- schema de resposta;
- limite de confiança;
- bloqueio de ferramentas sensíveis;
- handoff humano para decisões comerciais.

---

## 13. Fluxos principais

### Fluxo A — Lead recebido

1. webhook ou entrada manual recebe a mensagem;
2. sistema identifica ou cria contato;
3. sistema identifica ou cria conversa;
4. mensagem é armazenada de forma idempotente;
5. fila processa classificação;
6. IA extrai dados com schema;
7. sistema atualiza campos sem sobrescrever dado confirmado sem revisão;
8. score é calculado;
9. regra de distribuição é executada;
10. tarefa é criada;
11. vendedor recebe notificação;
12. agente sugere resposta ou envia apenas se autorizado pelo modo da organização.

### Fluxo B — Lead qualificado

1. score ultrapassa limiar;
2. sistema cria evento de qualificação;
3. lead é atribuído;
4. resumo é gerado;
5. tarefa de contato é criada;
6. vendedor recebe contexto;
7. lead é convidado a escolher horário;
8. agendamento é criado;
9. lembrete é registrado;
10. status passa para `scheduled`.

### Fluxo C — Agendamento confirmado

1. sistema valida data e horário;
2. cria appointment;
3. atualiza lead;
4. cria tarefa de confirmação;
5. envia mensagem autorizada;
6. registra status de envio;
7. alerta vendedor se houver falha;
8. registra tudo na timeline.

### Fluxo D — Follow-up

1. tarefa entra em estado vencido ou próximo do vencimento;
2. sistema verifica opt-out e janela permitida;
3. verifica se houve resposta recente;
4. gera preview;
5. exige aprovação humana no modo conservador;
6. envia template oficial quando aplicável;
7. registra envio;
8. cria próxima tarefa ou encerra o fluxo.

---

## 14. Integrações futuras preparadas

Crie adapters, sem implementar todos no MVP:

```text
InventoryProvider
CRMProvider
CalendarProvider
FinancingProvider
TradeInProvider
MessagingProvider
AIProvider
AnalyticsProvider
```

A plataforma deve funcionar com dados internos/mockados antes da integração real. Nunca acople o domínio diretamente a uma API de fornecedor.

---

## 15. Observabilidade

Implemente:

- logs estruturados com Pino ou equivalente;
- correlation ID por request;
- request ID por webhook;
- métricas de tempo de resposta;
- métricas de falha de integração;
- métricas de jobs;
- rastreamento de exceções;
- health check;
- readiness check;
- painel de erros no ambiente de desenvolvimento;
- alertas para webhook quebrado, fila parada e token expirado.

Não registre conteúdo completo de mensagens em logs técnicos por padrão.

---

## 16. Testes

### Unitários

Teste:

- cálculo de score;
- regras de prioridade;
- transição de estágio;
- permissão por papel;
- validações Zod;
- detecção de opt-out;
- idempotência;
- handoff;
- bloqueio de afirmações sem fonte confirmada.

### Integração

Teste:

- webhook recebido;
- persistência de mensagem;
- criação de lead;
- distribuição;
- criação de tarefa;
- agendamento;
- integração com provider mock;
- processamento da fila;
- isolamento de organizações.

### End-to-end

Cubra:

1. organização criada;
2. usuário convidado;
3. veículo importado;
4. lead recebido;
5. score calculado;
6. lead atribuído;
7. conversa assumida;
8. visita agendada;
9. follow-up aprovado;
10. opt-out bloqueando comunicação;
11. usuário sem permissão sendo impedido;
12. uma organização não acessando dados de outra.

---

## 17. Critérios de aceite do MVP

O MVP só deve ser considerado pronto quando:

- for possível criar uma organização e um usuário proprietário;
- for possível convidar vendedor e gerente;
- o isolamento multi-tenant estiver testado;
- for possível cadastrar e importar veículos;
- for possível criar um lead manualmente;
- o sistema calcular score com breakdown visível;
- o lead puder ser atribuído a um vendedor;
- existir inbox com histórico de mensagens;
- o vendedor puder assumir uma conversa;
- existir resumo de IA validado;
- a IA não puder inventar dados do veículo;
- for possível criar tarefa e próxima ação;
- for possível agendar visita/test-drive;
- existir follow-up com preview e aprovação;
- opt-out impedir novos envios promocionais;
- webhook tiver validação e idempotência;
- logs não vazarem tokens ou dados desnecessários;
- existir trilha de auditoria;
- dashboard mostrar dados reais ou estado vazio honesto;
- existir documentação local para rodar o projeto;
- testes principais passarem;
- o projeto não depender de credenciais reais para rodar em modo demo.

---

## 18. Ordem de implementação

Implemente em etapas pequenas e verificáveis.

### Fase 1 — Fundação

Configure monorepo, lint, format, TypeScript strict, banco, migrações, autenticação, organização e papéis.

### Fase 2 — Domínio comercial

Implemente veículos, leads, fontes, status, score, eventos, tarefas e appointments.

### Fase 3 — Interface operacional

Implemente dashboard, inbox, detalhe do lead, veículos, tarefas, agenda e equipe.

### Fase 4 — IA controlada

Implemente provider mock, extração estruturada, resumo, score assistido, draft de resposta e handoff.

### Fase 5 — WhatsApp oficial

Implemente webhook, adapter da Meta, templates, opt-out, status de envio e retry.

### Fase 6 — Qualidade

Implemente auditoria, observabilidade, testes, estados de erro, segurança e documentação.

Não avance para a próxima fase se os critérios de aceite da fase atual não estiverem funcionando.

---

## 19. Dados demo obrigatórios

Crie modo demo com uma organização fictícia chamada `AutoPrime Seminovos`, com:

- 8 veículos disponíveis;
- 2 veículos reservados;
- 1 veículo vendido;
- 12 leads em diferentes etapas;
- 4 vendedores;
- 3 conversas com mensagens;
- 2 visitas agendadas;
- 3 tarefas vencidas;
- 2 follow-ups pendentes;
- exemplos de lead curioso, interessado, qualificado e quente;
- exemplo de opt-out;
- exemplo de estoque não confirmado;
- exemplo de handoff humano.

Os dados demo devem ser claramente fictícios e não podem parecer dados reais de pessoas.

---

## 20. Instruções finais para o Antigravity

Antes de codificar:

1. inspecione o ambiente e o repositório;
2. identifique o que já existe;
3. não substitua arquivos sem necessidade;
4. apresente uma árvore de arquitetura;
5. apresente o plano de implementação;
6. identifique dependências e variáveis de ambiente;
7. sinalize decisões que exigem credencial externa;
8. comece pelo domínio e pela segurança;
9. entregue vertical slices funcionais;
10. execute testes depois de cada etapa.

Durante o desenvolvimento:

- não invente integrações disponíveis;
- não use credenciais de exemplo como se fossem reais;
- não implemente WhatsApp não oficial;
- não faça a IA tomar decisões financeiras;
- não esconda erros;
- não crie telas sem estados de loading, erro e vazio;
- não acople regras de negócio a componentes visuais;
- não deixe regras de autorização apenas no frontend;
- não use dados reais no modo demo;
- não considere o trabalho concluído apenas porque a tela renderiza.

Ao final, entregue:

1. árvore de arquivos;
2. instruções de instalação;
3. instruções de configuração;
4. `.env.example`;
5. instruções de migração;
6. credenciais demo locais, sem segredos reais;
7. documentação de APIs;
8. decisões arquiteturais;
9. riscos conhecidos;
10. testes executados;
11. critérios de aceite atendidos e pendentes;
12. próximos passos de produção.

Construa o produto com foco em confiabilidade operacional. A prioridade não é parecer uma “IA futurista”; é permitir que uma loja saiba **qual lead deve receber atenção agora, por quê, por quem e qual é o próximo passo**.
