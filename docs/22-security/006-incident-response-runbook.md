# Incident Response Runbook

## Purpose

Runbook operasional untuk insiden keamanan ChannelHub.

## Scope

Deteksi, containment, eradication, recovery, postmortem.

## Context

Observability: [docs/02-product-architecture/006-observability-architecture.md](../02-product-architecture/006-observability-architecture.md). Security baseline: [001-security-baseline-platform.md](./001-security-baseline-platform.md).

## Rules

- Tindakan pertama selalu preservasi bukti: simpan log dan `correlation_id` sebelum mengubah apa pun.
- Setiap langkah respons dicatat pada `audit_logs`.
- Komunikasi ke tenant terdampak memakai template dan hanya menyebut lingkup tenant yang bersangkutan.
- SEV1 dan SEV2 wajib menghasilkan postmortem tertulis.

### Severity

| Level | Kriteria | Waktu respons awal |
| --- | --- | --- |
| SEV1 | Dugaan kebocoran data atau akses lintas tenant terbukti | 15 menit |
| SEV2 | Kredensial bocor, otentikasi bypass, layanan tidak tersedia | 1 jam |
| SEV3 | Kerentanan tereksploitasi terbatas, lonjakan gagal login | 1 hari kerja |
| SEV4 | Temuan scanner tanpa eksploitasi, noise | Backlog terjadwal |

## Procedure

1. **Detect** — alert from failed auth spike, WAF, anomaly monitoring.
2. **Triage** — identify tenant scope, timeline, blast radius.
3. **Contain** — cabut session terdampak (`sessions.revoked_at`), nonaktifkan `channel_connections` yang bocor, blokir IP, batasi ke mode baca bila perlu.
4. **Eradicate** — perbaiki kerentanan, rotasi secret sesuai [004-secrets-and-credential-management.md](./004-secrets-and-credential-management.md).
5. **Recover** — pulihkan layanan, jalankan smoke test ([checklists/deployment-production.md](../../checklists/deployment-production.md)).
6. **Postmortem** — dokumentasikan akar masalah; buat ADR bila perubahan arsitektur diperlukan.

### Aksi containment cepat

| Gejala | Containment |
| --- | --- |
| Refresh token bocor | Revoke seluruh session user, paksa login ulang |
| Kunci JWT bocor | Rotasi signing key, seluruh access token gugur setelah 15 menit |
| Kredensial OTA bocor | Set `channel_connections.status = 'REVOKED'`, hapus kredensial, minta kredensial baru ke tenant |
| Webhook palsu diterima | Ganti webhook secret, tolak event tanpa HMAC valid |
| Akses lintas tenant | Nonaktifkan endpoint terdampak, audit `audit_logs` per `organization_id` |

## Rollback

- Prefer rollback release if exploit tied to recent deploy; otherwise hotfix forward.

## Verification

- Security tests pass; audit log complete for response actions.

## References

- [templates/runbook-template.md](../../templates/runbook-template.md)
- [checklists/maintenance-operations.md](../../checklists/maintenance-operations.md)
- [prompts/lifecycle/04-maintenance-increment.md](../../prompts/lifecycle/04-maintenance-increment.md)
