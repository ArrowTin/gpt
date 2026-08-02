# ChannelHub Engineering Standards

## Purpose

Ringkasan standar engineering yang mengikat development, testing, deployment, dan maintenance. Detail lengkap ada di `docs/`; file di folder ini adalah **index operasional** untuk manusia dan AI.

## Standard Index

| Standard | File | Primary docs |
| --- | --- | --- |
| Documentation | [documentation.md](./documentation.md) | docs/00-foundation/008-documentation-standard.md |
| API | [api.md](./api.md) | docs/16-api-contract/ |
| Database | [database.md](./database.md) | docs/15-database-implementation/ |
| Backend | [backend.md](./backend.md) | docs/13-backend-foundation/, docs/18-backend-implementation/ |
| Frontend | [frontend.md](./frontend.md) | docs/14-frontend-foundation/, docs/20-frontend-application/ |
| Security | [security.md](./security.md) | docs/22-security/, docs/02-product-architecture/009-security-architecture.md |
| Testing | [testing.md](./testing.md) | docs/02-product-architecture/010-testing-strategy.md |
| Deployment | [deployment.md](./deployment.md) | docs/21-integration-deployment/ |
| Observability | [observability.md](./observability.md) | docs/04-technical-blueprint/008-logging-monitoring-standard.md |
| AI development | [ai-development.md](./ai-development.md) | docs/05-ai-development-blueprint/ |

## Application Order

```text
Read standards/README.md
  → standards for layer you touch (api, db, …)
  → matching docs/ phase folder
  → matching ADR in adr/
  → execute one prompts/phases micro-prompt
  → validate with checklists/
```

## Non-Negotiables

- Multi-tenant: setiap request protected membawa tenant context.
- API First: kontrak di `docs/16-api-contract/` mengalahkan implementasi ad hoc.
- No secrets in git: gunakan environment & secret manager (docs/21-integration-deployment/004-environment-deployment.md).
- Observability: correlation id, structured logging, health/readiness (docs/21-integration-deployment/001-frontend-backend-integration.md).

## Sync Rule

Jika `docs/` dan `standards/` bertentangan, perbaiki keduanya dalam satu PR dokumentasi dan catat di ADR bila decision berubah.
