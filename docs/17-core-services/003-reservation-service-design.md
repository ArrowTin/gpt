# ChannelHub Reservation Service Design Blueprint

## Purpose

Mendefinisikan engine pengelolaan reservasi.

---

# Service Responsibility

```
Reservation Service
        |
Booking Lifecycle
        |
Guest Transaction
```

---

# Core Capability

Reservation Service menangani:

- Reservation creation.
- Booking status.
- Modification.
- Cancellation.
- Guest information.

---

# Lifecycle

```
Created
  |
Confirmed
  |
Completed
  |
Cancelled
```

---

# Goal

Menjaga konsistensi seluruh proses booking.
