# ChannelHub Domain Architecture

## Purpose

Dokumen ini mendefinisikan pembagian domain bisnis ChannelHub sebagai dasar desain microservice dan modular system.

---

# 1. Domain Architecture Principle

ChannelHub menggunakan pendekatan Domain Driven Design.

Tujuan:

- Memisahkan tanggung jawab bisnis.
- Mengurangi coupling.
- Mempermudah scale.
- Mempermudah maintenance.

---

# 2. Core Domain Map

```
ChannelHub Platform

├── Identity Domain
├── Organization Domain
├── Property Domain
├── Reservation Domain
├── OTA Integration Domain
├── Subscription Domain
├── Billing Domain
├── Notification Domain
├── Reporting Domain
└── Platform Management Domain
```

---

# 3. Identity Domain

Tanggung jawab:

- Authentication.
- User management.
- Session.
- Security policy.

Data owner:

```
users
roles
permissions
sessions
```

---

# 4. Organization Domain

Tanggung jawab:

- Subscriber organization.
- Tenant management.
- Membership.
- Organization setting.

Konsep:

```
One ChannelHub
      |
      +-- Organization A
      +-- Organization B
```

---

# 5. Property Domain

Tanggung jawab:

- Property profile.
- Room.
- Facility.
- Inventory master.

Property domain menjadi sumber data utama properti.

---

# 6. Reservation Domain

Tanggung jawab:

- Booking.
- Reservation lifecycle.
- Cancellation.
- Modification.

Event:

```
ReservationCreated
ReservationCancelled
ReservationUpdated
```

---

# 7. OTA Integration Domain

Tanggung jawab:

- Connector.
- Authentication OTA.
- Mapping.
- Synchronization.
- Webhook.

Tidak memiliki master property.

OTA hanya consumer data.

---

# 8. Subscription Domain

Tanggung jawab:

- Plan.
- Subscription lifecycle.
- Feature entitlement.
- Usage limit.

---

# 9. Billing Domain

Tanggung jawab:

- Invoice.
- Payment record.
- Transaction ledger.
- Settlement.

---

# 10. Notification Domain

Tanggung jawab:

- Email.
- Push notification.
- System alert.

---

# 11. Reporting Domain

Tanggung jawab:

- Analytics.
- Dashboard.
- Business report.

Reporting tidak menjadi sumber transaksi utama.

---

# 12. Platform Management Domain

Khusus internal ChannelHub.

Mengelola:

- System configuration.
- Feature flag.
- Monitoring.
- Audit log.

---

# 13. Domain Communication

Komunikasi antar domain menggunakan:

- REST API.
- Event driven messaging.
- Message queue.

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

# 14. Ownership Rule

Setiap domain memiliki:

- Database ownership.
- Business rule sendiri.
- Service lifecycle sendiri.

Tidak diperbolehkan service lain mengakses database domain secara langsung.

---

# 15. Future Expansion

Arsitektur mendukung penambahan:

- AI Assistant Domain.
- Marketing Domain.
- Marketplace Domain.
- Partner Domain.
 tanpa mengubah core domain.
