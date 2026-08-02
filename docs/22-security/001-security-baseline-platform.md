# Security Baseline Platform

## Purpose

Menetapkan baseline keamanan ChannelHub Enterprise setelah integrasi & deployment (Phase 21), sebagai fondasi hardening Phase 22.

## Scope

Platform NestJS + Next.js + PostgreSQL + Redis + BullMQ, multi-tenant SaaS, white-label ready.

## Context

Security architecture sudah didefinisikan di docs/02-product-architecture/009-security-architecture.md. Phase 22 operationalizes controls untuk auth, tenant, secrets, testing, dan incident response.

## Rules

- Defense in depth: edge, application, data, operations.
- Least privilege untuk user, service account, dan CI.
- Semua perubahan security-sensitive wajib [checklists/security-review.md](../../checklists/security-review.md).
- Tidak mengubah stack utama tanpa ADR.

## Technical Details

### Layer controls

| Layer | Control |
| --- | --- |
| Edge | TLS, WAF/rate limit, security headers |
| App | AuthN/Z, input validation, CSRF policy for web |
| Data | Tenant scoping, encryption, audit log |
| Ops | Secret rotation, monitoring, incident runbook |

### Baseline alignment

- Authentication API: docs/16-api-contract/005-api-authentication-standard.md
- Deployment secrets: docs/21-integration-deployment/004-environment-deployment.md
- ADR tenant: adr/ADR-006-multi-tenant-isolation.md

## Impact

- Development: micro-prompt security di prompts/phases/phase-22-security.md
- Testing: docs/22-security/005-security-testing-and-audit.md
- Deploy: tambah security scan stage (already in Phase 21 CI doc)
- Maintenance: docs/22-security/006-incident-response-runbook.md

## References

- docs/02-product-architecture/009-security-architecture.md
- standards/security.md
- diagrams/004-auth-flow.md
- adr/ADR-006-multi-tenant-isolation.md
