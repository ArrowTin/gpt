# Environment Deployment

## Overview

Dokumen ini mendefinisikan environment deployment ChannelHub dari development sampai production. Setiap environment harus konsisten secara konfigurasi, berbeda hanya pada kapasitas, credential, policy, dan data.

## Development

Tujuan development adalah mempercepat iterasi engineer dan AI agent.

Karakteristik:

- Berjalan lokal menggunakan Docker Compose.
- Data dapat di-reset melalui seed.
- Logging verbose diizinkan.
- External provider dapat menggunakan sandbox atau mock.
- Secret disimpan di `.env.local` yang tidak boleh masuk Git.

## Staging

Tujuan staging adalah validasi production-like.

Karakteristik:

- Menggunakan domain staging.
- Menggunakan database dan Redis terpisah dari production.
- Mengaktifkan TLS, rate limit, monitoring, dan queue worker.
- OTA/payment/notification menggunakan sandbox credential jika memungkinkan.
- Data testing disiapkan melalui seed terkontrol.

## Production

Tujuan production adalah melayani tenant nyata.

Karakteristik:

- TLS wajib.
- Backup database wajib.
- Monitoring dan alerting wajib.
- Secret dikelola di secret manager atau pipeline protected variable.
- Migration wajib memiliki rollback atau forward-fix strategy.
- Deployment production membutuhkan approval.

## Variables

Variable minimum:

```env
NODE_ENV=production
APP_ENV=production
APP_URL=https://app.channelhub.example
API_URL=https://api.channelhub.example
DATABASE_URL=postgresql://...
REDIS_URL=redis://...
JWT_ACCESS_SECRET=...
JWT_REFRESH_SECRET=...
CORS_ORIGINS=https://app.channelhub.example
LOG_LEVEL=info
OTEL_EXPORTER_OTLP_ENDPOINT=https://otel.example
```

Aturan variable:

- Public frontend variable harus diberi prefix yang eksplisit sesuai framework.
- Secret tidak boleh memakai prefix public.
- Default production tidak boleh mengarah ke localhost.
- Perubahan variable harus tercatat di release note.

## Secrets

Secret wajib diklasifikasikan:

- Authentication secret.
- Database credential.
- OTA provider credential.
- Payment provider credential.
- Notification provider credential.
- Observability token.
- Backup storage credential.

Rotasi secret dilakukan saat onboarding provider, insiden keamanan, perubahan personel kritikal, atau jadwal rotasi berkala.

## Monitoring

Monitoring minimum:

- API latency dan error rate.
- Frontend availability.
- Database connection, slow query, storage usage.
- Redis memory, evictions, queue latency.
- Worker success/failure rate.
- OTA sync error.
- Authentication failure spike.
- Payment callback failure.

Alert production harus memiliki owner, severity, runbook, dan escalation path.

## Deployment Checklist

Sebelum production deploy:

- Pipeline hijau.
- Migration tervalidasi di staging.
- Backup terbaru tersedia.
- Smoke test staging lulus.
- Feature flag siap.
- Rollback plan jelas.
- Monitoring aktif.

## Configuration Ownership

| Configuration | Owner | Storage |
| --- | --- | --- |
| Non-secret app config | Engineering | Repository template atau environment manifest |
| Secret production | DevOps/Security | Secret manager atau protected CI variable |
| OTA credential | Integration owner | Secret manager dengan rotation note |
| Payment credential | Finance/Platform owner | Secret manager dengan restricted access |
| Observability token | DevOps | Secret manager |
| Feature flag | Product/Engineering | Feature flag service atau config table |

## Environment Parity Rule

Development, staging, dan production harus memiliki struktur konfigurasi yang sama:

- Nama variable konsisten.
- Default value aman.
- Perbedaan hanya pada value, capacity, credential, dan provider mode.
- Staging wajib mendekati production untuk TLS, queue, migration, monitoring, dan rate limit.
- Production tidak boleh bergantung pada konfigurasi lokal developer.

## Release Environment Checklist

Sebelum menaikkan perubahan environment:

- Variable baru memiliki dokumentasi purpose.
- Secret baru memiliki owner dan rotation policy.
- Default value tidak membahayakan production.
- Pipeline sudah membaca variable dari source yang benar.
- Dashboard monitoring menampilkan dependency baru.
- Rollback value diketahui.

## Incident Environment Procedure

Jika terjadi insiden environment:

1. Freeze deployment.
2. Identifikasi variable atau secret yang berubah.
3. Bandingkan staging dan production manifest.
4. Rollback value jika aman.
5. Rotasi secret jika ada risiko exposure.
6. Catat incident note di changelog atau runbook.

## Completion Criteria

Environment deployment dianggap siap jika:

- Semua environment memiliki variable matrix yang jelas.
- Secret tidak pernah masuk repository.
- Staging dapat memvalidasi production deployment path.
- Monitoring dan alerting aktif sebelum production traffic dibuka.
- Setiap perubahan configuration memiliki owner dan rollback value.
