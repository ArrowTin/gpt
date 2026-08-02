# ChannelHub Error Code Catalog

## Purpose

Menetapkan **kode error resmi** ChannelHub beserta HTTP status dan artinya, sehingga client dapat menangani kegagalan secara deterministik tanpa membaca pesan bebas.

## Scope

Seluruh error yang keluar dari API v1 dan worker yang melaporkan status ke API.

## Context

Struktur envelope error ada pada [002-api-response-format.md](./002-api-response-format.md); endpoint yang memakai kode ini didaftar pada [009-api-endpoint-specification.md](./009-api-endpoint-specification.md).

## Rules

- Kode error adalah kontrak publik: mengubah arti kode adalah breaking change.
- Format kode `DOMAIN_KONDISI` dengan huruf kapital dan underscore.
- `message` boleh berubah dan boleh dilokalkan; `code` tidak.
- Kegagalan validasi mengisi `error.details` per field.
- Error tidak boleh membocorkan detail internal (query, stack trace, nama tabel) ke client.

## Technical Details

### Bentuk response error

```json
{
  "success": false,
  "data": null,
  "message": "Kamar tidak tersedia pada tanggal yang dipilih",
  "error": {
    "code": "RESERVATION_INVENTORY_UNAVAILABLE",
    "details": [
      { "field": "rooms[0].roomTypeId", "rule": "availability", "message": "Sisa 0 unit pada 2026-08-10" }
    ]
  },
  "metadata": {
    "requestId": "0f1c1f5e-6c0e-4f5d-9a3f-9a2f1a2f0d11",
    "correlationId": "6d1b0f24-6d0f-4b0a-8d55-2c9e1a2b3c4d",
    "timestamp": "2026-08-02T10:15:30.000Z"
  }
}
```

### Katalog

| Code | HTTP | Arti |
| --- | --- | --- |
| `AUTH_INVALID_CREDENTIALS` | 401 | Email atau password salah |
| `AUTH_TOKEN_EXPIRED` | 401 | Access token kedaluwarsa |
| `AUTH_TOKEN_INVALID` | 401 | Token rusak, dicabut, atau signature salah |
| `AUTH_REFRESH_REUSED` | 401 | Refresh token yang sudah dirotasi dipakai ulang; seluruh session dicabut |
| `AUTH_ACCOUNT_LOCKED` | 423 | Akun terkunci akibat percobaan login berlebihan |
| `AUTH_MFA_REQUIRED` | 401 | Perlu faktor kedua |
| `TENANT_HEADER_MISSING` | 400 | Header `X-Tenant-Id` tidak dikirim |
| `TENANT_MISMATCH` | 403 | Tenant pada header tidak sesuai klaim token |
| `PERMISSION_DENIED` | 403 | Permission tidak dimiliki user |
| `FEATURE_NOT_ENTITLED` | 403 | Fitur tidak termasuk dalam plan aktif |
| `QUOTA_EXCEEDED` | 429 | Kuota entitlement periode berjalan habis |
| `RATE_LIMIT_EXCEEDED` | 429 | Melebihi rate limit ([006-api-rate-limit-standard.md](./006-api-rate-limit-standard.md)) |
| `VALIDATION_FAILED` | 422 | Payload gagal validasi skema |
| `RESOURCE_NOT_FOUND` | 404 | Resource tidak ada pada tenant aktif |
| `RESOURCE_CONFLICT` | 409 | Melanggar constraint unik |
| `IDEMPOTENCY_KEY_REQUIRED` | 400 | Header `Idempotency-Key` wajib pada endpoint ini |
| `IDEMPOTENCY_KEY_REUSED` | 409 | Kunci sama dipakai dengan payload berbeda |
| `PROPERTY_NOT_ACTIVATABLE` | 409 | Properti tidak punya room type aktif |
| `INVENTORY_VERSION_CONFLICT` | 409 | Versi inventory basi, muat ulang lalu ulangi |
| `INVENTORY_OVERSELL_BLOCKED` | 409 | Perubahan membuat `booked + blocked > total` |
| `RESERVATION_INVENTORY_UNAVAILABLE` | 409 | Ketersediaan tidak cukup untuk rentang menginap |
| `RESERVATION_INVALID_TRANSITION` | 409 | Transisi status tidak diizinkan lifecycle |
| `RESERVATION_DATE_RANGE_INVALID` | 422 | `checkOut` tidak lebih besar dari `checkIn` |
| `CHANNEL_MAPPING_INCOMPLETE` | 409 | Ada room type tanpa mapping pada koneksi |
| `CHANNEL_CREDENTIALS_INVALID` | 400 | Kredensial OTA ditolak provider |
| `CHANNEL_SYNC_IN_PROGRESS` | 409 | Sync sejenis masih berjalan pada koneksi tersebut |
| `WEBHOOK_SIGNATURE_INVALID` | 401 | HMAC signature tidak cocok |
| `WEBHOOK_EVENT_DUPLICATE` | 200 | Event sudah pernah diproses; diabaikan secara sengaja |
| `SUBSCRIPTION_NOT_ACTIVE` | 402 | Subscription kedaluwarsa atau ditangguhkan |
| `WALLET_INSUFFICIENT_CREDIT` | 402 | Saldo credit tidak cukup untuk operasi |
| `INVOICE_ALREADY_PAID` | 409 | Invoice `PAID` tidak dapat diubah |
| `PAYMENT_PROVIDER_ERROR` | 502 | Provider pembayaran gagal merespons |
| `DEPENDENCY_UNAVAILABLE` | 503 | Database, Redis, atau service hilir tidak tersedia |
| `INTERNAL_ERROR` | 500 | Kegagalan tak terduga; `requestId` wajib dicatat pada log |

### Pemetaan ke exception layer backend

| Layer | Mekanisme |
| --- | --- |
| Validation pipe | `VALIDATION_FAILED` beserta detail per field |
| Domain exception | Kode spesifik domain (contoh `RESERVATION_INVALID_TRANSITION`) |
| Guard (auth/tenant/permission) | `AUTH_*`, `TENANT_*`, `PERMISSION_DENIED`, `FEATURE_NOT_ENTITLED` |
| Global exception filter | Menerjemahkan exception menjadi envelope; error tak dikenal menjadi `INTERNAL_ERROR` |

## Impact

- [docs/13-backend-foundation/002-backend-module-design.md](../13-backend-foundation/002-backend-module-design.md) — exception filter memetakan kode ini.
- [docs/14-frontend-foundation/005-api-client-architecture.md](../14-frontend-foundation/005-api-client-architecture.md) — penanganan error di client berdasarkan `code`.
- [checklists/code-review.md](../../checklists/code-review.md) — endpoint baru wajib memakai kode terdaftar.

## References

- [002-api-response-format.md](./002-api-response-format.md)
- [009-api-endpoint-specification.md](./009-api-endpoint-specification.md)
- [docs/22-security/001-security-baseline-platform.md](../22-security/001-security-baseline-platform.md)
