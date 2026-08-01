# ChannelHub Event Driven Architecture Blueprint

## Purpose

Mendefinisikan komunikasi antar service berbasis event.

---

# Event Flow

```
Domain Event
      |
Event Bus
      |
Subscriber Service
      |
Processing
```

---

# Event Example

```
ReservationCreated
ReservationCancelled
SyncCompleted
PaymentCompleted
```

---

# Principle

Event system harus:

- Loose coupling.
- Reliable.
- Traceable.
- Support retry.

---

# Goal

Membangun arsitektur yang scalable untuk pertumbuhan service.
