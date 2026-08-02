# Phase 21 — Integration & Deployment

## Standards

`standards/deployment.md` · `standards/observability.md`

## Micro-prompts

| MP | Doc | Blueprint goal | App repo goal |
| --- | --- | --- | --- |
| MP-001 | docs/21-integration-deployment/001-frontend-backend-integration.md | Alur integrasi & error handling | Base URL, header, retry client |
| MP-002 | docs/21-integration-deployment/002-docker-compose-production.md | Blueprint Docker production | `docker-compose.prod.yml` |
| MP-003 | docs/21-integration-deployment/003-ci-cd-pipeline.md | Workflow CI/CD | Pipeline build–test–scan–deploy |
| MP-004 | docs/21-integration-deployment/004-environment-deployment.md | Matriks environment | `.env` template + secret injection |

**Validation:** deployment-production · security-review

## Rule

Secret tidak pernah masuk repository; ikuti `docs/22-security/004-secrets-and-credential-management.md`.
