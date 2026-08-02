# Database Standard (Operational)

## Purpose

PostgreSQL schema, migration, indexing, backup.

## Rules

- Schema standard: [docs/15-database-implementation/001-postgresql-schema-standard.md](../docs/15-database-implementation/001-postgresql-schema-standard.md)
- **Tabel & relasi kanonik (wajib diikuti):** [docs/15-database-implementation/009-canonical-erd.md](../docs/15-database-implementation/009-canonical-erd.md)
- **DDL referensi (wajib diikuti):** [docs/15-database-implementation/010-postgresql-ddl-reference.md](../docs/15-database-implementation/010-postgresql-ddl-reference.md)
- Entity design: [docs/15-database-implementation/002-domain-entity-design.md](../docs/15-database-implementation/002-domain-entity-design.md)
- Migrations: [docs/15-database-implementation/003-database-migration-strategy.md](../docs/15-database-implementation/003-database-migration-strategy.md)
- Indexing: [docs/15-database-implementation/005-database-indexing-strategy.md](../docs/15-database-implementation/005-database-indexing-strategy.md)
- Transactions: [docs/15-database-implementation/007-transaction-consistency-pattern.md](../docs/15-database-implementation/007-transaction-consistency-pattern.md)
- Backup: [docs/15-database-implementation/008-database-backup-recovery-standard.md](../docs/15-database-implementation/008-database-backup-recovery-standard.md)
- Tenant column wajib pada entitas tenant-scoped (ADR-006).

## ADR

- [ADR-003-postgresql-primary-database.md](../adr/ADR-003-postgresql-primary-database.md)
