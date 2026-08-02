# Phase 15 — Database Implementation

## Standards

`standards/database.md` · `adr/ADR-003-postgresql-primary-database.md`

## Micro-prompts

| MP | Doc | Blueprint goal | App repo goal |
| --- | --- | --- | --- |
| MP-001 | docs/15-database-implementation/001-postgresql-schema-standard.md | Schema rules | Schema review |
| MP-002 | docs/15-database-implementation/002-domain-entity-design.md | Entity field & invariant | One entity + business rule |
| MP-003 | docs/15-database-implementation/003-database-migration-strategy.md | Migration tool | One migration |
| MP-004 | docs/15-database-implementation/004-data-seed-strategy.md | Seeds | Seed file |
| MP-005 | docs/15-database-implementation/005-database-indexing-strategy.md | Indexes | Index migration |
| MP-006 | docs/15-database-implementation/006-relationship-mapping-standard.md | Relations | Relation map |
| MP-007 | docs/15-database-implementation/007-transaction-consistency-pattern.md | Transactions | Service txn |
| MP-008 | docs/15-database-implementation/008-database-backup-recovery-standard.md | maintenance checklist | Backup doc/job |
| MP-009 | docs/15-database-implementation/009-canonical-erd.md | Jaga daftar tabel & relasi kanonik | Verifikasi model vs ERD |
| MP-010 | docs/15-database-implementation/010-postgresql-ddl-reference.md | Sync DDL saat tabel berubah | Migration sesuai DDL |

**Validation:** testing-release · maintenance-operations

## Rule

Nama tabel, enum, dan relasi hanya boleh berubah lewat MP-009; MP lain mengikutinya.
