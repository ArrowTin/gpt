# Security Testing and Audit

## Purpose

Mendefinisikan pengujian keamanan wajib dan struktur audit trail ChannelHub.

## Scope

Static analysis, dependency scan, uji auth, uji isolasi tenant, verifikasi audit log, dan gate CI.

## Context

Strategi testing: [docs/02-product-architecture/010-testing-strategy.md](../02-product-architecture/010-testing-strategy.md). Pipeline: [docs/21-integration-deployment/003-ci-cd-pipeline.md](../21-integration-deployment/003-ci-cd-pipeline.md). Struktur tabel: `audit_logs` pada [docs/15-database-implementation/010-postgresql-ddl-reference.md](../15-database-implementation/010-postgresql-ddl-reference.md).

## Rules

- Stage security scan wajib pada CI; temuan critical menggagalkan build kecuali dikecualikan lewat ADR.
- Suite regresi auth wajib mencakup login, refresh, rotasi, reuse, dan revocation.
- Setiap modul CRUD wajib memiliki uji negatif isolasi tenant.
- Aksi privileged wajib menulis `audit_logs`; ketiadaan entri diperlakukan sebagai kegagalan test.
- Audit log bersifat append-only; tidak ada endpoint update atau delete.

## Technical Details

### Kategori pengujian

| Kategori | Contoh kasus |
| --- | --- |
| Unit | Password policy validator, permission guard, HMAC verifier, state machine reservasi |
| Integration | `TENANT_MISMATCH` 403, akses lintas tenant 404, lockout setelah 5 gagal login, reuse refresh token mencabut session |
| Contract | Response dibandingkan dengan `contracts/openapi/channelhub.v1.yaml` |
| CI | SAST, dependency CVE, secret scanning, container scan |

### Aksi yang wajib diaudit

| Domain | Aksi |
| --- | --- |
| Identity | login sukses/gagal, logout, reset password, revocation session |
| Role | pembuatan role, perubahan permission, penetapan role ke user |
| Organization | perubahan status tenant, perubahan setting |
| OTA | pembuatan/pengubahan kredensial, pemutusan koneksi |
| Billing | penerbitan invoice, penandaan lunas, penyesuaian credit wallet |
| Platform | perubahan feature flag, menu, system configuration, seluruh aksi super admin |

### Field audit

Mengikuti kolom tabel `audit_logs`:

```text
id, organization_id, actor_user_id, action, resource_type, resource_id,
previous_value, new_value, correlation_id, ip_address, created_at
```

`previous_value` dan `new_value` menyimpan jsonb yang sudah dibersihkan dari field sensitif (password hash, kredensial, token).

### Gate rilis

```text
1. Lint + unit test hijau
2. Contract test terhadap OpenAPI hijau
3. Uji negatif tenant hijau
4. Security scan tanpa temuan critical
5. checklists/security-review.md terisi untuk perubahan security-sensitive
```

## Impact

- [prompts/lifecycle/02-testing-increment.md](../../prompts/lifecycle/02-testing-increment.md)
- [checklists/testing-release.md](../../checklists/testing-release.md)
- [checklists/security-review.md](../../checklists/security-review.md)

## References

- [docs/03-product-specification/010-audit-log-system.md](../03-product-specification/010-audit-log-system.md)
- [003-authorization-tenant-isolation.md](./003-authorization-tenant-isolation.md)
- [standards/testing.md](../../standards/testing.md)
