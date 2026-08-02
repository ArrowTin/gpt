# ChannelHub Module Execution Template

## Purpose

Template yang diisi pada setiap increment sebelum AI menulis kode, agar lingkup task jelas dan dapat diverifikasi.

## Rules

- Template diisi lengkap sebelum implementasi dimulai; bagian kontrak tidak boleh kosong.
- Satu template = satu increment. Bila daftar file melebihi satu module, pecah menjadi beberapa increment.
- Bagian "Kontrak yang dipakai" wajib menunjuk file nyata, bukan deskripsi.

## Template

```markdown
## Module
Nama module      : reservations
Fase / MP        : Phase 19 / MP-006
Repo             : apps/backend

## Kontrak yang dipakai
Endpoint         : contracts/openapi/channelhub.v1.yaml → /reservations, /reservations/{id}/status
Tabel            : reservations, reservation_rooms, reservation_events, inventory
Business rule    : docs/15-database-implementation/002-domain-entity-design.md § Reservation
Kode error       : RESERVATION_INVALID_TRANSITION, RESERVATION_INVENTORY_UNAVAILABLE

## Rencana file
Baru             : src/modules/reservations/{reservations.module.ts, ...}
Diubah           : src/app.module.ts
Migration        : 006_reservation_domain

## Perilaku yang harus benar
- Pembuatan reservasi mengurangi inventory dalam satu transaksi.
- Transisi status mengikuti state machine dan menulis reservation_events.
- Seluruh query difilter organization_id.

## Test
Unit             : state machine, kalkulasi malam menginap
Integrasi        : oversell ditolak, akses lintas tenant 404
Contract         : response cocok dengan skema OpenAPI

## Selesai bila
- [ ] Test hijau
- [ ] Tidak ada nilai bisnis hardcoded
- [ ] Kontrak tidak berubah, atau berubah beserta file kontraknya
```

## Validation Checklist

- Fungsi berjalan sesuai perilaku yang didaftarkan.
- Test tersedia dan hijau.
- Permission dan isolasi tenant diperiksa.
- Dokumentasi atau kontrak diperbarui bila perilaku berubah.

## References

- [002-vibe-code-workflow.md](./002-vibe-code-workflow.md)
- [prompts/lifecycle/01-development-increment.md](../../prompts/lifecycle/01-development-increment.md)
- [checklists/code-review.md](../../checklists/code-review.md)
