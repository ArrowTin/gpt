# ChannelHub API Endpoint Catalog Blueprint

> **Status: konseptual.** Dokumen ini menjelaskan pemikiran domain pada Phase 09.
> Sumber kebenaran implementasi adalah contract artifact:
> [contracts/openapi/channelhub.v1.yaml](../../contracts/openapi/channelhub.v1.yaml),
> [docs/15-database-implementation/009-canonical-erd.md](../15-database-implementation/009-canonical-erd.md),
> [docs/15-database-implementation/010-postgresql-ddl-reference.md](../15-database-implementation/010-postgresql-ddl-reference.md).
> Bila terjadi perbedaan, contract artifact yang berlaku.

## Purpose

Mendefinisikan daftar endpoint utama sebelum implementasi backend.

---

# Identity API

```
POST /auth/login
POST /auth/logout
GET /users/me
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
GET /users            # anggota tenant aktif, difilter X-Tenant-Id
```

Responsibility:

- Tenant management.
- Membership.

---

# Property API

```
POST /properties
GET /properties/:id
PUT /properties/:id/inventory
PUT /properties/:id/rates
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
POST /channel-connections
POST /channel-connections/:id/sync
GET /sync-jobs
POST /webhooks/ota/:channelCode
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

Daftar endpoint final beserta permission dan perilaku khususnya ada di [docs/16-api-contract/009-api-endpoint-specification.md](../16-api-contract/009-api-endpoint-specification.md).
