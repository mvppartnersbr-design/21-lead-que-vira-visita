# Plano de Implementação — Lead que Vira Visita

**Versão:** 1.0  
**Estado atual:** documentação e especificação  
**Próximo estado desejado:** MVP funcional para piloto assistido de 14 dias  
**Repositório de referência:** `mvppartnersbr-design/lead-que-vira-visita`

> Este documento transforma a especificação existente em um plano executável. Ele separa o que já foi definido do que ainda precisa ser construído, configurado, testado e validado com uma loja real.

---

## 1. Resumo executivo

O repositório atual contém uma documentação consistente de produto, arquitetura, segurança, dados, API, billing e operação. Entretanto, a auditoria demonstrou que não há código de aplicação, banco, migrations, integrações, testes, infraestrutura ou pipeline de deploy versionados.

Portanto, o próximo trabalho não é adicionar mais documentação. É criar uma implementação mínima que possa operar uma loja piloto com segurança suficiente, medir leads e próximas ações e produzir evidência real de valor.

A ordem recomendada é:

```text
Decisões técnicas
→ scaffold executável
→ banco e autenticação
→ domínio de leads e veículos
→ score e próxima ação
→ dashboard operacional
→ importação e operação assistida
→ integrações reais
→ billing e franquias
→ hardening e escala
```

Não comece pelo chatbot completo. O primeiro objetivo é garantir que todo lead tenha **responsável, status, score e próxima ação**.

---

## 2. O que já existe e o que não existe

### 2.1 Já existe

| Área | Situação |
|---|---|
| Nicho | Lojas de veículos seminovos |
| Proposta | Transformar leads em conversas qualificadas, visitas e test-drives |
| Jornada | Lead recebido → qualificação → distribuição → visita → follow-up |
| Critérios de qualificação | Score, prazo, veículo, pagamento, troca e compromisso |
| Planos | Essencial, Comercial e Pro, como referência configurável |
| Arquitetura conceitual | Next.js, TypeScript, PostgreSQL, Redis, filas, storage e adapters |
| Modelo de dados | Entidades e invariantes documentadas |
| API | Lista de rotas e convenções documentadas |
| Segurança | Threat model e controles conceituais documentados |
| Design | Direção visual e mockups conceituais |
| Operação | Runbook, deploy, incidentes e backups descritos |
| Repositório | Criado no GitHub e privado |

### 2.2 Ainda não existe

- [ ] Aplicação Next.js executável.
- [ ] `package.json` e lockfile.
- [ ] Schema de banco.
- [ ] Migrations.
- [ ] Seeds e dados demo.
- [ ] Login e controle de sessão.
- [ ] Multi-tenancy executável.
- [ ] Páginas funcionais.
- [ ] API implementada.
- [ ] Testes automatizados.
- [ ] Integração oficial de WhatsApp.
- [ ] Provider real de IA.
- [ ] Billing real.
- [ ] Serviço de medição de uso.
- [ ] Workflows n8n versionados.
- [ ] Docker Compose funcional.
- [ ] CI/CD.
- [ ] Observabilidade.
- [ ] Ambiente de staging.
- [ ] Piloto real documentado.

---

## 3. Decisões que precisam ser fechadas antes do código

A documentação atual contém algumas alternativas. Antes de implementar, registre decisões definitivas em ADRs.

| Decisão | Recomendação inicial |
|---|---|
| Framework | Next.js App Router com TypeScript strict |
| UI | Tailwind CSS, shadcn/ui e Radix UI |
| ORM | Escolher **Drizzle** ou **Prisma**; não manter os dois |
| Banco | PostgreSQL |
| Fila | Redis + BullMQ apenas quando houver jobs reais |
| Auth | Provedor de autenticação com suporte a organizações e MFA; manter adapter próprio |
| IA | Provider abstrato com modo mock e provider real configurável |
| WhatsApp | API oficial/BSP oficial; não usar automação de WhatsApp Web |
| Billing | Provider abstrato; iniciar com cobrança manual ou mock durante o piloto |
| n8n | Executor auxiliar, nunca fonte de verdade do domínio |
| Deploy | Staging e produção isolados |
| Storage | S3-compatible privado para anexos |
| Monorepo | Começar com um único app modular; separar serviços somente quando necessário |

### Critério de conclusão desta fase

- [ ] Todas as escolhas acima foram confirmadas.
- [ ] Um ADR foi criado para cada decisão relevante.
- [ ] O sócio técnico e o responsável pelo produto concordaram com o corte do MVP.
- [ ] Nenhuma integração crítica permanece sem dono.

---

# 4. Fase 0 — Preparação do projeto

## Objetivo

Transformar o repositório documental em um repositório de desenvolvimento executável, sem ainda construir todas as funcionalidades.

## Implementar

- [ ] Criar aplicação Next.js com TypeScript strict.
- [ ] Configurar ESLint e Prettier.
- [ ] Configurar Tailwind e biblioteca de componentes.
- [ ] Criar estrutura `app`, `components`, `lib`, `server`, `db`, `tests`.
- [ ] Criar `.env.example`.
- [ ] Criar `docker-compose.yml` para PostgreSQL e Redis local.
- [ ] Adicionar script de lint.
- [ ] Adicionar script de typecheck.
- [ ] Adicionar script de testes.
- [ ] Adicionar README de desenvolvimento.
- [ ] Configurar branch protection quando houver equipe.
- [ ] Criar CI mínimo para lint, typecheck, testes e build.

## Estrutura inicial recomendada

```text
src/
  app/
    (auth)/
    (dashboard)/
    api/
  components/
  db/
    schema/
    migrations/
    seed/
  lib/
    auth/
    permissions/
    validation/
    observability/
  server/
    services/
    repositories/
    integrations/
    jobs/
  types/
tests/
  unit/
  integration/
  e2e/
docs/
```

## Critérios de aceite

- [ ] `pnpm install` funciona em uma máquina limpa.
- [ ] `pnpm dev` inicia a aplicação.
- [ ] `pnpm lint` passa.
- [ ] `pnpm typecheck` passa.
- [ ] `pnpm test` passa com pelo menos um teste real.
- [ ] `pnpm build` passa.
- [ ] Nenhum segredo está no Git.

---

# 5. Fase 1 — Banco, organizações e autenticação

## Objetivo

Criar a base de identidade e isolamento para que qualquer funcionalidade posterior nasça dentro de uma organização.

## Entidades mínimas

- `Organization`.
- `User`.
- `Membership` ou relação equivalente entre usuário e organização.
- `Role`.
- `AuditLog`.

## Papéis iniciais

| Papel | Permissões principais |
|---|---|
| Owner | Configuração, faturamento, usuários e todos os dados |
| Manager | Operação, equipe, leads, veículos e relatórios |
| Seller | Leads atribuídos, conversas, tarefas e agenda própria |
| Viewer | Leitura restrita |
| Support | Acesso temporário e auditado, sem acesso permanente amplo |

## Implementar

- [ ] Cadastro ou convite de usuário.
- [ ] Login e logout.
- [ ] Recuperação de acesso.
- [ ] Seleção de organização ativa.
- [ ] Verificação server-side de membership.
- [ ] Middleware de rotas protegidas.
- [ ] Permissões por caso de uso.
- [ ] Auditoria de login, convite e alterações de papel.
- [ ] Testes de acesso entre organizações.

## Critérios de aceite

- [ ] Usuário sem membership não acessa dados da organização.
- [ ] Trocar o `organization_id` no request não altera o tenant efetivo.
- [ ] Usuário vendedor não acessa faturamento.
- [ ] Acesso de suporte exige motivo e expiração.
- [ ] Toda mudança de permissão gera `AuditLog`.

---

# 6. Fase 2 — Domínio de veículos e leads

## Objetivo

Implementar o núcleo operacional sem depender de IA ou WhatsApp real.

## Entidades

- `Vehicle`.
- `Lead`.
- `TradeInVehicle`.
- `LeadEvent`.
- `Task`.
- `Appointment`.

## Estados de veículo

```text
available → reserved → sold
available → inactive
reserved → available
```

A aplicação nunca deve confirmar disponibilidade apenas com base em texto produzido pela IA. A disponibilidade deve vir do banco ou da fonte de estoque configurada.

## Estados de lead

```text
new → researching → interested → qualified → hot
                         ↓          ↓       ↓
                      nurture    scheduled → visited → proposal → won/lost
```

O estado `opted_out` deve impedir comunicações promocionais.

## Implementar

- [ ] CRUD de veículos.
- [ ] Importação de veículos por CSV.
- [ ] Validação de duplicidade de estoque.
- [ ] CRUD de leads.
- [ ] Busca e filtros.
- [ ] Associação entre lead e veículo.
- [ ] Cadastro de veículo de troca.
- [ ] Eventos de mudança de status.
- [ ] Atribuição a vendedor ou fila.
- [ ] Criação de tarefa.
- [ ] Criação de agendamento.
- [ ] Timeline do lead.
- [ ] Histórico de alterações.

## Critérios de aceite

- [ ] Todo lead pertence a uma organização.
- [ ] Todo lead ativo tem responsável ou fila.
- [ ] Todo lead ativo tem próxima ação ou justificativa.
- [ ] Um veículo vendido não aparece como disponível.
- [ ] Importação repetida não cria duplicidade silenciosa.
- [ ] Todas as transições importantes criam eventos.

---

# 7. Fase 3 — Score de qualificação determinístico

## Objetivo

Colocar a regra comercial no produto antes de adicionar geração de texto por IA.

## Score inicial

Use 0 a 100 pontos, dividido em quatro dimensões:

| Dimensão | Pontos |
|---|---:|
| Interesse específico | 0–25 |
| Prazo de compra | 0–25 |
| Capacidade de avançar | 0–25 |
| Compromisso com próximo passo | 0–25 |

## Regras sugeridas

- Modelo e versão específicos: pontuação alta.
- Compra nesta semana: pontuação alta.
- Financiamento ou troca informados: pontuação intermediária.
- Visita ou test-drive confirmado: pontuação muito alta.
- Pergunta genérica sem veículo: pontuação baixa.
- Ausência de prazo: não presumir urgência.

## Faixas

| Score | Classe | Ação |
|---:|---|---|
| 0–24 | Pesquisa | Nutrir e não ocupar vendedor prioritário |
| 25–49 | Interessado | Solicitar dados faltantes |
| 50–74 | Qualificado | Encaminhar para vendedor |
| 75–100 | Quente | Priorizar contato e próximo compromisso |

## Implementar

- [ ] Função de score pura e testável.
- [ ] Registro de fatores que geraram a pontuação.
- [ ] Recalculo manual.
- [ ] Recalculo ao atualizar dados relevantes.
- [ ] Histórico de mudanças de score.
- [ ] Explicação legível para o vendedor.
- [ ] Proteção contra critérios discriminatórios.

## Critérios de aceite

- [ ] Mesma entrada produz mesma pontuação.
- [ ] O vendedor consegue entender por que o lead recebeu o score.
- [ ] O score não usa nome, foto, gênero, raça, idade, bairro ou estilo de escrita.
- [ ] A IA não pode alterar score sem registrar origem e revisão.

---

# 8. Fase 4 — Interface operacional MVP

## Objetivo

Criar uma aplicação navegável que a loja consiga usar no piloto.

## Telas obrigatórias

### Dashboard

- [ ] Leads novos.
- [ ] Leads quentes.
- [ ] Visitas hoje.
- [ ] Leads sem próxima ação.
- [ ] Funil.
- [ ] Próximas ações.

### Leads

- [ ] Lista com busca e filtros.
- [ ] Filtro por score.
- [ ] Filtro por vendedor.
- [ ] Filtro por etapa.
- [ ] Filtro por próxima ação.
- [ ] Ações em lote somente quando seguras.

### Detalhe do lead

- [ ] Identidade e contato.
- [ ] Score e explicação.
- [ ] Veículo de interesse.
- [ ] Veículo de troca.
- [ ] Timeline.
- [ ] Próxima ação.
- [ ] Responsável.
- [ ] Agendamento.
- [ ] Observações.

### Veículos

- [ ] Lista.
- [ ] Status.
- [ ] Busca por marca, modelo e versão.
- [ ] Importação.
- [ ] Detalhe do estoque.

### Tarefas e agenda

- [ ] Lista de tarefas.
- [ ] Agenda de visitas e test-drives.
- [ ] Conclusão ou reagendamento.
- [ ] Alertas de atraso.

### Uso

- [ ] Franquia utilizada.
- [ ] Consumo atual.
- [ ] Alertas.
- [ ] Plano atual.

## Critérios de aceite

- [ ] Uma loja demo pode concluir o fluxo sem acessar banco.
- [ ] O layout funciona em desktop e tablet.
- [ ] Estados vazios são tratados.
- [ ] Loading, erro e sucesso são visíveis.
- [ ] A interface não expõe stack trace ou IDs internos desnecessários.
- [ ] A ação primária fica clara em cada tela.

---

# 9. Fase 5 — Importação e operação assistida do piloto

## Objetivo

Permitir que a primeira loja use o produto sem depender de integrações complexas.

## Implementar

- [ ] Importação de leads por CSV.
- [ ] Importação de veículos por CSV.
- [ ] Validação e prévia antes de confirmar importação.
- [ ] Relatório de linhas inválidas.
- [ ] Deduplicação por telefone e oportunidade.
- [ ] Registro da origem do lead.
- [ ] Atribuição manual para vendedores.
- [ ] Registro de conversa por texto ou arquivo autorizado.
- [ ] Score automático após importação.
- [ ] Próxima ação obrigatória.
- [ ] Exportação de relatório do piloto.

## Operação de concierge

Durante o piloto, a equipe pode complementar manualmente o que ainda não estiver automatizado. Isso deve ser registrado como atividade operacional, não escondido como se fosse automação completa.

O sistema deve permitir marcar a origem de cada ação:

```text
manual | rule_engine | ai_suggestion | automated_workflow
```

## Critérios de aceite

- [ ] A loja consegue importar sua base sem intervenção direta no banco.
- [ ] Erros de importação são explicados.
- [ ] O relatório mostra leads, responsáveis, próximas ações e visitas.
- [ ] A equipe consegue operar por 14 dias.
- [ ] Existe fallback manual para qualquer integração indisponível.

---

# 10. Fase 6 — IA controlada

## Objetivo

Adicionar IA onde ela reduz trabalho, sem permitir decisões comerciais perigosas ou respostas não verificadas.

## Casos de uso permitidos no MVP

- [ ] Classificação da intenção.
- [ ] Extração de modelo, prazo, pagamento e troca.
- [ ] Resumo de conversa.
- [ ] Sugestão de próxima ação.
- [ ] Rascunho de resposta para revisão humana.

## Casos proibidos no MVP

- [ ] Confirmar preço sem fonte aprovada.
- [ ] Confirmar disponibilidade sem consultar estoque.
- [ ] Prometer aprovação de crédito.
- [ ] Avaliar troca como valor definitivo.
- [ ] Negociar sem regra aprovada.
- [ ] Enviar mensagem promocional para opt-out.
- [ ] Decidir sozinho sobre lead sensível.

## Implementar

- [ ] Interface `AIProvider`.
- [ ] Provider mock.
- [ ] Provider real configurável por ambiente.
- [ ] Schemas de saída com validação.
- [ ] Timeout.
- [ ] Retry limitado.
- [ ] Circuit breaker.
- [ ] Registro de modelo, tokens, custo estimado e confiança.
- [ ] Redação de dados desnecessários.
- [ ] Handoff humano quando a confiança for baixa.
- [ ] Limite de custo por organização.

## Critérios de aceite

- [ ] Falha do provider não impede registro do lead.
- [ ] Resposta inválida é rejeitada ou enviada para revisão.
- [ ] Conteúdo do lead é tratado como dado, não como instrução de sistema.
- [ ] Nenhuma resposta sensível é enviada automaticamente sem aprovação.
- [ ] O custo técnico é associado ao tenant correto.

---

# 11. Fase 7 — Inbox e WhatsApp oficial

## Objetivo

Integrar conversas reais somente depois que o fluxo interno de leads estiver funcionando.

## Implementar

- [ ] Escolher BSP/provedor oficial.
- [ ] Configurar conta do cliente.
- [ ] Configurar número e permissões.
- [ ] Validar assinatura de webhook.
- [ ] Implementar recebimento de mensagens.
- [ ] Implementar deduplicação por `external_message_id`.
- [ ] Implementar status de entrega.
- [ ] Implementar envio de mensagem aprovado.
- [ ] Implementar templates quando aplicável.
- [ ] Implementar opt-out.
- [ ] Implementar janela de atendimento.
- [ ] Implementar handoff humano.
- [ ] Implementar retries e dead-letter queue.

## Critérios de aceite

- [ ] Webhook falso é rejeitado.
- [ ] Evento duplicado não cria mensagem duplicada.
- [ ] Mensagem de lead é associada à organização correta.
- [ ] Mensagem promocional para opt-out é bloqueada.
- [ ] Falha do WhatsApp cria alerta e não perde o evento.
- [ ] A equipe consegue assumir a conversa.

---

# 12. Fase 8 — n8n como automação auxiliar

## Objetivo

Usar n8n somente para integrações e tarefas assíncronas, mantendo o domínio no produto.

## Workflows iniciais

- [ ] Lead recebido → normalização → API do produto.
- [ ] Lead qualificado → criar tarefa.
- [ ] Visita confirmada → criar lembrete.
- [ ] Lead sem próxima ação → alerta interno.
- [ ] Relatório diário → gerar resumo operacional.

## Regras

- [ ] Workflows versionados.
- [ ] Workflows idempotentes.
- [ ] Sem credenciais no export.
- [ ] Sem dados de produção em fixtures.
- [ ] O n8n não decide permissões.
- [ ] O n8n não é banco de leads.
- [ ] O n8n não é fonte de billing.
- [ ] Todo workflow tem timeout e tratamento de erro.
- [ ] Todo workflow registra `correlation_id`.

## Critérios de aceite

- [ ] Reprocessar um evento não duplica lead, mensagem ou cobrança.
- [ ] Falha de workflow aparece no painel operacional.
- [ ] Credenciais podem ser rotacionadas.
- [ ] É possível desativar um workflow sem derrubar o produto.

---

# 13. Fase 9 — Uso, franquias e billing

## Objetivo

Implementar cobrança recorrente somente depois que o consumo real estiver sendo medido.

## Unidade comercial

A unidade inicial deve ser **lead processado**, não token. Internamente, registre também chamadas, tokens, modelo e custo estimado.

## Planos de referência

| Plano | Franquia | Mensalidade | Excedente |
|---|---:|---:|---:|
| Essencial | 150 leads | R$ 2.190 | R$ 7,90 |
| Comercial | 400 leads | R$ 3.990 | R$ 6,90 |
| Pro | 800 leads | R$ 7.990 | R$ 5,90 |

Esses valores são configuráveis e devem ser validados com custos reais, tributação e estratégia comercial.

## Implementar

- [ ] `Plan` configurável.
- [ ] `Subscription` por organização.
- [ ] `UsagePeriod`.
- [ ] `UsageEvent` idempotente.
- [ ] Registro de custo técnico.
- [ ] Alertas de 70%, 80%, 90% e 100%.
- [ ] Teto de uso.
- [ ] Excedente autorizado.
- [ ] Invoice.
- [ ] Adapter de billing.
- [ ] Provider mock.
- [ ] Webhook idempotente.
- [ ] Upgrade e downgrade.
- [ ] Cancelamento.
- [ ] Exportação de dados no encerramento.

## Critérios de aceite

- [ ] O mesmo evento de uso não é contabilizado duas vezes.
- [ ] Dois workers concorrentes não duplicam cobrança.
- [ ] O cliente recebe alerta antes do limite.
- [ ] O sistema respeita o teto.
- [ ] O invoice aponta para um período válido.
- [ ] O provider real pode ser substituído por adapter.

---

# 14. Fase 10 — Segurança, privacidade e LGPD operacional

## Implementar

- [ ] Matriz de permissões.
- [ ] Isolamento por organização em todas as queries.
- [ ] Validação de autorização no servidor.
- [ ] Criptografia de credenciais de integração.
- [ ] Secret manager.
- [ ] Redaction de logs.
- [ ] Rate limiting.
- [ ] Proteção de webhook.
- [ ] Política de retenção.
- [ ] Opt-out.
- [ ] Exportação de dados.
- [ ] Exclusão ou anonimização.
- [ ] Auditoria de acesso a dados sensíveis.
- [ ] Rotação de credenciais.
- [ ] Backups e teste de restauração.
- [ ] Procedimento de incidente.
- [ ] Termos, política de privacidade e contrato revisados profissionalmente.

## Testes de segurança obrigatórios

- [ ] Usuário de tenant A não vê tenant B.
- [ ] Usuário sem permissão não acessa billing.
- [ ] ID alterado na URL não bypassa autorização.
- [ ] Webhook sem assinatura é rejeitado.
- [ ] Payload grande é limitado.
- [ ] Upload com MIME falso é rejeitado.
- [ ] Token não aparece em log.
- [ ] Prompt injection não altera instruções do sistema.
- [ ] Opt-out bloqueia envio.

---

# 15. Fase 11 — Observabilidade e operação

## Implementar

- [ ] Logs estruturados.
- [ ] `request_id`.
- [ ] `correlation_id`.
- [ ] Métricas de latência.
- [ ] Métricas de erro.
- [ ] Métricas de fila.
- [ ] Métricas de custo de IA.
- [ ] Health check.
- [ ] Readiness check.
- [ ] Alertas de provider indisponível.
- [ ] Alertas de consumo anormal.
- [ ] Alertas de webhook parado.
- [ ] Dashboard de operação.
- [ ] Runbook de incidentes.
- [ ] Procedimento de rollback.
- [ ] Backup automatizado.
- [ ] Restauração testada.

## Critérios de aceite

- [ ] Um operador consegue descobrir por que um lead não foi processado.
- [ ] Uma falha de provider não fica silenciosa.
- [ ] Existe trilha desde mensagem externa até ação interna.
- [ ] É possível identificar o tenant afetado.
- [ ] O restore de backup foi testado em staging.

---

# 16. Fase 12 — Testes e qualidade

## Pirâmide de testes

| Camada | Cobertura esperada |
|---|---|
| Unitário | Score, regras, schemas, permissões e billing |
| Integração | Banco, repositories, filas, providers mock e webhooks |
| E2E | Login, criação de lead, atribuição, próxima ação, agenda e uso |
| Contrato | WhatsApp, IA, billing e adapters |
| Segurança | Isolamento, autorização, secrets, payloads e opt-out |
| Regressão | Fluxos críticos a cada release |

## Casos críticos

- [ ] Criar lead.
- [ ] Atualizar lead.
- [ ] Atribuir lead.
- [ ] Calcular score.
- [ ] Criar próxima ação.
- [ ] Agendar visita.
- [ ] Importar CSV.
- [ ] Receber webhook duplicado.
- [ ] Processar falha de IA.
- [ ] Bloquear opt-out.
- [ ] Contabilizar uso.
- [ ] Rejeitar tenant incorreto.
- [ ] Fazer upgrade.
- [ ] Cancelar assinatura.

## Definição de pronto para uma feature

Uma feature só está pronta quando:

- [ ] Requisito foi escrito.
- [ ] Caso de uso foi implementado.
- [ ] Validação de entrada existe.
- [ ] Autorização existe.
- [ ] Erros foram tratados.
- [ ] Eventos/auditoria foram considerados.
- [ ] Testes foram criados.
- [ ] Loading, vazio e erro existem na UI.
- [ ] Documentação foi atualizada.
- [ ] Não há segredo ou dado real nos fixtures.
- [ ] Revisão foi feita.

---

# 17. Fase 13 — Staging e deploy

## Ambientes

| Ambiente | Finalidade |
|---|---|
| Local | Desenvolvimento individual |
| Staging | Testes integrados e demo |
| Produção | Clientes reais |

Cada ambiente precisa ter banco, storage, Redis, webhooks e credenciais separados.

## Implementar

- [ ] Docker Compose local.
- [ ] Configuração de staging.
- [ ] Configuração de produção.
- [ ] Variáveis de ambiente documentadas.
- [ ] Migrations automatizadas com controle.
- [ ] Pipeline de CI.
- [ ] Pipeline de deploy.
- [ ] Smoke tests pós-deploy.
- [ ] Rollback documentado.
- [ ] Backups.
- [ ] Domínio e TLS.
- [ ] Monitoramento.

## Checklist de release

- [ ] CI verde.
- [ ] Migration revisada.
- [ ] Backup recente.
- [ ] Secrets configurados.
- [ ] Webhooks apontando para ambiente correto.
- [ ] Provider mock desligado em produção.
- [ ] Smoke test executado.
- [ ] Plano de rollback aprovado.

---

# 18. Fase 14 — Preparação do piloto de 14 dias

## Oferta piloto

- **Preço:** R$ 2.900.
- **Duração:** 14 dias corridos.
- **Franquia:** 100 leads processados.
- **Excedente:** R$ 9,90 por lead adicional, somente com autorização.
- **Teto:** 150 leads.
- **Pagamento:** 60% no início e 40% no dia 8.
- **Suporte:** até 6 horas.
- **Veículos importados:** até 100.
- **Templates:** até 15.

Esses valores são uma hipótese comercial inicial e devem ser revisados com dados reais de custo, impostos e esforço.

## Antes do início

- [ ] Contrato ou termo de piloto assinado.
- [ ] Primeira parcela recebida.
- [ ] Formulário de onboarding preenchido.
- [ ] Responsável da loja definido.
- [ ] Lista de vendedores recebida.
- [ ] Estoque recebido.
- [ ] Canais e credenciais autorizados.
- [ ] Política de dados apresentada.
- [ ] Escopo confirmado.
- [ ] Critérios de sucesso acordados.

## Durante o piloto

- [ ] Medir leads recebidos.
- [ ] Medir tempo de resposta quando possível.
- [ ] Medir leads com responsável.
- [ ] Medir leads com próxima ação.
- [ ] Medir visitas agendadas.
- [ ] Medir test-drives.
- [ ] Registrar falhas.
- [ ] Registrar ações manuais.
- [ ] Monitorar consumo.
- [ ] Fazer revisão intermediária no dia 7.

## Encerramento

- [ ] Relatório final entregue.
- [ ] Dados exportados ou mantidos conforme contrato.
- [ ] Reunião de resultados realizada.
- [ ] Próximas melhorias priorizadas.
- [ ] Depoimento solicitado, se apropriado.
- [ ] Proposta de continuidade apresentada.
- [ ] Decisão de conversão para plano mensal registrada.

---

# 19. Backlog priorizado

## P0 — obrigatório antes do primeiro piloto funcional

- [ ] Scaffold executável.
- [ ] Banco e migrations.
- [ ] Autenticação.
- [ ] Organizações e permissões.
- [ ] Leads.
- [ ] Veículos.
- [ ] Score determinístico.
- [ ] Responsável e próxima ação.
- [ ] Dashboard simples.
- [ ] Importação CSV.
- [ ] Seed demo.
- [ ] Testes de isolamento.
- [ ] Backup básico.
- [ ] Fallback manual.

## P1 — necessário para piloto com operação mais eficiente

- [ ] Inbox interno.
- [ ] Registro de mensagens.
- [ ] Provider mock de IA.
- [ ] Resumo e extração estruturada.
- [ ] Auditoria completa.
- [ ] Tarefas e agenda.
- [ ] Métricas do piloto.
- [ ] Workflows n8n versionados.
- [ ] Staging.
- [ ] Monitoramento.

## P2 — necessário para produto comercial recorrente

- [ ] WhatsApp oficial.
- [ ] Provider real de IA.
- [ ] Billing real.
- [ ] Franquia e excedentes automáticos.
- [ ] Webhooks de produção.
- [ ] Integração de estoque.
- [ ] Multiusuário avançado.
- [ ] Retenção configurável.
- [ ] Exportação self-service.
- [ ] E2E completo.

## P3 — escala e diferenciação

- [ ] Reativação de base.
- [ ] Múltiplos canais.
- [ ] Analytics avançado.
- [ ] Recomendação de estoque.
- [ ] Distribuição inteligente.
- [ ] Integração com CRM externos.
- [ ] Portal para grupos de lojas.
- [ ] Automação avançada de campanhas.

---

# 20. Ordem recomendada de execução

## Sprint 1 — Fundação

Entregar scaffold, banco local, autenticação básica, organizações, permissões, lint, typecheck, testes e CI.

**Pronto quando:** a aplicação roda, o usuário entra e o tenant é isolado.

## Sprint 2 — Núcleo comercial

Entregar veículos, leads, importação, status, responsáveis, tarefas e próxima ação.

**Pronto quando:** uma loja demo consegue controlar leads sem planilha.

## Sprint 3 — Qualificação e dashboard

Entregar score determinístico, explicação do score, funil, KPIs e timeline.

**Pronto quando:** o gestor consegue saber quais leads precisam de atenção e por quê.

## Sprint 4 — Operação assistida

Entregar dados demo, relatório do piloto, registro manual de mensagens, tarefas e acompanhamento de consumo.

**Pronto quando:** uma primeira loja consegue operar por 14 dias com fallback manual.

## Sprint 5 — IA controlada

Adicionar provider mock, extração, resumo, rascunho e validação de schema.

**Pronto quando:** a IA ajuda sem enviar respostas comerciais sensíveis sozinha.

## Sprint 6 — WhatsApp e n8n

Adicionar webhook oficial, inbox, envio autorizado e workflows idempotentes.

**Pronto quando:** mensagens reais entram, são associadas ao lead correto e podem ser assumidas por uma pessoa.

## Sprint 7 — Billing e produção

Adicionar franquia, uso, excedente, invoices, provider real, staging, observabilidade e deploy.

**Pronto quando:** o cliente pode ser cobrado, o uso é auditável e a operação pode ser recuperada após falha.

## Sprint 8 — Hardening e escala

Adicionar testes de segurança, restauração, políticas de retenção, otimização e integrações adicionais.

**Pronto quando:** o produto suporta mais de um cliente com controles verificados.

---

# 21. Critério geral de pronto para vender como SaaS

O sistema só deve ser apresentado como SaaS funcional quando todos estes itens estiverem completos:

- [ ] Cliente consegue entrar sem intervenção técnica.
- [ ] Organização e usuários funcionam.
- [ ] Leads e veículos são operacionais.
- [ ] Inbox ou canal contratado funciona.
- [ ] Score e próxima ação funcionam.
- [ ] O sistema não inventa estoque, preço ou financiamento.
- [ ] Billing e franquia são mensuráveis.
- [ ] Uso é isolado por tenant.
- [ ] Webhooks são idempotentes.
- [ ] Backups são testados.
- [ ] Logs são auditáveis.
- [ ] Erros têm fallback.
- [ ] Testes críticos passam.
- [ ] Contrato e política de privacidade foram revisados.
- [ ] Existe processo de suporte.
- [ ] Existe procedimento de saída e exportação de dados.

Antes disso, venda como **piloto de implantação assistida**, com escopo, limites e acompanhamento humano explicitamente definidos.

---

# 22. Próximas cinco ações imediatas

1. [ ] Escolher ORM, autenticação, provedor de deploy e provedor oficial de WhatsApp.
2. [ ] Criar o scaffold executável no repositório GitHub.
3. [ ] Implementar banco, organização, usuário, lead, veículo, tarefa e score.
4. [ ] Criar uma loja demo navegável com seed e dashboard.
5. [ ] Preparar uma primeira loja piloto antes de construir billing e automações complexas.

> A decisão mais importante é preservar a ordem: primeiro provar a operação com leads, responsáveis e próximas ações; depois automatizar; por último transformar em SaaS escalável.

---

## Referências internas

- Repositório: `https://github.com/mvppartnersbr-design/lead-que-vira-visita`
- Documentação de produto: [`docs/product`](https://github.com/mvppartnersbr-design/lead-que-vira-visita/tree/main/docs/product)
- Arquitetura: [`docs/architecture`](https://github.com/mvppartnersbr-design/lead-que-vira-visita/tree/main/docs/architecture)
- Segurança: [`docs/security`](https://github.com/mvppartnersbr-design/lead-que-vira-visita/tree/main/docs/security)
- Billing: [`docs/billing`](https://github.com/mvppartnersbr-design/lead-que-vira-visita/tree/main/docs/billing)

> Este documento é um plano técnico e operacional. Contratos, política de privacidade, tratamento de dados pessoais, tributação e conformidade devem ser revisados por profissionais qualificados antes da operação comercial.
