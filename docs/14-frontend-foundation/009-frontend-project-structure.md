# ChannelHub Frontend Project Structure

## Purpose

Menetapkan **struktur file dan folder konkret** aplikasi Next.js ChannelHub beserta pemetaan route ke fitur, sehingga generator kode menempatkan file pada lokasi yang sama setiap increment.

## Scope

`apps/frontend` pada monorepo ChannelHub. Mencakup App Router, layer API client, state, dan komponen.

## Context

Standar arsitektur: [001-nextjs-architecture-standard.md](./001-nextjs-architecture-standard.md). Routing: [007-frontend-routing-standard.md](./007-frontend-routing-standard.md). API client: [005-api-client-architecture.md](./005-api-client-architecture.md).

## Rules

- Next.js App Router; server component sebagai default, `"use client"` hanya bila butuh interaksi.
- Semua pemanggilan API melalui `src/lib/api`; komponen tidak memanggil `fetch` langsung.
- Tipe request/response digenerate dari [contracts/openapi/channelhub.v1.yaml](../../contracts/openapi/channelhub.v1.yaml) ke `src/lib/api/generated`; file generated tidak diedit manual.
- Menu, permission, dan feature dibaca dari API (`/menus`, `/users/me`), tidak boleh hardcode.
- Kode fitur berada di `src/features/<domain>`; `src/components` hanya berisi komponen reusable lintas fitur.

## Technical Details

### Struktur aplikasi

```text
apps/frontend/
├── src/
│   ├── app/
│   │   ├── layout.tsx
│   │   ├── (public)/
│   │   │   ├── page.tsx                  # landing page dari CMS config
│   │   │   ├── login/page.tsx
│   │   │   └── pricing/page.tsx
│   │   ├── (app)/
│   │   │   ├── layout.tsx                # shell: sidebar dinamis + tenant switcher
│   │   │   ├── dashboard/page.tsx
│   │   │   ├── properties/
│   │   │   │   ├── page.tsx
│   │   │   │   └── [propertyId]/
│   │   │   │       ├── page.tsx
│   │   │   │       ├── room-types/page.tsx
│   │   │   │       ├── inventory/page.tsx
│   │   │   │       └── rates/page.tsx
│   │   │   ├── reservations/
│   │   │   │   ├── page.tsx
│   │   │   │   └── [reservationId]/page.tsx
│   │   │   ├── channels/
│   │   │   │   ├── page.tsx
│   │   │   │   └── [connectionId]/mappings/page.tsx
│   │   │   ├── billing/
│   │   │   │   ├── page.tsx
│   │   │   │   └── invoices/[invoiceId]/page.tsx
│   │   │   ├── settings/
│   │   │   │   ├── organization/page.tsx
│   │   │   │   ├── users/page.tsx
│   │   │   │   └── roles/page.tsx
│   │   │   └── notifications/page.tsx
│   │   ├── (admin)/                      # konsol super admin
│   │   │   ├── tenants/page.tsx
│   │   │   ├── plans/page.tsx
│   │   │   ├── feature-flags/page.tsx
│   │   │   └── audit-logs/page.tsx
│   │   └── api/                          # route handler tipis (proxy cookie/session)
│   ├── features/
│   │   ├── auth/
│   │   ├── properties/
│   │   ├── inventory/
│   │   ├── reservations/
│   │   ├── channels/
│   │   ├── billing/
│   │   ├── settings/
│   │   └── notifications/
│   ├── components/
│   │   ├── ui/                           # design system primitif
│   │   ├── layout/                       # AppShell, Sidebar, Topbar
│   │   └── data/                         # DataTable, DateRangePicker, EmptyState
│   ├── lib/
│   │   ├── api/
│   │   │   ├── client.ts                 # fetch wrapper: token, X-Tenant-Id, envelope
│   │   │   ├── errors.ts                 # mapping error.code → pesan UI
│   │   │   └── generated/                # tipe & operasi dari OpenAPI
│   │   ├── auth/                         # session, refresh, guard route
│   │   ├── permissions/                  # can(), useEntitlement()
│   │   └── format/                       # tanggal, mata uang, timezone properti
│   ├── stores/                           # state global minimal (tenant aktif, UI)
│   ├── hooks/
│   ├── styles/
│   └── types/
├── public/
├── tests/
│   ├── unit/
│   └── e2e/
├── Dockerfile
├── next.config.ts
├── tsconfig.json
└── package.json
```

### Isi wajib satu feature

```text
features/reservations/
├── components/
│   ├── ReservationTable.tsx
│   ├── ReservationStatusBadge.tsx
│   └── CreateReservationForm.tsx
├── hooks/
│   ├── useReservations.ts               # query list + filter
│   └── useUpdateReservationStatus.ts    # mutation + invalidasi cache
├── api/
│   └── reservations.api.ts              # pemanggilan endpoint via lib/api/client
├── schemas/
│   └── reservation.schema.ts            # validasi form, selaras DTO OpenAPI
└── __tests__/
    └── ReservationTable.test.tsx
```

### Peta route ke API

| Route | Endpoint utama | Permission |
| --- | --- | --- |
| `/dashboard` | `/reservations`, `/availability` | `RESERVATION_READ` |
| `/properties` | `/properties` | `PROPERTY_READ` |
| `/properties/[id]/inventory` | `GET/PUT /properties/{id}/inventory` | `INVENTORY_READ`, `INVENTORY_UPDATE` |
| `/properties/[id]/rates` | `GET/PUT /properties/{id}/rates` | `RATE_READ`, `RATE_UPDATE` |
| `/reservations` | `/reservations` | `RESERVATION_READ` |
| `/reservations/[id]` | `/reservations/{id}`, `/reservations/{id}/status` | `RESERVATION_READ`, `RESERVATION_UPDATE` |
| `/channels` | `/channel-connections`, `/sync-jobs` | `CHANNEL_READ` |
| `/channels/[id]/mappings` | `PUT /channel-connections/{id}/mappings` | `CHANNEL_UPDATE` |
| `/billing` | `/invoices`, `/wallet` | `BILLING_READ` |
| `/settings/users` | `/users`, `/roles` | `USER_READ` |
| `/settings/roles` | `/roles`, `/permissions` | `ROLE_READ` |
| `/notifications` | `/notifications` | authenticated |
| `(admin)/*` | endpoint dengan scope super admin | `isSuperAdmin` |

Item sidebar tidak ditulis di kode; `AppShell` merender hasil `GET /menus`.

### Konvensi penamaan

| Jenis | Pola |
| --- | --- |
| Komponen | `PascalCase.tsx` |
| Hook | `useSesuatu.ts` |
| Modul API fitur | `<domain>.api.ts` |
| Skema form | `<entity>.schema.ts` |
| Test | `<Nama>.test.tsx` |
| Route folder | `kebab-case` |

### Urutan bootstrap increment

```text
1. layout + design system primitif
2. auth (login, refresh, proteksi route)
3. AppShell + menu dinamis + tenant switcher
4. properties + room types
5. inventory & rate calendar
6. reservations
7. channels & sync monitoring
8. billing, settings, notifications
9. konsol super admin
```

## Impact

- [docs/20-frontend-application/](../20-frontend-application/) — modul UI mengikuti struktur ini.
- [docs/21-integration-deployment/001-frontend-backend-integration.md](../21-integration-deployment/001-frontend-backend-integration.md) — konfigurasi base URL dan header.
- [checklists/development-ready.md](../../checklists/development-ready.md) — kesiapan frontend diperiksa terhadap struktur ini.

## References

- [002-frontend-folder-design.md](./002-frontend-folder-design.md)
- [docs/16-api-contract/009-api-endpoint-specification.md](../16-api-contract/009-api-endpoint-specification.md)
- [docs/03-product-specification/002-role-permission-system.md](../03-product-specification/002-role-permission-system.md)
- [adr/ADR-002-nextjs-frontend-framework.md](../../adr/ADR-002-nextjs-frontend-framework.md)
