# Security Testing and Audit

## Purpose

Mendefinisikan pengujian keamanan dan auditability untuk ChannelHub.

## Scope

Static analysis, dependency scan, auth/tenant tests, audit log verification.

## Context

Testing strategy: docs/02-product-architecture/010-testing-strategy.md. CI: docs/21-integration-deployment/003-ci-cd-pipeline.md.

## Rules

- Security scan stage mandatory in CI (fail on critical unless waived with ADR).
- Auth regression suite for login, refresh, revoke.
- Tenant isolation negative tests mandatory for CRUD modules.
- Audit log for privileged actions (role change, tenant config, OTA credential view).

## Technical Details

### Test categories

| Category | Examples |
| --- | --- |
| Unit | Permission guard, password policy validator |
| Integration | Cross-tenant 403, rate limit lockout |
| CI | SAST, dependency CVE, container scan |

### Audit fields

- actor_id, tenant_id, action, resource, ip, correlation_id, timestamp

## Impact

- prompts/lifecycle/02-testing-increment.md
- checklists/testing-release.md + security-review.md

## References

- docs/03-product-specification/010-audit-log-system.md
- docs/22-security/003-authorization-tenant-isolation.md
- standards/testing.md
