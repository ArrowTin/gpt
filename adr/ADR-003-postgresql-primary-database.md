# ADR-003: PostgreSQL sebagai Database Utama

## Status

Accepted

## Date

2026-08-02

## Context

Data hospitality (property, reservation, OTA sync) membutuhkan ACID, relational integrity, dan migrasi terkontrol.

## Decision

**PostgreSQL** sebagai database sistem record; schema per domain dengan migration strategy terpusat.

## Alternatives

- MongoDB primary — ditolak untuk transactional reservation flows.
- Multi-DB per service (fase 1) — ditolak: operational overhead.

## Consequences

- Positive: selaras docs/15-database-implementation/.
- Negative: scaling write-heavy sync perlu indexing & queue.
- Mitigation: docs/15-database-implementation/005-database-indexing-strategy.md

## Implementation Notes

- Migration: docs/15-database-implementation/003-database-migration-strategy.md
- Backup: docs/15-database-implementation/008-database-backup-recovery-standard.md

## References

- docs/02-product-architecture/004-database-architecture.md
- standards/database.md
