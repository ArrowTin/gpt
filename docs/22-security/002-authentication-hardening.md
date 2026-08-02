# Authentication Hardening

## Purpose

Memperketat authentication end-to-end dengan nilai konkret: umur token, isi klaim, rotasi refresh token, dan proteksi brute force.

## Scope

Login, penerbitan token, refresh, revocation session, password policy, dan proteksi brute force. Kontrak API-nya ada di [docs/16-api-contract/005-api-authentication-standard.md](../16-api-contract/005-api-authentication-standard.md); dokumen ini menetapkan parameter hardening-nya.

## Context

Tabel `sessions` pada [docs/15-database-implementation/010-postgresql-ddl-reference.md](../15-database-implementation/010-postgresql-ddl-reference.md) menyimpan hash refresh token beserta kolom `rotated_from`, `revoked_at`, dan `expires_at`. Kolom `users.failed_login_count` dan `users.locked_until` menopang lockout.

## Rules

- Access token JWT 15 menit; refresh token 14 hari dan dirotasi setiap pemakaian.
- Refresh token hanya disimpan sebagai hash (`sessions.refresh_token_hash`); nilai asli tidak pernah dipersistensikan.
- Refresh hanya melalui `POST /auth/refresh`; API client melakukan maksimal satu retry per request.
- Pemakaian ulang refresh token yang sudah dirotasi mencabut **seluruh** session milik user tersebut dan mengembalikan `AUTH_REFRESH_REUSED`.
- Password di-hash dengan Argon2id; perbandingan rahasia memakai fungsi constant-time.
- Refresh token tidak pernah disimpan di `localStorage`; produksi memakai cookie `HttpOnly`, `Secure`, `SameSite=Lax`.
- Login gagal dan berhasil selalu menulis `audit_logs`; password dan token tidak pernah masuk log.
- Kebijakan password dan lockout bersifat configuration-driven melalui `system_configurations`, bukan konstanta di kode.

## Technical Details

### Parameter baseline

| Parameter | Nilai default | Sumber konfigurasi |
| --- | --- | --- |
| Access token TTL | 15 menit | `auth.access_token_ttl` |
| Refresh token TTL | 14 hari | `auth.refresh_token_ttl` |
| Panjang password minimum | 12 karakter | `auth.password_min_length` |
| Riwayat password | 3 terakhir tidak boleh dipakai ulang | `auth.password_history` |
| Batas gagal login per akun | 5 percobaan | `auth.max_failed_login` |
| Durasi lockout | 15 menit (`users.locked_until`) | `auth.lockout_duration` |
| Rate limit login per IP | 10 percobaan / 5 menit | `auth.login_rate_limit_ip` |
| Argon2id | m=64MB, t=3, p=1 | `auth.argon2_params` |

Hitungan gagal login direset saat login berhasil. Counter per IP disimpan di Redis; counter per akun disimpan pada `users.failed_login_count` agar tahan restart.

### Klaim token minimum

| Klaim | Isi |
| --- | --- |
| `sub` | `users.id` |
| `tenantId` | `organizations.id` aktif |
| `roles` | kode role efektif |
| `permissions` | kode permission efektif |
| `features` | feature entitlement aktif |
| `isSuperAdmin` | boolean break-glass |
| `sid` | `sessions.id`, dipakai untuk revocation |
| `iat`, `exp` | waktu terbit dan kedaluwarsa |

Contoh isi lengkap ada di [docs/16-api-contract/005-api-authentication-standard.md](../16-api-contract/005-api-authentication-standard.md). Revocation dicek dengan mencocokkan `sid` ke `sessions.revoked_at`.

### Rotasi refresh token

```text
1. Client kirim refresh token.
2. Server mencari sessions.refresh_token_hash yang cocok.
3. Tidak ada baris cocok → token tidak dikenal: tolak (AUTH_TOKEN_INVALID).
4. Baris cocok tetapi revoked_at terisi → reuse: revoke seluruh session user, tulis audit, tolak (AUTH_REFRESH_REUSED).
5. Baris cocok dan masih aktif → terbitkan session baru dengan rotated_from = session lama, lalu set revoked_at pada session lama.
```

### MFA

Interface MFA disiapkan (`users.mfa_secret_encrypted`) tetapi belum diaktifkan. Pengaktifan memerlukan ADR baru dan endpoint tambahan pada kontrak OpenAPI.

## Impact

- Prompt: [prompts/phases/phase-22-security.md](../../prompts/phases/phase-22-security.md) → MP-002
- Checklist: [checklists/security-review.md](../../checklists/security-review.md)
- Testing: [005-security-testing-and-audit.md](./005-security-testing-and-audit.md)

## References

- [001-security-baseline-platform.md](./001-security-baseline-platform.md)
- [docs/16-api-contract/010-error-code-catalog.md](../16-api-contract/010-error-code-catalog.md)
- [diagrams/004-auth-flow.md](../../diagrams/004-auth-flow.md)
- [standards/api.md](../../standards/api.md)
