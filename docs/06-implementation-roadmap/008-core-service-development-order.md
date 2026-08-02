# ChannelHub Core Service Development Order

## Purpose

Menentukan urutan increment pembangunan agar dependency data dan kontrak tetap sehat, serta setiap langkah dapat diuji sebelum langkah berikutnya.

## Rules

- Satu increment dikerjakan pada satu waktu; tidak membangun seluruh domain sekaligus.
- Urutan mengikuti dependency foreign key pada [docs/15-database-implementation/010-postgresql-ddl-reference.md](../15-database-implementation/010-postgresql-ddl-reference.md).
- Increment berikutnya tidak dimulai sebelum test increment sebelumnya hijau.
- Setiap service memiliki kepemilikan tabel, endpoint, event, test, dan dokumentasi sendiri.

## Technical Details

### Urutan increment

| # | Increment | Migration | Endpoint | Prasyarat |
| --- | --- | --- | --- | --- |
| 1 | Fondasi: config, database, observability, health | 001 | `/health` | — |
| 2 | Cross-cutting: guard, interceptor, exception filter, envelope | — | — | 1 |
| 3 | Identity: user, role, permission, session | 002, 004 | `/auth/*`, `/users*`, `/roles`, `/permissions` | 2 |
| 4 | Organization: tenant, membership, setting | 003 | `/organizations*` | 3 |
| 5 | Property: property, room type, room | 005 | `/properties*` | 4 |
| 6 | Inventory & rate: kalender, optimistic locking | 005, 010 | `/properties/{id}/inventory`, `/properties/{id}/rates`, `/availability` | 5 |
| 7 | Reservation: lifecycle transaksional, anti oversell | 006 | `/reservations*` | 6 |
| 8 | OTA: koneksi, mapping, sync job, webhook | 007 | `/channels`, `/channel-connections*`, `/sync-jobs`, `/webhooks/ota/*` | 7 |
| 9 | Subscription & billing: plan, entitlement, invoice, wallet | 008 | `/subscription-plans`, `/subscriptions/current`, `/invoices*`, `/wallet*` | 4 |
| 10 | Notification & platform: notifikasi, menu, audit, feature flag | 009 | `/notifications*`, `/menus`, `/audit-logs` | 9 |
| 11 | Frontend: shell, auth UI, lalu feature mengikuti urutan 5–10 | — | — | 3 |
| 12 | Integrasi & deployment: compose production, CI/CD, environment | — | — | 11 |

Increment 9 hanya bergantung pada organization sehingga dapat dikerjakan paralel dengan 5–8 bila ada dua jalur kerja.

### Alasan urutan

- Identity mendahului organization karena `organization_members` merujuk `users`.
- Inventory mendahului reservation karena konfirmasi reservasi memperbarui inventory dalam satu transaksi.
- OTA mendahului notification karena event sinkronisasi menjadi salah satu pemicu notifikasi.

### Kriteria selesai per service

- Implementasi sesuai kontrak OpenAPI dan DDL.
- Test unit dan integrasi hijau, termasuk uji negatif tenant.
- Event domain diterbitkan bila perilaku mensyaratkannya.
- Monitoring dan log terstruktur aktif.

## References

- [docs/13-backend-foundation/009-backend-project-structure.md](../13-backend-foundation/009-backend-project-structure.md)
- [docs/14-frontend-foundation/009-frontend-project-structure.md](../14-frontend-foundation/009-frontend-project-structure.md)
- [docs/05-ai-development-blueprint/002-vibe-code-workflow.md](../05-ai-development-blueprint/002-vibe-code-workflow.md)
- [009-development-milestone-plan.md](./009-development-milestone-plan.md)
