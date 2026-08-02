# Secrets and Credential Management

## Purpose

Standar pengelolaan secret untuk development, staging, dan production, termasuk kredensial OTA per tenant.

## Scope

API key OTA, kredensial database, kunci penandatangan JWT, kunci enkripsi data, password Redis, webhook secret, dan secret CI.

## Context

Environment deployment: [docs/21-integration-deployment/004-environment-deployment.md](../21-integration-deployment/004-environment-deployment.md). Kolom penyimpanan: `channel_connections.credentials_encrypted` (bytea) dan `channel_connections.webhook_secret_hash` pada [docs/15-database-implementation/010-postgresql-ddl-reference.md](../15-database-implementation/010-postgresql-ddl-reference.md).

## Rules

- Tidak ada secret asli di git, termasuk file `.env` yang ter-commit; repository hanya memuat `.env.example` berisi nama variabel.
- Secret berbeda per environment; kredensial production tidak pernah dipakai di staging atau lokal.
- Kredensial OTA disimpan terenkripsi (AES-256-GCM) pada `channel_connections.credentials_encrypted`; kunci enkripsi berasal dari secret manager, bukan dari database.
- Kredensial OTA tidak pernah dikembalikan oleh API; response hanya memuat status koneksi dan metadata.
- Webhook secret disimpan sebagai hash; verifikasi HMAC SHA-256 memakai perbandingan constant-time.
- Setiap secret punya pemilik dan frekuensi rotasi yang terdokumentasi.
- Aplikasi gagal cepat saat start bila ada variabel environment wajib yang kosong.

## Technical Details

### Klasifikasi

| Secret | Penyimpanan | Rotasi |
| --- | --- | --- |
| JWT signing key | Secret manager, di-inject sebagai env | 90 hari |
| Kunci enkripsi data (OTA credential) | Secret manager, versioned | 180 hari |
| DATABASE_URL | Secret manager | 180 hari |
| Redis password | Secret manager | 180 hari |
| Kredensial OTA per tenant | `channel_connections.credentials_encrypted` | mengikuti kebijakan OTA |
| Webhook secret per koneksi | hash pada `channel_connections.webhook_secret_hash` | saat koneksi dibuat ulang |
| CI deploy key | Secret store CI | 180 hari |

### Enkripsi kredensial OTA

```text
plaintext  : { "apiKey": "...", "apiSecret": "..." }
key        : DATA_ENCRYPTION_KEY versi aktif dari secret manager
algoritma  : AES-256-GCM
disimpan   : keyVersion || nonce || ciphertext || authTag  (bytea)
dekripsi   : hanya di worker/service OTA, tidak pernah pada layer controller
```

Versi kunci disimpan di depan payload sehingga rotasi kunci dapat berjalan bertahap tanpa migrasi serentak.

### Prosedur rotasi

```text
1. Terbitkan kunci baru dan tandai sebagai versi aktif.
2. Jalankan overlap window: verifikasi menerima kunci lama dan baru.
3. Deploy konsumer yang memakai kunci baru.
4. Enkripsi ulang data lama secara batch (job terjadwal).
5. Cabut kunci lama.
6. Tulis entri audit_logs untuk aktivitas rotasi.
```

Rotasi JWT signing key memakai overlap window minimal selama umur access token (15 menit) ditambah margin deploy.

### Aturan logging

- Nilai secret, token, dan password tidak pernah masuk log maupun pesan error.
- Field sensitif direduksi menjadi placeholder pada log terstruktur.
- Log yang memuat kredensial dianggap insiden; ikuti [006-incident-response-runbook.md](./006-incident-response-runbook.md).

## Impact

- CI: [docs/21-integration-deployment/003-ci-cd-pipeline.md](../21-integration-deployment/003-ci-cd-pipeline.md) — tambahkan secret scanning
- Maintenance: [checklists/maintenance-operations.md](../../checklists/maintenance-operations.md)
- Database: kolom terenkripsi pada [docs/15-database-implementation/010-postgresql-ddl-reference.md](../15-database-implementation/010-postgresql-ddl-reference.md)

## References

- [001-security-baseline-platform.md](./001-security-baseline-platform.md)
- [standards/deployment.md](../../standards/deployment.md)
- [adr/ADR-009-configuration-driven-platform.md](../../adr/ADR-009-configuration-driven-platform.md)
