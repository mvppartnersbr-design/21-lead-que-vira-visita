# Desenvolvimento local

## Pré-requisitos

- Node.js LTS;
- pnpm;
- Docker e Docker Compose;
- Git;
- acesso às credenciais apenas para integrações reais.

## Instalação

```bash
git clone <repository-url>
cd lead-que-vira-visita
pnpm install
cp .env.example .env.local
docker compose up -d postgres redis
pnpm db:migrate
pnpm db:seed
pnpm dev
```

O modo demo deve funcionar sem WhatsApp, IA ou billing reais.

## Variáveis de ambiente

```text
NODE_ENV=development
APP_URL=http://localhost:3000
DATABASE_URL=postgresql://...
REDIS_URL=redis://...
AUTH_SECRET=...
STORAGE_ENDPOINT=...
STORAGE_BUCKET=...
STORAGE_ACCESS_KEY=...
STORAGE_SECRET_KEY=...
AI_PROVIDER=mock
WHATSAPP_PROVIDER=mock
BILLING_PROVIDER=mock
ENCRYPTION_KEY=...
```

O `.env.example` deve conter nomes e exemplos não funcionais. Segredos reais ficam fora do repositório.

## Scripts

```bash
pnpm dev
pnpm build
pnpm lint
pnpm typecheck
pnpm test
pnpm test:e2e
pnpm db:migrate
pnpm db:seed
pnpm format
```

## Convenções

Usar TypeScript strict, funções pequenas, nomes explícitos, schemas Zod nas fronteiras, serviços de aplicação para casos de uso e adapters para integrações. Não colocar acesso ao banco em componentes React.

Commits devem ser pequenos e descritivos. Pull requests devem informar intenção, alterações, testes, migrações, riscos e screenshots quando houver mudança de interface.

## Definition of Done

Uma tarefa está pronta quando possui implementação, validação, testes relevantes, tratamento de erro, atualização documental e revisão de segurança quando aplicável.

## Dados demo

Nunca usar dados reais. Seeds devem criar uma organização fictícia, veículos, leads, conversas, tarefas, visitas, plano Comercial e consumo próximo do limite para testar billing.
