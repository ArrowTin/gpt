# ChannelHub Backend Project Structure

## Purpose

Menetapkan **struktur file dan folder konkret** aplikasi backend NestJS, termasuk file wajib setiap module, sehingga generator kode menghasilkan susunan yang sama pada setiap increment.

## Scope

`apps/backend` pada monorepo ChannelHub ([docs/12-project-foundation/001-monorepo-architecture.md](../12-project-foundation/001-monorepo-architecture.md)). Worker BullMQ berada pada aplikasi yang sama dengan entrypoint terpisah.

## Context

Arsitektur NestJS: [001-nestjs-architecture-standard.md](./001-nestjs-architecture-standard.md). Module boundary: [002-backend-module-design.md](./002-backend-module-design.md). Domain: [docs/15-database-implementation/002-domain-entity-design.md](../15-database-implementation/002-domain-entity-design.md).

## Rules

- Satu domain = satu module folder; nama folder memakai bentuk jamak sesuai domain (`properties`, `reservations`).
- Module hanya boleh mengimpor module lain melalui service publiknya, tidak melalui file internal.
- Controller tipis: validasi + delegasi. Aturan bisnis berada di `*.service.ts` atau `domain/`.
- Akses database hanya melalui repository di dalam module pemilik data.
- Kode lintas module yang generik ditempatkan di `common/`, kontrak lintas aplikasi di `packages/`.
- Setiap module wajib memiliki test unit service dan test e2e controller.

## Technical Details

### Struktur aplikasi

```text
apps/backend/
├── src/
│   ├── main.ts                     # bootstrap HTTP
│   ├── worker.ts                   # bootstrap worker BullMQ
│   ├── app.module.ts
│   ├── config/
│   │   ├── configuration.ts        # loader env → typed config
│   │   ├── env.validation.ts       # skema validasi env (fail fast)
│   │   └── config.module.ts
│   ├── common/
│   │   ├── decorators/             # @CurrentUser, @Permissions, @TenantId
│   │   ├── guards/                 # JwtAuthGuard, TenantContextGuard, PermissionGuard, EntitlementGuard
│   │   ├── interceptors/           # ResponseEnvelopeInterceptor, LoggingInterceptor
│   │   ├── filters/                # GlobalExceptionFilter
│   │   ├── pipes/                  # ZodValidationPipe
│   │   ├── errors/                 # AppError + kode dari error catalog
│   │   └── pagination/
│   ├── database/
│   │   ├── database.module.ts
│   │   ├── migrations/             # 001_extensions_and_enums, dst
│   │   ├── seeds/
│   │   └── entities/               # definisi ORM per tabel kanonik
│   ├── queue/
│   │   ├── queue.module.ts
│   │   ├── queues.ts               # nama queue: sync, notification, billing
│   │   └── processors/
│   ├── observability/
│   │   ├── logger.module.ts        # structured log + correlationId
│   │   ├── metrics.module.ts
│   │   └── health.controller.ts    # GET /health
│   └── modules/
│       ├── auth/
│       ├── users/
│       ├── organizations/
│       ├── roles/
│       ├── properties/
│       ├── inventory/
│       ├── rates/
│       ├── reservations/
│       ├── channels/
│       ├── subscriptions/
│       ├── billing/
│       ├── notifications/
│       └── platform/               # menus, feature flags, audit log, system config
├── test/
│   └── e2e/
├── Dockerfile
├── nest-cli.json
├── tsconfig.json
└── package.json
```

### Isi wajib satu module

```text
modules/reservations/
├── reservations.module.ts
├── reservations.controller.ts       # hanya routing + guard + DTO
├── reservations.service.ts          # orkestrasi use case
├── domain/
│   ├── reservation.entity.ts        # invariant & transisi status
│   └── reservation-status.ts        # state machine dari endpoint spec
├── repositories/
│   └── reservations.repository.ts   # query, selalu difilter organizationId
├── dto/
│   ├── create-reservation.dto.ts
│   ├── update-reservation-status.dto.ts
│   └── reservation.response.ts
├── events/
│   └── reservation.events.ts        # ReservationCreated/Updated/Cancelled
├── processors/
│   └── reservation-sync.processor.ts
└── __tests__/
    ├── reservations.service.spec.ts
    └── reservations.controller.e2e-spec.ts
```

### Peta module ke kontrak

| Module | Endpoint utama | Tabel yang dimiliki |
| --- | --- | --- |
| `auth` | `/auth/*` | `sessions` |
| `users` | `/users*` | `users`, `user_roles` |
| `organizations` | `/organizations*` | `organizations`, `organization_members`, `organization_settings` |
| `roles` | `/roles`, `/permissions` | `roles`, `permissions`, `role_permissions` |
| `properties` | `/properties*` | `properties`, `room_types`, `rooms` |
| `inventory` | `/properties/{id}/inventory`, `/availability` | `inventory` |
| `rates` | `/properties/{id}/rates` | `rate_plans`, `rate_calendar` |
| `reservations` | `/reservations*` | `reservations`, `reservation_rooms`, `reservation_events`, `guests` |
| `channels` | `/channels*`, `/sync-jobs`, `/webhooks/ota/*` | `channel_connections`, `channel_mappings`, `sync_jobs`, `sync_logs`, `webhook_events` |
| `subscriptions` | `/subscription-plans`, `/subscriptions/current` | `subscription_plans`, `subscriptions`, `feature_entitlements` |
| `billing` | `/invoices*`, `/wallet*` | `invoices`, `invoice_items`, `payments`, `credit_wallets`, `credit_transactions` |
| `notifications` | `/notifications*` | `notifications`, `notification_templates` |
| `platform` | `/menus`, `/audit-logs`, `/health` | `menus`, `feature_flags`, `system_configurations`, `audit_logs` |

### Konvensi penamaan file

| Jenis | Pola |
| --- | --- |
| Module | `<domain>.module.ts` |
| Controller | `<domain>.controller.ts` |
| Service | `<domain>.service.ts` |
| Repository | `<domain>.repository.ts` |
| DTO request | `<action>-<entity>.dto.ts` |
| DTO response | `<entity>.response.ts` |
| Processor | `<domain>-<job>.processor.ts` |
| Test unit | `<file>.spec.ts` |
| Test e2e | `<domain>.controller.e2e-spec.ts` |

### Urutan bootstrap increment

```text
1. config + database + observability
2. common (guard, interceptor, filter, error)
3. auth + users + organizations + roles
4. properties + inventory + rates
5. reservations
6. channels + queue processor
7. subscriptions + billing
8. notifications + platform
```

Urutan ini adalah urutan micro-prompt yang dipakai vibe code ([prompts/README.md](../../prompts/README.md)); setiap langkah selesai berarti test hijau sebelum lanjut.

## Impact

- [docs/18-backend-implementation/](../18-backend-implementation/) — pola implementasi mengikuti struktur ini.
- [docs/19-backend-application/](../19-backend-application/) — module aplikasi memakai peta di atas.
- [checklists/development-ready.md](../../checklists/development-ready.md) — kesiapan diperiksa terhadap struktur ini.

## References

- [docs/12-project-foundation/002-folder-structure-standard.md](../12-project-foundation/002-folder-structure-standard.md)
- [docs/16-api-contract/009-api-endpoint-specification.md](../16-api-contract/009-api-endpoint-specification.md)
- [docs/15-database-implementation/010-postgresql-ddl-reference.md](../15-database-implementation/010-postgresql-ddl-reference.md)
- [adr/ADR-001-nestjs-backend-framework.md](../../adr/ADR-001-nestjs-backend-framework.md)
