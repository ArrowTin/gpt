# Frontend Backend Integration

## Overview

Dokumen ini mendefinisikan pola integrasi antara aplikasi NextJS dan layanan backend NestJS ChannelHub. Integrasi wajib API First, aman untuk multi-tenant, observable, dan siap dipisah menjadi microservice tanpa mengubah kontrak frontend.

## Architecture

```text
Browser
  |
  v
NextJS Web App
  |
  v
API Client Layer
  |
  v
API Gateway / Backend NestJS
  |
  +--> Identity Service
  +--> Property Service
  +--> Reservation Service
  +--> Channel Sync Service
  +--> Notification Service
```

Prinsip integrasi:

- Frontend hanya berkomunikasi melalui API Gateway atau backend facade resmi.
- Kontrak data mengikuti standar response, DTO, versioning, dan authentication pada blueprint API.
- Domain logic tetap berada di backend; frontend hanya mengelola presentasi, form state, route guard, dan client-side cache.
- Semua request membawa correlation id untuk observability end-to-end.

## Authentication Flow

1. User melakukan login dari NextJS auth page.
2. Frontend mengirim credential ke endpoint authentication backend.
3. Backend memvalidasi credential, tenant, role, permission, dan status subscription.
4. Backend mengembalikan access token berdurasi pendek dan refresh token sesuai kebijakan keamanan.
5. Frontend menyimpan token menggunakan mekanisme aman sesuai target deployment.
6. Setiap protected request mengirim `Authorization: Bearer <access_token>`.
7. Jika access token kedaluwarsa, API client melakukan refresh satu kali dan mengulang request awal.
8. Jika refresh gagal, session dihapus dan user diarahkan ke login.

## API Communication

API client frontend wajib memiliki:

- Base URL berbasis environment variable.
- Request interceptor untuk token, tenant id, locale, timezone, dan correlation id.
- Response interceptor untuk normalisasi error.
- Typed DTO atau contract mapping untuk mencegah drift antara frontend dan backend.
- Pagination, filtering, sorting, dan search mengikuti catalog endpoint resmi.

Contoh header minimum:

```http
Authorization: Bearer <access_token>
X-Tenant-Id: <tenant_id>
X-Correlation-Id: <uuid>
Accept: application/json
Content-Type: application/json
```

## Error Handling

Frontend wajib menangani error secara konsisten:

- `400`: tampilkan pesan validasi per field.
- `401`: refresh token atau arahkan ke login.
- `403`: tampilkan halaman akses ditolak.
- `404`: tampilkan state data tidak ditemukan.
- `409`: tampilkan konflik bisnis, misalnya inventory atau reservation conflict.
- `422`: tampilkan kesalahan domain validation.
- `429`: tampilkan retry later dan hormati rate limit.
- `500+`: tampilkan fallback error dan kirim telemetry.

## Retry

Retry hanya boleh dilakukan untuk operasi idempotent:

- GET list/detail.
- Polling status sync.
- Fetch notification/report.

Aturan retry:

- Maksimal 3 percobaan.
- Gunakan exponential backoff.
- Jangan retry otomatis untuk payment, create reservation, channel push, atau mutation non-idempotent kecuali backend menyediakan idempotency key.
- Semua retry harus mempertahankan correlation id yang sama untuk satu logical operation.

## Deployment Notes

- Frontend dan backend dapat dideploy terpisah selama API base URL dan CORS dikonfigurasi benar.
- Production wajib berjalan di belakang reverse proxy atau load balancer.
- Healthcheck frontend memvalidasi kemampuan server render dan asset serving.
- Healthcheck backend memvalidasi database, Redis, queue, dan dependency utama.
- Build artifact frontend tidak boleh menyertakan secret server-side ke bundle client.

## Best Practice

- Gunakan contract test untuk endpoint kritikal.
- Gunakan generated client atau shared DTO package jika monorepo sudah aktif.
- Jangan hardcode endpoint di komponen UI.
- Pisahkan data fetching, state management, dan UI rendering.
- Gunakan feature flag untuk integrasi OTA dan workflow yang belum stabil.
