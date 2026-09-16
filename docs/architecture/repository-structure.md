# Estrutura do repositório de código

```text
/apps
  /web
    /src/app
    /src/components
    /src/features
    /src/server
    /src/styles
  /worker
    /src/jobs
    /src/consumers
/packages
  /db
    /schema
    /migrations
    /repositories
  /shared
    /schemas
    /types
    /constants
  /ui
    /components
    /tokens
  /integrations
    /whatsapp
    /ai
    /billing
    /inventory
    /calendar
  /billing
    /domain
    /application
    /infrastructure
  /config
    /env
/docs
/adr
/scripts
/docker
```

## Regras de dependência

- `domain` não importa provider externo.
- `application` depende de interfaces e políticas.
- `infrastructure` implementa interfaces.
- `web` chama casos de uso e não acessa tabelas diretamente.
- `worker` chama casos de uso idempotentes.
- `shared` não deve conter lógica específica de interface.
- `billing` não deve depender de componente React.

## Nomenclatura

Usar nomes de domínio em inglês no código para consistência técnica e textos em português na interface. Schemas de entrada e saída devem possuir nomes explícitos. Evitar abreviações ambíguas.
