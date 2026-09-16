# Segurança e privacidade

## Objetivos

O sistema processa nomes, telefones, conversas, preferências de compra, dados de veículos e credenciais de integração. Os objetivos são confidencialidade, integridade, disponibilidade, rastreabilidade e controle do uso de dados.

## Threat model resumido

| Ameaça | Controle |
|---|---|
| Vazamento entre tenants | Filtro obrigatório, autorização server-side, testes de isolamento e RLS quando adequado |
| Token exposto | Secret manager, redaction, sem secrets no frontend ou Git |
| Webhook falsificado | Assinatura, timestamp, idempotência e allowlist |
| Prompt injection | Conteúdo do lead tratado como dado, ferramentas allowlisted e prompt de sistema imutável |
| IA inventando estoque | Fonte aprovada, validação de status e handoff |
| Cobrança duplicada | Idempotency key, constraints e transação |
| Abuso de API | Rate limiting, limites por tenant e monitoramento |
| Upload malicioso | MIME real, tamanho, antivírus quando aplicável e storage privado |
| Acesso indevido de suporte | Impersonation temporária, motivo, expiração e auditoria |
| Indisponibilidade | Fila, retry, health check, backup e procedimento de restauração |

## Controle de acesso

A autorização ocorre no backend. O contexto do tenant é derivado da sessão e não do input do usuário. O sistema deve aplicar menor privilégio por papel e por equipe.

## Segredos

Use `.env.example` apenas com nomes. Segredos reais devem ficar em secret manager ou variáveis protegidas do ambiente. Nunca registrar tokens em logs. Credenciais de integração devem ser criptografadas em repouso e descriptografadas apenas no serviço que precisa usá-las.

## Dados de IA

Enviar ao modelo somente dados necessários para a tarefa. Redigir CPF, documentos e dados financeiros não necessários. Armazenar prompt, resposta, modelo e confiança somente conforme a política de retenção. Respostas da IA devem ser marcadas como sugestão ou geração automática.

## Privacidade operacional

O sistema deve suportar opt-out, exclusão, correção e exportação conforme a política da organização. O fluxo de exclusão deve registrar a solicitação e remover ou anonimizar dados conforme a política aprovada.

## WhatsApp

Usar apenas API oficial ou BSP oficial. Templates precisam ser aprovados pelo provedor quando aplicável. O sistema deve respeitar janela de atendimento, consentimento, opt-out e status de entrega.

## Auditoria

Auditar login, convite, alteração de papel, leitura de dados sensíveis, exportação, alteração de plano, uso de suporte, envio de mensagem, handoff, mudança de score, mudança de status, alteração de estoque e alterações de integração.

## Resposta a incidentes

1. Identificar e classificar o incidente.
2. Conter acesso ou credencial comprometida.
3. Preservar evidências mínimas.
4. Avaliar escopo e organizações afetadas.
5. Corrigir a causa.
6. Rotacionar segredos.
7. Restaurar serviço de maneira controlada.
8. Registrar timeline e ações.
9. Comunicar partes responsáveis conforme política.
10. Criar ação preventiva e revisar controles.

## Checklist antes de produção

- [ ] Nenhum secret no repositório.
- [ ] Isolamento multi-tenant testado.
- [ ] Webhooks com assinatura e idempotência.
- [ ] Rate limiting ativo.
- [ ] Backups testados.
- [ ] Logs sem dados desnecessários.
- [ ] Acesso de suporte auditado.
- [ ] Opt-out testado.
- [ ] Rotação de credenciais documentada.
- [ ] Processo de exclusão e exportação definido.
