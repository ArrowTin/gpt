# ChannelHub Database Architecture

## Purpose

Mendefinisikan strategi database ChannelHub agar data aman, scalable, dan mudah dipelihara.

---

# 1. Principle

Database mengikuti ownership domain.

Aturan:

- Service memiliki database/schema sendiri.
- Tidak ada direct database access antar service.
- Migration terkontrol.
- Backup dan recovery wajib tersedia.

---

# 2. Primary Database

PostgreSQL menjadi database utama untuk transactional data.

Digunakan untuk:

- User.
- Organization.
- Property.
- Room.
- Reservation.
- Subscription.
- Billing.

---

# 3. Database Ownership

Contoh:

```
identity-db
organization-db
property-db
reservation-db
subscription-db
billing-db
```

---

# 4. Redis Usage

Redis digunakan untuk:

- Cache.
- Session.
- Queue support.
- Rate limiting.
- Temporary state.

---

# 5. Data Security

Requirement:

- Encryption.
- Access control.
- Audit trail.
- Credential protection.

---

# 6. Backup Strategy

Mendukung:

- Scheduled backup.
- Point in time recovery.
- Backup verification.

---

# 7. Migration Strategy

Setiap service memiliki:

- Migration version.
- Rollback strategy.
- Schema documentation.
