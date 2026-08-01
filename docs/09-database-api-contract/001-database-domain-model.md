# ChannelHub Database Domain Model Blueprint

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
