# Security Baseline Platform

## Purpose

Menetapkan baseline keamanan ChannelHub Enterprise setelah integrasi & deployment (Phase 21), sebagai fondasi hardening Phase 22.

## Scope

Platform NestJS + Next.js + PostgreSQL + Redis + BullMQ, multi-tenant SaaS, white-label ready.

## Context

Security architecture sudah didefinisikan di [docs/02-product-architecture/009-security-architecture.md](../02-product-architecture/009-security-architecture.md). Phase 22 mengoperasionalkan kontrol untuk auth, tenant, secrets, testing, dan incident response.

## Rules

- Defense in depth: edge, application, data, operations.
- Least privilege untuk user, service account, dan CI.
- Semua perubahan security-sensitive wajib [checklists/security-review.md](../../checklists/security-review.md).
- Tidak mengubah stack utama tanpa ADR.

## Technical Details

### Layer controls

| Layer | Control | Nilai baseline |
| --- | --- | --- |
| Edge | TLS, rate limit, security header | TLS 1.2+, 100 req/menit per IP per endpoint publik |
| App | AuthN/Z, validasi input, kebijakan CORS | Guard chain 4 lapis, validasi skema pada semua DTO |
| Data | Tenant scoping, enkripsi, audit log | `organization_id` wajib, AES-256-GCM untuk kredensial |
| Ops | Rotasi secret, monitoring, runbook | Rotasi kunci JWT 90 hari |

### Security header wajib

```text
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Referrer-Policy: strict-origin-when-cross-origin
Content-Security-Policy: default-src 'self'
```

CORS hanya mengizinkan origin frontend yang terdaftar per environment; API tidak memakai wildcard.

### Baseline alignment

- Authentication API: [docs/16-api-contract/005-api-authentication-standard.md](../16-api-contract/005-api-authentication-standard.md)
- Rate limit: [docs/16-api-contract/006-api-rate-limit-standard.md](../16-api-contract/006-api-rate-limit-standard.md)
- Deployment secrets: [docs/21-integration-deployment/004-environment-deployment.md](../21-integration-deployment/004-environment-deployment.md)
- ADR tenant: [adr/ADR-006-multi-tenant-isolation.md](../../adr/ADR-006-multi-tenant-isolation.md)

## Impact

- Development: micro-prompt security di [prompts/phases/phase-22-security.md](../../prompts/phases/phase-22-security.md)
- Testing: [005-security-testing-and-audit.md](./005-security-testing-and-audit.md)
- Deploy: stage security scan pada [docs/21-integration-deployment/003-ci-cd-pipeline.md](../21-integration-deployment/003-ci-cd-pipeline.md)
- Maintenance: [006-incident-response-runbook.md](./006-incident-response-runbook.md)

## References

- [docs/02-product-architecture/009-security-architecture.md](../02-product-architecture/009-security-architecture.md)
- [standards/security.md](../../standards/security.md)
- [diagrams/004-auth-flow.md](../../diagrams/004-auth-flow.md)
- [adr/ADR-006-multi-tenant-isolation.md](../../adr/ADR-006-multi-tenant-isolation.md)
