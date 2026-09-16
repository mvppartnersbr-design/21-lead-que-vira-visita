# Decisões arquiteturais

## ADR-001 — Multi-tenant desde o início

**Decisão:** toda entidade de negócio pertence a uma organização.

**Motivo:** o produto é SaaS e precisa suportar várias lojas sem risco de mistura de dados.

**Consequência:** todas as queries, índices, eventos, caches e jobs precisam carregar contexto de organização.

## ADR-002 — Providers abstratos

**Decisão:** WhatsApp, IA, billing, calendário e estoque serão acessados por interfaces.

**Motivo:** permite usar mocks no desenvolvimento, trocar fornecedor e evitar acoplamento ao vendor.

**Consequência:** cada provider precisa de contrato, adapter, testes e tratamento de erros próprio.

## ADR-003 — WhatsApp oficial

**Decisão:** usar somente a WhatsApp Business Platform ou BSP oficial.

**Motivo:** reduzir risco de bloqueio, instabilidade e uso não autorizado.

**Consequência:** templates, janela de atendimento, opt-out e status de entrega precisam ser modelados.

## ADR-004 — IA com aprovação humana

**Decisão:** a IA pode classificar, extrair, resumir e sugerir, mas não controla decisões comerciais sensíveis.

**Motivo:** estoque, preço, troca e financiamento exigem fonte confiável e responsabilidade humana.

**Consequência:** o sistema precisa de confidence score, handoff e trilha de auditoria.

## ADR-005 — Medição de uso como evento de domínio

**Decisão:** o consumo será gravado por `UsageEvent`, e não calculado somente ao fechar o mês.

**Motivo:** permite idempotência, auditoria, alertas em tempo real e correção de falhas.

**Consequência:** toda operação faturável precisa de chave de idempotência e política de unidade.

## ADR-006 — Billing por franquia e excedente

**Decisão:** os planos possuem unidades incluídas, excedente configurável e teto de segurança.

**Motivo:** o cliente ganha previsibilidade e o negócio protege a margem contra consumo inesperado.

**Consequência:** billing precisa ter estado, períodos, alertas, invoices e circuit breaker.

## ADR-007 — Domínio independente do n8n

**Decisão:** o n8n pode executar automações, mas não deve ser a fonte de verdade do produto.

**Motivo:** leads, uso, billing, permissões e auditoria precisam de consistência transacional.

**Consequência:** o n8n é tratado como executor externo; eventos importantes devem ser confirmados na aplicação.

## ADR-008 — Interface operacional, não editor técnico

**Decisão:** o cliente utiliza inbox, leads, tarefas, agenda e billing; não precisa operar o editor técnico de workflows.

**Motivo:** o produto vendido é a operação, não a ferramenta interna de automação.

**Consequência:** acesso ao n8n, quando existir, deve ser restrito e auditado.
