# Authorization & Tenant Isolation

## Purpose

Memastikan RBAC dan isolasi tenant tidak dapat dilanggar di API, worker, dan query database.

## Scope

Role, permission, feature entitlement, tenant context propagation.

## Context

Product spec: docs/03-product-specification/002-role-permission-system.md. ADR: adr/ADR-006-multi-tenant-isolation.md.

## Rules

- Setiap protected handler validates tenant membership before data access.
- Permission check at gateway/controller layer; domain layer assumes tenant id passed.
- Background jobs carry tenant id in job payload; workers reject missing tenant.
- Cross-tenant admin (super admin) explicit break-glass role with audit.

## Technical Details

### Header & context

- `X-Tenant-Id` must match token tenant unless super admin scope.
- Correlation id preserved: docs/21-integration-deployment/001-frontend-backend-integration.md

### Data access

- ORM/query filters mandatory on tenant-owned tables.
- Integration tests include negative case: tenant A cannot read tenant B.

## Impact

- standards/security.md
- checklists/testing-release.md (tenant negative tests)

## References

- adr/ADR-006-multi-tenant-isolation.md
- docs/13-backend-foundation/004-authentication-authorization-foundation.md
- docs/22-security/002-authentication-hardening.md
