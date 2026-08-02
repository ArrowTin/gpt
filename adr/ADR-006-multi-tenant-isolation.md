# ADR-006: Multi-Tenant Isolation dengan Tenant Context

## Status

Accepted

## Date

2026-08-02

## Context

ChannelHub SaaS multi-subscriber dengan white-label; data property dan reservation harus terisolasi.

## Decision

Setiap request authenticated membawa **tenant context** (`X-Tenant-Id` + validasi server-side). Row-level scoping by `tenant_id` pada entitas tenant-owned.

## Alternatives

- Database per tenant — ditolak untuk fase awal (cost).
- Shared DB tanpa enforcement — ditolak: risiko kebocoran data.

## Consequences

- Positive: selaras API auth standard docs/16-api-contract/005-api-authentication-standard.md
- Negative: semua query wajib filter tenant.

## References

- docs/02-product-architecture/002-domain-architecture.md
- docs/22-security/003-authorization-tenant-isolation.md
