# ChannelHub API Endpoint Specification

## Purpose

Menjelaskan **seluruh endpoint REST v1** ChannelHub: tanggung jawab, permission, dan perilaku khusus. Definisi teknis request/response berada pada file OpenAPI [contracts/openapi/channelhub.v1.yaml](../../contracts/openapi/channelhub.v1.yaml) yang menjadi sumber kebenaran.

## Scope

API publik platform di bawah prefix `/api/v1`. Endpoint internal antar service dan endpoint worker tidak termasuk.

## Context

Standar REST: [001-rest-api-standard.md](./001-rest-api-standard.md). Envelope: [002-api-response-format.md](./002-api-response-format.md). Auth: [005-api-authentication-standard.md](./005-api-authentication-standard.md). Model data: [docs/15-database-implementation/009-canonical-erd.md](../15-database-implementation/009-canonical-erd.md).

## Rules

- OpenAPI adalah kontrak; controller, DTO, dan client SDK diturunkan darinya, bukan sebaliknya.
- Seluruh endpoint tenant-scoped wajib mengirim header `X-Tenant-Id` dan divalidasi terhadap klaim token (ADR-006).
- Endpoint yang mengubah state secara bulk atau transaksional wajib menerima header `Idempotency-Key`.
- Response selalu memakai envelope; tidak ada endpoint yang mengembalikan array telanjang.
- Penambahan field bersifat backward compatible; penghapusan atau perubahan tipe menuntut versi baru ([007-api-versioning-strategy.md](./007-api-versioning-strategy.md)).

## Technical Details

### Konvensi

| Aspek | Aturan |
| --- | --- |
| Base path | `/api/v1` |
| Format id | `uuid` |
| Penamaan JSON | `camelCase` (kolom database `snake_case` dipetakan di layer mapper) |
| Pagination | `?page=1&pageSize=20`, maksimum `pageSize=100` |
| Tanggal | `date` untuk tanggal menginap, `date-time` ISO 8601 UTC untuk timestamp |
| Header wajib | `Authorization: Bearer <accessToken>`, `X-Tenant-Id`, `X-Correlation-Id` (opsional, diteruskan ke log) |

### Katalog endpoint

| Method & Path | Tanggung jawab | Permission |
| --- | --- | --- |
| `POST /auth/login` | Terbitkan access + refresh token | publik |
| `POST /auth/refresh` | Rotasi refresh token | publik (token) |
| `POST /auth/logout` | Cabut session aktif | authenticated |
| `GET /users/me` | Profil, permission efektif, entitlement, daftar organisasi | authenticated |
| `GET /users` | Daftar user tenant | `USER_READ` |
| `POST /users` | Undang user | `USER_CREATE` |
| `GET /users/{userId}` | Detail user | `USER_READ` |
| `PATCH /users/{userId}` | Ubah profil, status, role | `USER_UPDATE` |
| `GET /organizations` | Daftar organisasi user (semua tenant untuk super admin) | authenticated |
| `POST /organizations` | Buat organisasi baru | authenticated |
| `GET /organizations/{organizationId}` | Detail organisasi | `ORGANIZATION_READ` |
| `PATCH /organizations/{organizationId}` | Ubah profil dan branding | `ORGANIZATION_UPDATE` |
| `GET /roles` | Daftar role + permission | `ROLE_READ` |
| `POST /roles` | Buat role custom | `ROLE_CREATE` |
| `GET /permissions` | Katalog permission platform | `ROLE_READ` |
| `GET /properties` | Daftar properti | `PROPERTY_READ` |
| `POST /properties` | Buat properti | `PROPERTY_CREATE` |
| `GET /properties/{propertyId}` | Detail properti | `PROPERTY_READ` |
| `PATCH /properties/{propertyId}` | Ubah properti | `PROPERTY_UPDATE` |
| `DELETE /properties/{propertyId}` | Arsipkan properti | `PROPERTY_DELETE` |
| `GET /properties/{propertyId}/room-types` | Daftar room type | `PROPERTY_READ` |
| `POST /properties/{propertyId}/room-types` | Buat room type | `PROPERTY_UPDATE` |
| `GET /properties/{propertyId}/inventory` | Kalender inventory | `INVENTORY_READ` |
| `PUT /properties/{propertyId}/inventory` | Update inventory bulk + jadwalkan sync | `INVENTORY_UPDATE` |
| `GET /properties/{propertyId}/rates` | Kalender harga | `RATE_READ` |
| `PUT /properties/{propertyId}/rates` | Update harga bulk + jadwalkan sync | `RATE_UPDATE` |
| `GET /availability` | Ketersediaan yang dapat dijual | `INVENTORY_READ` |
| `GET /reservations` | Daftar reservasi | `RESERVATION_READ` |
| `POST /reservations` | Buat reservasi (transaksional) | `RESERVATION_CREATE` |
| `GET /reservations/{reservationId}` | Detail reservasi | `RESERVATION_READ` |
| `PATCH /reservations/{reservationId}/status` | Transisi status lifecycle | `RESERVATION_UPDATE` |
| `GET /reservations/{reservationId}/events` | Riwayat lifecycle | `RESERVATION_READ` |
| `GET /channels` | Katalog channel OTA platform | authenticated |
| `GET /channel-connections` | Koneksi channel tenant | `CHANNEL_READ` |
| `POST /channel-connections` | Hubungkan property ke channel | `CHANNEL_CONNECT` |
| `PUT /channel-connections/{connectionId}/mappings` | Set mapping room/rate ke id OTA | `CHANNEL_UPDATE` |
| `POST /channel-connections/{connectionId}/sync` | Jadwalkan sync job | `CHANNEL_SYNC` |
| `GET /sync-jobs` | Riwayat sync | `CHANNEL_READ` |
| `POST /webhooks/ota/{channelCode}` | Terima webhook OTA | signature HMAC |
| `GET /subscription-plans` | Katalog plan | publik |
| `GET /subscriptions/current` | Subscription + entitlement tenant | `SUBSCRIPTION_READ` |
| `PATCH /subscriptions/current` | Upgrade / downgrade plan | `SUBSCRIPTION_UPDATE` |
| `GET /invoices` | Daftar invoice | `BILLING_READ` |
| `GET /invoices/{invoiceId}` | Detail invoice | `BILLING_READ` |
| `GET /wallet` | Saldo credit | `BILLING_READ` |
| `GET /wallet/transactions` | Riwayat credit | `BILLING_READ` |
| `GET /notifications` | Notifikasi user | authenticated |
| `POST /notifications/{notificationId}/read` | Tandai terbaca | authenticated |
| `GET /menus` | Menu dinamis sesuai permission + entitlement | authenticated |
| `GET /audit-logs` | Audit log tenant | `AUDIT_READ` |
| `GET /health` | Liveness & readiness | publik |

### Perilaku khusus

**`POST /reservations`** menjalankan satu transaksi database: validasi ketersediaan setiap malam, menaikkan `inventory.booked_units`, menulis `reservations` + `reservation_rooms`, lalu menulis `reservation_events` dan mempublikasikan `ReservationCreated`. Kekurangan inventory menghasilkan `409` dengan kode `RESERVATION_INVENTORY_UNAVAILABLE`, bukan `422`.

**`PUT .../inventory` dan `PUT .../rates`** bersifat bulk dan idempoten. Client mengirim `version` terakhir yang dibaca; mismatch menghasilkan `409` `INVENTORY_VERSION_CONFLICT`. Setelah sukses, response memuat `syncJobIds` yang dijadwalkan ke setiap koneksi channel aktif.

**`POST /channel-connections/{id}/sync`** mengembalikan `202` karena diproses worker BullMQ (ADR-005). Bila credit wallet tidak mencukupi, response `402` `WALLET_INSUFFICIENT_CREDIT`.

**`POST /webhooks/ota/{channelCode}`** tidak memakai bearer token. Verifikasi memakai HMAC pada header `X-Signature` dan deduplikasi memakai `X-Event-Id`. Response selalu `202` untuk event valid agar OTA tidak melakukan retry berlebihan; kegagalan pemrosesan ditangani internal ([003-webhook-architecture.md](./003-webhook-architecture.md)).

**`GET /menus`** menghasilkan menu dari tabel `menus` yang difilter `required_permission` dan `required_feature`; frontend tidak boleh menyusun menu secara statis.

### Transisi status reservasi yang diizinkan

| Dari | Ke |
| --- | --- |
| `PENDING` | `CONFIRMED`, `CANCELLED` |
| `CONFIRMED` | `CHECKED_IN`, `CANCELLED`, `NO_SHOW` |
| `CHECKED_IN` | `CHECKED_OUT` |
| `CHECKED_OUT` | — |
| `CANCELLED` | — |
| `NO_SHOW` | — |

Transisi di luar tabel ditolak `409` `RESERVATION_INVALID_TRANSITION`.

## Impact

- [contracts/openapi/channelhub.v1.yaml](../../contracts/openapi/channelhub.v1.yaml) — perubahan endpoint wajib diikuti perubahan spec.
- [docs/19-backend-application/](../19-backend-application/) — controller mengikuti katalog ini.
- [docs/20-frontend-application/](../20-frontend-application/) — API client digenerate dari OpenAPI.
- [checklists/testing-release.md](../../checklists/testing-release.md) — contract test memakai spec ini.

## References

- [010-error-code-catalog.md](./010-error-code-catalog.md)
- [008-api-documentation-standard.md](./008-api-documentation-standard.md)
- [docs/09-database-api-contract/009-api-endpoint-catalog.md](../09-database-api-contract/009-api-endpoint-catalog.md)
- [adr/ADR-008-api-first-rest-contract.md](../../adr/ADR-008-api-first-rest-contract.md)
