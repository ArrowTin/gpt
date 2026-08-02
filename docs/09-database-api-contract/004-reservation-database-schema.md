# ChannelHub Reservation Database Schema Blueprint

> **Status: konseptual.** Dokumen ini menjelaskan pemikiran domain pada Phase 09.
> Sumber kebenaran implementasi adalah contract artifact:
> [contracts/openapi/channelhub.v1.yaml](../../contracts/openapi/channelhub.v1.yaml),
> [docs/15-database-implementation/009-canonical-erd.md](../15-database-implementation/009-canonical-erd.md),
> [docs/15-database-implementation/010-postgresql-ddl-reference.md](../15-database-implementation/010-postgresql-ddl-reference.md).
> Bila terjadi perbedaan, contract artifact yang berlaku.

## Purpose

Mendefinisikan schema transaksi reservasi.

---

# Tables

## reservations

Menyimpan transaksi booking.

Fields:

- id.
- property_id.
- guest_id.
- status.
- check_in.
- check_out.

---

## guests

Menyimpan informasi tamu.

---

## reservation_events

Menyimpan perubahan status transaksi.

---

## booking_sources

Menyimpan asal booking.

Contoh:

- Direct.
- OTA.

---

# Transaction Flow

```
Booking Request
      |
Availability Check
      |
Reservation
      |
Event
```

---

# Validation

Schema siap jika:

- Status lifecycle tercatat.
- Transaction aman.
- Event dapat dipublikasikan.
