# Prompt Registry by Phase

## Purpose

Mapping resmi: setiap file di `docs/{phase}/` punya micro-prompt (`MP-NNN`) di `prompts/phases/phase-{phase}-*.md`.

## How to use

1. Bootstrap: `prompts/000-session-bootstrap.md`
2. Open phase file below
3. Run **one** MP per AI session
4. Run lifecycle prompts 01→04 when slice touches code deploy

## Registry

| Phase | Docs folder | Prompt file | Doc count |
| --- | --- | --- | --- |
| 00 | docs/00-foundation | [phase-00-foundation.md](./phases/phase-00-foundation.md) | 8 |
| 01 | docs/01-business | [phase-01-business.md](./phases/phase-01-business.md) | 10 |
| 02 | docs/02-product-architecture | [phase-02-product-architecture.md](./phases/phase-02-product-architecture.md) | 10 |
| 03 | docs/03-product-specification | [phase-03-product-specification.md](./phases/phase-03-product-specification.md) | 10 |
| 04 | docs/04-technical-blueprint | [phase-04-technical-blueprint.md](./phases/phase-04-technical-blueprint.md) | 10 |
| 05 | docs/05-ai-development-blueprint | [phase-05-ai-development.md](./phases/phase-05-ai-development.md) | 10 |
| 06 | docs/06-implementation-roadmap | [phase-06-implementation-roadmap.md](./phases/phase-06-implementation-roadmap.md) | 10 |
| 07 | docs/07-source-code-generation | [phase-07-source-code-generation.md](./phases/phase-07-source-code-generation.md) | 10 |
| 08 | docs/08-core-application-modules | [phase-08-core-modules.md](./phases/phase-08-core-modules.md) | 10 |
| 09 | docs/09-database-api-contract | [phase-09-database-api-contract.md](./phases/phase-09-database-api-contract.md) | 10 |
| 10 | docs/10-ai-agent-orchestration | [phase-10-ai-agent-orchestration.md](./phases/phase-10-ai-agent-orchestration.md) | 10 |
| 11 | docs/11-autonomous-development-workflow | [phase-11-autonomous-workflow.md](./phases/phase-11-autonomous-workflow.md) | 8 |
| 12 | docs/12-project-foundation | [phase-12-project-foundation.md](./phases/phase-12-project-foundation.md) | 8 |
| 13 | docs/13-backend-foundation | [phase-13-backend-foundation.md](./phases/phase-13-backend-foundation.md) | 9 |
| 14 | docs/14-frontend-foundation | [phase-14-frontend-foundation.md](./phases/phase-14-frontend-foundation.md) | 9 |
| 15 | docs/15-database-implementation | [phase-15-database-implementation.md](./phases/phase-15-database-implementation.md) | 10 |
| 16 | docs/16-api-contract | [phase-16-api-contract.md](./phases/phase-16-api-contract.md) | 10 |
| 17 | docs/17-core-services | [phase-17-core-services.md](./phases/phase-17-core-services.md) | 8 |
| 18 | docs/18-backend-implementation | [phase-18-backend-implementation.md](./phases/phase-18-backend-implementation.md) | 8 |
| 19 | docs/19-backend-application | [phase-19-backend-application.md](./phases/phase-19-backend-application.md) | 8 |
| 20 | docs/20-frontend-application | [phase-20-frontend-application.md](./phases/phase-20-frontend-application.md) | 8 |
| 21 | docs/21-integration-deployment | [phase-21-integration-deployment.md](./phases/phase-21-integration-deployment.md) | 4 |
| 22 | docs/22-security | [phase-22-security.md](./phases/phase-22-security.md) | 6 |

## Micro-prompt rules

| Mode | Rule |
| --- | --- |
| Blueprint repo | MP goal = sync/review/extend **documentation** only |
| App repo | MP goal = implement **one** slice; cite same doc path as spec |

## Machine-readable contract

Spesifikasi teknis yang dipakai lintas fase:

| Artefak | Dipakai oleh |
| --- | --- |
| [contracts/openapi/channelhub.v1.yaml](../contracts/openapi/channelhub.v1.yaml) | Phase 16, 19, 20 |
| [docs/15-database-implementation/010-postgresql-ddl-reference.md](../docs/15-database-implementation/010-postgresql-ddl-reference.md) | Phase 15, 19 |
| [docs/13-backend-foundation/009-backend-project-structure.md](../docs/13-backend-foundation/009-backend-project-structure.md) | Phase 18, 19 |
| [docs/14-frontend-foundation/009-frontend-project-structure.md](../docs/14-frontend-foundation/009-frontend-project-structure.md) | Phase 20 |

## Cross-links

- Standards: `standards/README.md`
- ADR: `adr/README.md`
- Checklists: `checklists/README.md`
