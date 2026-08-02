# ChannelHub API Authentication Standard

## Purpose

Menetapkan mekanisme **autentikasi, tenant context, dan otorisasi** pada seluruh API ChannelHub, termasuk isi token dan aturan rotasi.

## Scope

Autentikasi user melalui frontend, autentikasi service-to-service, dan verifikasi webhook OTA.

## Context

Hardening: [docs/22-security/002-authentication-hardening.md](../22-security/002-authentication-hardening.md). Isolasi tenant: [docs/22-security/003-authorization-tenant-isolation.md](../22-security/003-authorization-tenant-isolation.md) dan [adr/ADR-006-multi-tenant-isolation.md](../../adr/ADR-006-multi-tenant-isolation.md).

## Rules

- Access token JWT berumur pendek (15 menit); refresh token berumur panjang (14 hari) dan wajib dirotasi setiap pemakaian.
- Refresh token disimpan sebagai hash pada tabel `sessions`; token asli tidak pernah tersimpan di server.
- Frontend menyimpan refresh token pada cookie `HttpOnly`, `Secure`, `SameSite=Lax`; tidak pernah di `localStorage`.
- Setiap request tenant-scoped mengirim `X-Tenant-Id` dan server memvalidasinya terhadap klaim `tenantId` pada token.
- Permission diperiksa di layer gateway/controller; domain layer menerima tenant id yang sudah tervalidasi.
- Pemakaian ulang refresh token yang sudah dirotasi mencabut seluruh session user (`AUTH_REFRESH_REUSED`).

## Technical Details

### Alur autentikasi

```text
POST /auth/login
      |
Validasi credential (Argon2id) + cek lock
      |
Buat sessions (refresh_token_hash)
      |
Terbitkan accessToken + refreshToken
      |
Request ber-Authorization + X-Tenant-Id
      |
Guard: token → tenant → permission → entitlement
```

### Klaim access token

```json
{
  "sub": "d3f1...",
  "email": "owner@hotel.example",
  "tenantId": "7a2c...",
  "roles": ["OWNER"],
  "permissions": ["PROPERTY_READ", "PROPERTY_UPDATE"],
  "features": ["ota_sync", "analytics"],
  "isSuperAdmin": false,
  "sid": "9b1e...",
  "iat": 1785600000,
  "exp": 1785600900
}
```

- `sid` merujuk `sessions.id` sehingga token dapat dicabut sebelum kedaluwarsa.
- `permissions` dan `features` disalin ke token untuk menghindari query per request; perubahan permission mencabut session terkait agar token basi tidak dipakai.
- Token tidak boleh memuat data pribadi selain email dan identitas minimum.

### Urutan guard

| Urutan | Guard | Kegagalan |
| --- | --- | --- |
| 1 | `JwtAuthGuard` | `AUTH_TOKEN_INVALID`, `AUTH_TOKEN_EXPIRED` |
| 2 | `TenantContextGuard` | `TENANT_HEADER_MISSING`, `TENANT_MISMATCH` |
| 3 | `PermissionGuard` | `PERMISSION_DENIED` |
| 4 | `EntitlementGuard` | `FEATURE_NOT_ENTITLED`, `QUOTA_EXCEEDED` |

### Super admin

Akses lintas tenant hanya untuk token dengan `isSuperAdmin: true`, wajib memakai `X-Tenant-Id` eksplisit sasaran, dan setiap request menulis `audit_logs` dengan aksi `SUPER_ADMIN_ACCESS`.

### Service-to-service dan webhook

- Antar service internal memakai token service account berumur pendek dengan scope terbatas, bukan token user.
- Webhook OTA tidak memakai bearer token; verifikasi memakai HMAC SHA-256 pada header `X-Signature` dengan secret per `channel_connections` ([003-webhook-architecture.md](./003-webhook-architecture.md)).

## Impact

- [docs/13-backend-foundation/004-authentication-authorization-foundation.md](../13-backend-foundation/004-authentication-authorization-foundation.md) — implementasi guard.
- [docs/14-frontend-foundation/006-frontend-authentication-flow.md](../14-frontend-foundation/006-frontend-authentication-flow.md) — penyimpanan token dan refresh di client.
- [checklists/security-review.md](../../checklists/security-review.md) — verifikasi rotasi dan pencabutan token.

## References

- [009-api-endpoint-specification.md](./009-api-endpoint-specification.md)
- [010-error-code-catalog.md](./010-error-code-catalog.md)
- [docs/21-integration-deployment/001-frontend-backend-integration.md](../21-integration-deployment/001-frontend-backend-integration.md)
