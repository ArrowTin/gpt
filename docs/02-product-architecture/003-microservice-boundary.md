# ChannelHub Microservice Boundary

## Purpose

Dokumen ini mendefinisikan batas tanggung jawab setiap microservice ChannelHub agar sistem mudah dikembangkan, di-scale, dimonitor, dan dipelihara.

---

# 1. Microservice Principle

Setiap service harus memiliki:

- Business responsibility yang jelas.
- Database ownership sendiri.
- API contract sendiri.
- Logging sendiri.
- Monitoring sendiri.
- Testing sendiri.

Service tidak boleh mengakses database service lain secara langsung.

---

# 2. Identity Service

Responsibility:

- Authentication.
- User account.
- Session management.
- Security policy.

Database ownership:

```
users
sessions
security_policy
```

---

# 3. Organization Service

Responsibility:

- Tenant management.
- Organization profile.
- Membership.

Database ownership:

```
organizations
memberships
organization_settings
```

---

# 4. Property Service

Responsibility:

- Property management.
- Room management.
- Facility.
- Property configuration.

Database ownership:

```
properties
rooms
facilities
```

---

# 5. Inventory Service

Responsibility:

- Availability.
- Stock room.
- Allocation.
- Inventory rules.

Database ownership:

```
inventory
availability
```

---

# 6. Reservation Service

Responsibility:

- Reservation lifecycle.
- Booking state.
- Cancellation.
- Modification.

Events:

```
ReservationCreated
ReservationUpdated
ReservationCancelled
```

---

# 7. OTA Service

Responsibility:

- OTA connector.
- Authentication OTA.
- Synchronization.
- Webhook processing.
- Mapping.

Connector:

```
booking-com
agoda
traveloka
airbnb
expedia
tiket-com
```

---

# 8. Subscription Service

Responsibility:

- Plan management.
- Subscription lifecycle.
- Feature entitlement.
- Usage quota.

---

# 9. Billing Service

Responsibility:

- Invoice.
- Billing record.
- Payment history.
- Transaction ledger.

---

# 10. Notification Service

Responsibility:

- Email.
- Push notification.
- Alert delivery.

---

# 11. Reporting Service

Responsibility:

- Analytics.
- Dashboard aggregation.
- Business report.

Reporting menggunakan data melalui API atau event, bukan akses database langsung.

---

# 12. Platform Management Service

Internal ChannelHub service.

Responsibility:

- System configuration.
- Feature flag.
- Audit log.
- Monitoring metadata.

---

# 13. Communication Pattern

Synchronous:

```
REST API
```

Asynchronous:

```
Event Bus
Message Queue
```

Contoh:

```
Reservation Service
        |
        v
Event Bus
        |
        +--> Notification
        +--> Reporting
        +--> OTA Sync
```

---

# 14. Scaling Strategy

Service dengan beban tinggi dapat di-scale independen.

Contoh:

```
OTA Service
  |
  +-- Instance 1
  +-- Instance 2
  +-- Instance 3
```

---

# 15. Failure Isolation

Jika satu service mengalami masalah:

- Tidak menjatuhkan seluruh sistem.
- Memiliki retry mechanism.
- Memiliki circuit breaker.
- Memiliki error tracking.

---

# 16. Future Expansion

Boundary ini memungkinkan penambahan:

- AI Service.
- Marketing Service.
- Marketplace Service.
- Partner Service.
 tanpa mengubah core system.
