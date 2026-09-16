# Contribuindo

## Antes de começar

Leia o README, a visão do produto, a arquitetura, o design system e as decisões arquiteturais. Se a mudança alterar billing, segurança, dados ou integração, atualize a documentação correspondente.

## Branches

Usar nomes descritivos:

```text
feat/leads-score
fix/usage-idempotency
docs/billing-rules
refactor/provider-interface
```

## Pull requests

Toda pull request deve conter contexto, problema, solução, testes executados, migrações, riscos, impacto em segurança e screenshot quando alterar interface.

## Checklist

- [ ] Typecheck executado.
- [ ] Lint executado.
- [ ] Testes relevantes executados.
- [ ] Autorização revisada.
- [ ] Logs não expõem dados sensíveis.
- [ ] Dados multi-tenant estão isolados.
- [ ] Documentação atualizada.
- [ ] Eventos e migrações são compatíveis.
- [ ] Provider mock permanece funcionando.

## Commits

Preferir mensagens no formato Conventional Commits:

```text
feat: add lead qualification score
fix: prevent duplicate usage events
docs: describe billing thresholds
```

## Regra de produto

Não introduzir uma automação que possa inventar estoque, preço, financiamento, troca ou disponibilidade sem fonte confirmada e handoff humano.
