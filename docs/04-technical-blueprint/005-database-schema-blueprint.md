# ChannelHub Database Schema Blueprint

## Purpose

Mendefinisikan standar desain database berdasarkan ownership microservice.

---

# 1. Database Principle

Setiap service memiliki data ownership sendiri.

```
Service
  |
  owns
  |
Database Schema
```

Tidak ada direct query antar service.

---

# 2. Core Schema Direction

Identity:

```
users
roles
permissions
sessions
```

Organization:

```
organizations
memberships
settings
```

Property:

```
properties
rooms
facilities
```

Reservation:

```
reservations
guests
booking_history
```

---

# 3. Migration Strategy

Setiap service memiliki:

- Versioned migration.
- Rollback plan.
- Schema documentation.

---

# 4. Performance

Menggunakan:

- Indexing.
- Query optimization.
- Connection pooling.
- Caching.

---

# 5. Data Governance

Setiap perubahan schema harus:

- Review.
- Tested.
- Documented.
