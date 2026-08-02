# Docker Compose Production Blueprint

## Overview

Docker Compose production digunakan sebagai baseline deployment ChannelHub untuk environment kecil sampai menengah. Blueprint ini menjadi referensi sebelum platform dipindahkan ke orchestrator seperti Kubernetes.

## Services

Service minimum:

- `reverse-proxy`: terminasi TLS, routing, compression, dan security header.
- `frontend`: NextJS production server.
- `api-gateway`: NestJS API gateway atau backend facade.
- `worker`: BullMQ worker untuk background job dan channel sync.
- `postgres`: database utama.
- `redis`: cache, session coordination, queue broker.
- `observability`: log aggregation, metrics, dan tracing collector sesuai kapasitas deployment.

## Networks

Gunakan network terpisah:

- `public`: hanya reverse proxy yang terekspos ke internet.
- `app`: komunikasi reverse proxy, frontend, dan backend.
- `data`: komunikasi backend, worker, PostgreSQL, dan Redis.

Aturan:

- Database dan Redis tidak boleh berada di network publik.
- Frontend tidak boleh mengakses database langsung.
- Worker hanya mengekspos healthcheck internal.

## Volumes

Volume minimum:

- `postgres_data`: penyimpanan data PostgreSQL.
- `redis_data`: persistence Redis jika diaktifkan.
- `proxy_certs`: sertifikat TLS.
- `app_logs`: log aplikasi jika log belum sepenuhnya dikirim ke collector.
- `backup_data`: hasil backup terjadwal sebelum dipindahkan ke object storage.

## Secrets

Secret production tidak boleh ditulis langsung di file compose. Gunakan secret manager, file secret terenkripsi, atau environment injection dari pipeline.

Secret wajib:

- Database password.
- JWT signing secret.
- Refresh token secret.
- OTA credential.
- Payment provider key.
- SMTP/API notification credential.
- Observability exporter token.

## Reverse Proxy

Reverse proxy bertanggung jawab untuk:

- TLS termination.
- Redirect HTTP ke HTTPS.
- Routing `/` ke frontend.
- Routing `/api` ke backend.
- Rate limit dasar.
- Security header.
- Request body limit.
- Webhook route exception jika diperlukan OTA provider.

## Scaling

Scaling awal:

- `frontend`: 2 replica.
- `api-gateway`: 2 replica.
- `worker`: 1 sampai N replica berdasarkan queue backlog.
- `postgres`: single primary dengan backup dan recovery plan.
- `redis`: single instance untuk Compose baseline; gunakan managed Redis untuk production kritikal.

Gunakan sticky session hanya bila diperlukan. Prefer stateless API dengan token dan shared Redis.

## Healthcheck

Healthcheck minimum:

- Frontend: `GET /health`.
- Backend: `GET /api/health`.
- Worker: queue heartbeat atau worker health endpoint internal.
- PostgreSQL: `pg_isready`.
- Redis: `redis-cli ping`.

Container dianggap ready hanya jika dependency wajib tersedia. Liveness tidak boleh terlalu agresif agar tidak membuat restart loop saat migration atau cold start.
