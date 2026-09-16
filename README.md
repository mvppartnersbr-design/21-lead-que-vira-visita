# Lead que Vira Visita

Plataforma SaaS verticalizada para lojas de veículos seminovos que transforma leads recebidos por WhatsApp, site, redes sociais e portais em oportunidades organizadas, visitas, test-drives e follow-ups.

> **A plataforma ajuda a loja a saber qual lead merece atenção agora, por quê, por quem e qual é o próximo passo.**

## Status

O projeto está em fase de especificação e construção do MVP. Esta documentação é a fonte de verdade para produto, arquitetura, operação e decisões técnicas.

## O problema

Lojas de seminovos recebem contatos, mas enfrentam respostas atrasadas, distribuição desigual, informações inconsistentes, estoque desatualizado, leads sem próxima ação e pouca visibilidade para gestores.

O produto organiza a jornada:

```text
Lead recebido → veículo identificado → qualificação → score → distribuição → visita/test-drive → follow-up → proposta/venda
```

## Capacidades do MVP

| Área | Capacidade |
|---|---|
| Operação | Inbox, leads, veículos, tarefas, agenda e funil |
| Qualificação | Score de 0 a 100, intenção, prazo, modalidade e troca |
| Atendimento | WhatsApp oficial, templates, sugestões de resposta e handoff humano |
| Gestão | Dashboard, distribuição, próximas ações e auditoria |
| IA | Extração estruturada, resumo, classificação e rascunho controlado |
| SaaS | Multi-tenant, usuários, planos, franquia, consumo e excedentes |
| Segurança | Isolamento de organizações, controle de acesso, logs e privacidade |

## Planos comerciais de referência

Os valores abaixo são defaults configuráveis, não regras hardcoded:

| Plano | Franquia | Mensalidade de referência | Excedente |
|---|---:|---:|---:|
| Essencial | 150 leads processados | R$ 2.190/mês | R$ 7,90/unidade |
| Comercial | 400 leads processados | R$ 3.990/mês | R$ 6,90/unidade |
| Pro | 800 leads processados | R$ 7.990/mês | R$ 5,90/unidade |

## Stack resumida

- Next.js App Router e TypeScript strict;
- Tailwind CSS, shadcn/ui e Radix UI;
- PostgreSQL com Prisma ou Drizzle;
- Redis e BullMQ para jobs;
- storage S3-compatible;
- provider oficial do WhatsApp;
- provider abstrato de IA;
- provider abstrato de billing;
- Docker Compose para desenvolvimento;
- Playwright para testes end-to-end.

## Estrutura documental

| Caminho | Conteúdo |
|---|---|
| `docs/product` | Visão, requisitos, personas e regras de negócio |
| `docs/architecture` | Arquitetura, decisões e diagramas textuais |
| `docs/design` | Design system, UX e acessibilidade |
| `docs/api` | Convenções e endpoints |
| `docs/data` | Modelo de dados e eventos |
| `docs/security` | Segurança, privacidade e threat model |
| `docs/billing` | Planos, uso, franquias e faturamento |
| `docs/integrations` | WhatsApp, IA e provedores externos |
| `docs/development` | Setup, convenções e fluxo de contribuição |
| `docs/operations` | Deploy, observabilidade, incidentes e backups |
| `adr` | Architecture Decision Records |
| `examples` | Payloads, seeds e exemplos de configuração |

## Princípios

1. **Humano no controle:** a IA sugere, classifica e resume; decisões comerciais sensíveis continuam com pessoas autorizadas.
2. **Fonte confirmada:** estoque, preço, troca, financiamento e disponibilidade não podem ser inventados.
3. **Próxima ação sempre visível:** todo lead ativo deve ter responsável e próximo passo.
4. **Tenant isolado:** nenhum dado pode atravessar organizações.
5. **Consumo rastreável:** toda unidade de uso deve ter evento idempotente e auditável.
6. **Infraestrutura oficial:** usar somente APIs oficiais e provedores documentados.
7. **Operação mensurável:** decisões devem ser apoiadas por dados reais, não por métricas inventadas.

## Quick start documental

Leia nesta ordem:

1. [`docs/product/vision.md`](docs/product/vision.md)
2. [`docs/product/requirements.md`](docs/product/requirements.md)
3. [`docs/architecture/overview.md`](docs/architecture/overview.md)
4. [`docs/architecture/decisions.md`](docs/architecture/decisions.md)
5. [`docs/design/design-system.md`](docs/design/design-system.md)
6. [`docs/data/data-model.md`](docs/data/data-model.md)
7. [`docs/api/README.md`](docs/api/README.md)
8. [`docs/security/security.md`](docs/security/security.md)
9. [`docs/billing/billing.md`](docs/billing/billing.md)
10. [`docs/development/setup.md`](docs/development/setup.md)

## Licença e responsabilidade

Defina a licença do código antes de publicar o repositório. Não versionar credenciais, dados reais de leads, tokens, exportações do n8n ou conversas de clientes.

Este repositório é documentação de produto e engenharia. Políticas legais, termos de serviço e tratamento de dados devem ser revisados por profissionais qualificados antes da operação comercial.
