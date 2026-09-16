# Design system

## Direção visual

O produto deve parecer um cockpit de vendas B2B: claro, confiável, denso o suficiente para uso diário e sem estética de chatbot experimental.

A paleta base utiliza fundo branco ou cinza muito claro, azul profundo para ações primárias, verde para estados positivos, âmbar para atenção e vermelho somente para erro ou risco. Não utilizar cor como único indicador.

## Tokens

```css
:root {
  --color-bg: #f8fafc;
  --color-surface: #ffffff;
  --color-text: #0f172a;
  --color-muted: #64748b;
  --color-primary: #0f3d56;
  --color-primary-hover: #0a2f43;
  --color-success: #15803d;
  --color-warning: #b45309;
  --color-danger: #b91c1c;
  --color-border: #e2e8f0;
  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 14px;
  --shadow-sm: 0 1px 2px rgba(15, 23, 42, .06);
}
```

## Tipografia

Usar Inter ou equivalente sans-serif. Títulos devem ter hierarquia clara. Textos de operação devem priorizar leitura rápida. Números de score, consumo e funil podem usar peso semibold, nunca uma escala visual exagerada.

## Componentes

| Componente | Uso |
|---|---|
| `AppShell` | Estrutura global autenticada |
| `Sidebar` | Navegação principal |
| `Topbar` | Organização, busca, notificações e usuário |
| `DataTable` | Leads, veículos, usuários e invoices |
| `FilterBar` | Filtros persistentes e busca |
| `LeadScoreBadge` | Score com faixa e texto acessível |
| `UsageMeter` | Franquia, uso, excedente e projeção |
| `Timeline` | Eventos de lead e auditoria |
| `ConversationPanel` | Conversa e ações de atendimento |
| `LeadContextPanel` | Contexto comercial ao lado da conversa |
| `TaskCard` | Próxima ação e prazo |
| `AppointmentCalendar` | Visitas e test-drives |
| `EmptyState` | Ausência de dados com instrução |
| `ErrorState` | Falha com mensagem acionável |
| `UpgradeDialog` | Mudança de plano e confirmação |

## Estados

Toda tela deve definir loading, vazio, erro, sucesso, sem permissão e dados desatualizados. A experiência não pode depender apenas de toasts: ações críticas precisam aparecer na entidade ou timeline.

## Inbox

No desktop, usar três colunas: lista de conversas, conversa atual e painel de contexto. No mobile, empilhar as áreas e permitir retornar à lista.

O painel do lead deve mostrar veículo, score, breakdown, prazo, modalidade, troca, responsável, próxima ação, agendamento, resumo da IA e alertas de confirmação.

## Billing

A página de billing deve mostrar claramente:

- plano atual;
- franquia incluída;
- consumo do período;
- excedente;
- projeção;
- alertas;
- teto;
- invoice;
- próximo ciclo;
- ação de upgrade.

Nunca esconder o limite atrás de tooltip. O cliente deve entender quanto já consumiu e o que acontece ao atingir o teto.

## Acessibilidade

- HTML semântico;
- foco visível;
- navegação por teclado;
- labels para campos;
- contraste adequado;
- mensagens de erro associadas a campos;
- status anunciados quando necessário;
- não depender apenas de ícones;
- tabelas responsivas com alternativa de cards no mobile.

## Tom de voz

O texto deve ser operacional, claro e respeitoso. Evitar jargão técnico para usuários de loja. Preferir “leads processados” a “tokens”, “próxima ação” a “trigger” e “confirmar com vendedor” a “fallback humano”.
