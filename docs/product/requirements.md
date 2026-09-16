# Requisitos do produto

## Requisitos funcionais

### Organização e usuários

- Criar organização e configurar fuso horário.
- Convidar usuários por papel.
- Isolar todos os dados por organização.
- Permitir ativar ou pausar integrações.
- Permitir configurar plano, franquia e limites.

### Veículos

- Cadastrar veículo manualmente.
- Importar catálogo por CSV ou JSON validado.
- Registrar status: disponível, reservado, vendido, inativo e desconhecido.
- Informar quando a última atualização está vencida.
- Nunca confirmar disponibilidade quando o status for desconhecido.

### Leads

- Criar lead manualmente, via webhook ou via importação.
- Associar fonte, campanha e veículo.
- Manter score e breakdown.
- Registrar prazo, modalidade, troca e próxima ação.
- Distribuir para vendedor ou equipe.
- Registrar timeline completa.

### Conversas

- Listar conversas por status, canal e responsável.
- Exibir mensagens em ordem temporal.
- Permitir assumir conversa.
- Permitir enviar mensagem manual.
- Permitir solicitar resumo ou rascunho de IA.
- Bloquear automação quando houver handoff obrigatório.

### Agenda

- Agendar visita, test-drive, ligação e videochamada.
- Associar agendamento a lead e veículo.
- Criar lembrete e tarefa de confirmação.
- Registrar cancelamento e comparecimento.

### Billing e consumo

- Exibir plano atual e franquia.
- Registrar eventos de uso idempotentes.
- Calcular unidades usadas e excedentes.
- Emitir alertas de uso.
- Permitir upgrade e cancelamento conforme provider.
- Mostrar invoices e status de pagamento.
- Nunca processar cartão diretamente no frontend.

## Regras de negócio

1. Todo lead ativo deve ter `assigned_to` ou permanecer explicitamente na fila.
2. Todo lead ativo deve ter `next_action_at` ou uma razão registrada para não ter próxima ação.
3. Visita/test-drive confirmado cria tarefa imediata.
4. Estoque desconhecido impede confirmação automática de disponibilidade.
5. Pedido de humano desliga o agente automático naquela conversa até reativação autorizada.
6. Opt-out impede comunicações promocionais futuras.
7. Dados extraídos pela IA não sobrescrevem dados confirmados sem revisão.
8. Um evento de uso deve ter chave de idempotência única.
9. O score é prioridade comercial e não é score de crédito.
10. Atingir o teto pausa automações caras, mas não deve apagar ou impedir a captação básica.
11. Apenas papéis autorizados podem alterar preço, plano, limite e credenciais.
12. Excluir uma organização exige confirmação forte e período de retenção configurado.

## Critérios de aceite

O MVP é aceito quando:

- uma organização é criada e isolada;
- usuários só acessam o escopo permitido;
- leads podem percorrer o funil;
- score possui explicação;
- o vendedor recebe contexto;
- uma visita pode ser agendada;
- uma conversa pode ser assumida por humano;
- a IA não gera informação não confirmada;
- o consumo é contabilizado;
- alertas são disparados;
- billing é auditável;
- testes críticos passam;
- não há segredos no cliente nem no repositório.
