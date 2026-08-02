# Phase 19 — Backend Application

## Standards

`standards/backend.md` · `standards/api.md` · `contracts/openapi/channelhub.v1.yaml`

## Micro-prompts

| MP | Doc | Blueprint goal | App repo goal |
| --- | --- | --- | --- |
| MP-001 | docs/19-backend-application/001-nestjs-bootstrap.md | Urutan bootstrap | `main.ts`, config, health |
| MP-002 | docs/19-backend-application/002-database-integration.md | Pola akses data | Migration 001–010 + repository |
| MP-003 | docs/19-backend-application/003-auth-module-implementation.md | Alur auth | Module `auth` + guard chain |
| MP-004 | docs/19-backend-application/004-user-module-implementation.md | User & role | Module `users`, `roles` |
| MP-005 | docs/19-backend-application/005-property-module-implementation.md | Property, inventory, rate | Module `properties`, `inventory`, `rates` |
| MP-006 | docs/19-backend-application/006-reservation-module-implementation.md | Transaksi booking | Module `reservations` (anti oversell) |
| MP-007 | docs/19-backend-application/007-channel-sync-module-implementation.md | Integrasi OTA | Module `channels` + webhook |
| MP-008 | docs/19-backend-application/008-queue-worker-implementation.md | Worker | `worker.ts` + processor |

**Validation:** code-review · testing-release · security-review (tenant negative test)

## Rule

Setiap module hanya boleh membaca/menulis tabel miliknya sesuai peta pada `docs/13-backend-foundation/009-backend-project-structure.md`.
