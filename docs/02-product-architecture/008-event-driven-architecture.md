# ChannelHub Event Driven Architecture

## Purpose

Mendefinisikan komunikasi asynchronous antar service menggunakan event.

---

# Event Flow

```
Service
  |
  v
Event Bus
  |
  +--> Consumer Services
```

---

# Main Events

```
ReservationCreated
ReservationCancelled
PaymentCompleted
SubscriptionChanged
OTAInventoryUpdated
```

---

# Benefits

- Loose coupling.
- Better scalability.
- Reliable processing.
- Background processing.

---

# Queue Requirement

Setiap consumer harus memiliki:

- Retry.
- Dead letter queue.
- Error tracking.
- Processing status.
