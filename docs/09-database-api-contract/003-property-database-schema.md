# ChannelHub Property Database Schema Blueprint

> **Status: konseptual.** Dokumen ini menjelaskan pemikiran domain pada Phase 09.
> Sumber kebenaran implementasi adalah contract artifact:
> [contracts/openapi/channelhub.v1.yaml](../../contracts/openapi/channelhub.v1.yaml),
> [docs/15-database-implementation/009-canonical-erd.md](../15-database-implementation/009-canonical-erd.md),
> [docs/15-database-implementation/010-postgresql-ddl-reference.md](../15-database-implementation/010-postgresql-ddl-reference.md).
> Bila terjadi perbedaan, contract artifact yang berlaku.

## Purpose

Mendefinisikan penyimpanan data properti dan inventory.

---

# Tables

## properties

Data properti utama.

Fields:

- id.
- organization_id.
- name.
- address.
- status.

---

## room_types

Definisi tipe kamar.

---

## rooms

Unit kamar yang tersedia.

---

## inventory

Menyimpan jumlah ketersediaan.

---

## rate_plans

Menyimpan aturan harga.

---

# Relationship

```
Property
 |
Room Type
 |
Room
 |
Inventory
```

---

# Validation

Schema siap jika:

- Inventory dapat berubah.
- Rate dapat dikelola.
- Data siap sinkronisasi OTA.
