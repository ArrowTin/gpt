# Authorization & Tenant Isolation

## Purpose

Memastikan RBAC, feature entitlement, dan isolasi tenant tidak dapat dilanggar pada API, worker, maupun query database.

## Scope

Role, permission, feature entitlement, propagasi tenant context, dan akses super admin.

## Context

Model role/permission: [docs/03-product-specification/002-role-permission-system.md](../03-product-specification/002-role-permission-system.md). Keputusan isolasi: [adr/ADR-006-multi-tenant-isolation.md](../../adr/ADR-006-multi-tenant-isolation.md). Tabel tenant-owned didaftar pada [docs/15-database-implementation/009-canonical-erd.md](../15-database-implementation/009-canonical-erd.md).

## Rules

- Strategi isolasi adalah shared database dengan kolom `organization_id`; setiap query pada tabel tenant-owned wajib difilter kolom tersebut.
- Tenant id berasal dari token yang sudah divalidasi, tidak pernah dari body request.
- Header `X-Tenant-Id` wajib cocok dengan klaim `tenantId`; ketidakcocokan mengembalikan `TENANT_MISMATCH` (403).
- Permission diperiksa di layer controller/gateway; domain layer menerima tenant id yang sudah tervalidasi.
- Feature entitlement diperiksa terpisah dari permission; kekurangan entitlement mengembalikan `FEATURE_NOT_ENTITLED` (403), bukan 404.
- Background job membawa `organizationId` pada payload; worker menolak job tanpa tenant.
- Super admin adalah break-glass eksplisit dan setiap aksinya wajib tercatat pada `audit_logs`.
- Resource milik tenant lain dijawab 404 `RESOURCE_NOT_FOUND`, bukan 403, agar keberadaannya tidak bocor.

## Technical Details

### Urutan guard

```text
1. JwtAuthGuard        → verifikasi signature, exp, dan sessions.revoked_at via klaim sid
2. TenantContextGuard  → cocokkan X-Tenant-Id dengan tenantId; isi request context
3. PermissionGuard     → cek permission yang dideklarasikan handler
4. EntitlementGuard    → cek feature entitlement subscription aktif
```

Guard berhenti pada kegagalan pertama; response memakai envelope error standar.

### Penegakan di layer data

| Layer | Penegakan |
| --- | --- |
| Repository | Setiap method menerima `organizationId` sebagai parameter wajib, bukan opsional |
| Query | `WHERE organization_id = $1` pada seluruh tabel tenant-owned |
| Tabel turunan | Difilter lewat join ke induk tenant-owned (mis. `reservation_rooms` → `reservations`) |
| Worker | Job payload memuat `organizationId`; processor memuat konteks yang sama seperti HTTP |
| Cache Redis | Key diawali `tenant:{organizationId}:` agar tidak tercampur antar tenant |

### Super admin

Akses lintas tenant hanya untuk endpoint dengan scope platform (konsol admin). Ketentuan:

- Klaim `isSuperAdmin` bernilai true.
- `X-Tenant-Id` boleh menunjuk tenant mana pun, dan wajib tetap dikirim.
- Setiap request menulis `audit_logs` dengan `action` berawalan `SUPERADMIN_`.

### Uji negatif wajib

| Kasus | Ekspektasi |
| --- | --- |
| Token tenant A + `X-Tenant-Id` tenant B | 403 `TENANT_MISMATCH` |
| Token tenant A membaca id resource milik tenant B | 404 `RESOURCE_NOT_FOUND` |
| User tanpa permission memanggil endpoint | 403 `PERMISSION_DENIED` |
| Plan tanpa fitur memanggil endpoint OTA | 403 `FEATURE_NOT_ENTITLED` |
| Job tanpa `organizationId` | Job ditolak dan dicatat sebagai error |

## Impact

- [standards/security.md](../../standards/security.md)
- [checklists/testing-release.md](../../checklists/testing-release.md) — uji negatif tenant wajib per modul CRUD
- [docs/13-backend-foundation/009-backend-project-structure.md](../13-backend-foundation/009-backend-project-structure.md) — lokasi guard

## References

- [adr/ADR-006-multi-tenant-isolation.md](../../adr/ADR-006-multi-tenant-isolation.md)
- [docs/13-backend-foundation/004-authentication-authorization-foundation.md](../13-backend-foundation/004-authentication-authorization-foundation.md)
- [docs/16-api-contract/010-error-code-catalog.md](../16-api-contract/010-error-code-catalog.md)
- [002-authentication-hardening.md](./002-authentication-hardening.md)
