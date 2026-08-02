# ChannelHub Database Domain Model Blueprint

> **Status: konseptual.** Dokumen ini menjelaskan pemikiran domain pada Phase 09.
> Sumber kebenaran implementasi adalah contract artifact:
> [contracts/openapi/channelhub.v1.yaml](../../contracts/openapi/channelhub.v1.yaml),
> [docs/15-database-implementation/009-canonical-erd.md](../15-database-implementation/009-canonical-erd.md),
> [docs/15-database-implementation/010-postgresql-ddl-reference.md](../15-database-implementation/010-postgresql-ddl-reference.md).
> Bila terjadi perbedaan, contract artifact yang berlaku.

## Purpose

Mendefinisikan model data tingkat domain sebelum implementasi schema database.

---

# Domain Ownership

Setiap service memiliki ownership data masing-masing.

```
Identity
   |
Organization
   |
Property
   |
Reservation
   |
OTA
```

---

# Core Domain Entity

```
User
Organization
Property
Room
Inventory
Reservation
Channel
SyncJob
Notification
Report
```

---

# Data Principle

Database harus:

- Konsisten dengan domain.
- Mendukung transaksi.
- Memiliki audit trail.
- Mudah dimigrasikan.

---

# Design Rule

Tidak ada service yang mengubah data milik service lain secara langsung.

Komunikasi melalui:

- API contract.
- Event contract.
