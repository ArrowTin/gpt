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
