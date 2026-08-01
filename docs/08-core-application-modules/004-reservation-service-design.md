# ChannelHub Reservation Service Design Blueprint

## Purpose

Mendefinisikan pengelolaan reservasi sebagai core transaction domain.

---

# Responsibility

Reservation Service menangani:

- Booking.
- Reservation status.
- Guest data.
- Availability locking.
- Reservation event.

---

# Domain Flow

```
Booking Request
      |
Availability Check
      |
Reservation Creation
      |
Event Distribution
```

---

# Core Entity

```
Reservation
Guest
BookingStatus
ReservationEvent
```

---

# Integration

Berhubungan dengan:

- Property Service.
- OTA Service.
- Notification Service.

---

# Completion Criteria

- Reservation lifecycle berjalan.
- Transaction aman.
- Event tersedia untuk service lain.
