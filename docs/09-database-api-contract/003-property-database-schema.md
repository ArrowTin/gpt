# ChannelHub Property Database Schema Blueprint

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
