# ChannelHub API Endpoint Catalog Blueprint

## Purpose

Mendefinisikan daftar endpoint utama sebelum implementasi backend.

---

# Identity API

```
POST /auth/login
POST /auth/logout
GET /users/profile
```

Responsibility:

- Authentication.
- Session.
- User identity.

---

# Organization API

```
POST /organizations
GET /organizations/:id
GET /members
```

Responsibility:

- Tenant management.
- Membership.

---

# Property API

```
POST /properties
GET /properties/:id
POST /inventory/update
POST /rates/update
```

Responsibility:

- Property.
- Inventory.
- Rate.

---

# Reservation API

```
POST /reservations
GET /reservations/:id
PATCH /reservations/:id/status
```

Responsibility:

- Booking lifecycle.
- Transaction management.

---

# OTA API

```
POST /channels/connect
POST /sync/start
GET /sync/logs
POST /webhooks/ota
```

Responsibility:

- Channel connection.
- Synchronization.
- Webhook handling.

---

# API Rule

Setiap endpoint wajib memiliki:

- Authentication.
- Authorization.
- Validation.
- Documentation.
- Error contract.
