# ChannelHub Reservation Database Schema Blueprint

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
