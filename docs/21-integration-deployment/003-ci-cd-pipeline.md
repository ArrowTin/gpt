# CI/CD Pipeline

## Overview

CI/CD ChannelHub memastikan setiap perubahan dokumentasi, backend, frontend, infrastructure, dan deployment melewati validasi otomatis sebelum masuk ke production.

## Workflow

```text
Pull Request
  -> Static Check
  -> Test
  -> Security Scan
  -> Build Artifact
  -> Docker Build
  -> Staging Deploy
  -> Smoke Test
  -> Production Approval
  -> Production Deploy
  -> Post Deploy Monitoring
```

## Build

Build pipeline wajib:

- Menginstall dependency dengan lockfile.
- Menjalankan typecheck untuk TypeScript.
- Membuat production build frontend.
- Membuat production build backend.
- Memvalidasi migration database.
- Menyimpan artifact atau image dengan tag immutable.

## Test

Layer test minimum:

- Unit test untuk domain logic dan utility.
- Integration test untuk repository, service, queue, dan API client.
- Contract test untuk frontend-backend DTO dan response format.
- End-to-end smoke test untuk login, dashboard, property list, reservation flow, dan channel sync status.

## Security Scan

Security gate wajib mencakup:

- Dependency vulnerability scan.
- Secret scanning.
- Container image scan.
- Static analysis untuk risky pattern.
- License policy check.
- IaC/Compose misconfiguration scan.

Build gagal jika ditemukan secret aktif, critical vulnerability tanpa waiver, atau image berjalan sebagai root tanpa alasan teknis.

## Docker Build

Docker build wajib:

- Menggunakan multi-stage build.
- Menghasilkan image sekecil mungkin.
- Menjalankan aplikasi dengan non-root user.
- Memiliki healthcheck.
- Menggunakan tag berdasarkan commit SHA dan semantic version bila tersedia.
- Mendorong image ke registry setelah semua quality gate lulus.

## Deploy

Strategi deployment:

- Development: auto deploy dari branch development.
- Staging: auto deploy dari main atau release branch setelah pipeline hijau.
- Production: manual approval dengan changelog, migration note, dan rollback plan.

Langkah deploy:

1. Pull image immutable.
2. Inject environment dan secret.
3. Jalankan migration terkontrol.
4. Start service baru.
5. Jalankan smoke test.
6. Pantau error rate, latency, queue backlog, dan database health.
7. Rollback jika threshold gagal.

## Release Governance

Setiap release wajib memiliki:

- Commit SHA.
- Image tag.
- Migration list.
- Feature flag state.
- Known risk.
- Rollback instruction.
