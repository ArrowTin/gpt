# Architecture Decision Records (ADR)

## Purpose

Catatan resmi keputusan arsitektur ChannelHub Enterprise. Setiap ADR di folder ini harus selaras dengan `docs/02-product-architecture/`, `docs/04-technical-blueprint/`, dan `standards/`.

## Status Legend

| Status | Meaning |
| --- | --- |
| Accepted | Berlaku; implementasi wajib mengikuti |
| Proposed | Dalam review; jangan ubah arsitektur utama |
| Superseded | Digantikan ADR lain (cantumkan link) |

## Index

| ID | Title | Status | Related docs |
| --- | --- | --- | --- |
| [ADR-001](./ADR-001-nestjs-backend-framework.md) | NestJS sebagai backend framework | Accepted | docs/13-backend-foundation/ |
| [ADR-002](./ADR-002-nextjs-frontend-framework.md) | Next.js sebagai frontend framework | Accepted | docs/14-frontend-foundation/ |
| [ADR-003](./ADR-003-postgresql-primary-database.md) | PostgreSQL sebagai database utama | Accepted | docs/15-database-implementation/ |
| [ADR-004](./ADR-004-redis-cache-and-queue.md) | Redis untuk cache & queue backing | Accepted | docs/13-backend-foundation/006-redis-cache-queue-foundation.md |
| [ADR-005](./ADR-005-bullmq-job-processing.md) | BullMQ untuk background job | Accepted | docs/18-backend-implementation/008-background-job-processing.md |
| [ADR-006](./ADR-006-multi-tenant-isolation.md) | Multi-tenant dengan tenant context | Accepted | docs/02-product-architecture/002-domain-architecture.md |
| [ADR-007](./ADR-007-event-driven-integration.md) | Event-driven antar modul | Accepted | docs/17-core-services/008-event-driven-architecture.md |
| [ADR-008](./ADR-008-api-first-rest-contract.md) | API First & REST contract | Accepted | docs/16-api-contract/ |
| [ADR-009](./ADR-009-configuration-driven-platform.md) | Configuration & metadata driven | Accepted | README.md, docs/00-foundation/004-core-principles.md |
| [ADR-010](./ADR-010-monorepo-blueprint-repository.md) | Monorepo & blueprint SSOT | Accepted | docs/12-project-foundation/001-monorepo-architecture.md |

## Process

1. Buat ADR baru dari [templates/adr-template.md](../templates/adr-template.md).
2. Update index di file ini.
3. Perbarui dokumen terkait di `docs/` jika decision mengubah spesifikasi.
4. Tambahkan micro-prompt di `prompts/phases/` jika implementasi membutuhkan increment baru.

## Rule

Tidak ada perubahan arsitektur utama tanpa ADR **Accepted**.
