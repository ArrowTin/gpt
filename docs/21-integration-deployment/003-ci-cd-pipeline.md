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

## Branch and Environment Mapping

| Branch/Trigger | Target | Rule |
| --- | --- | --- |
| Pull request | Validation only | Tidak deploy production |
| `develop` | Development | Auto deploy jika quality gate lulus |
| `main` | Staging | Auto deploy staging setelah build dan test lulus |
| Release tag | Production | Butuh approval manual dan rollback plan |
| Hotfix tag | Production | Butuh approval manual, smoke test, dan postmortem note |

## Quality Gate

Pipeline wajib gagal jika:

- Typecheck gagal.
- Unit atau integration test gagal.
- Contract test gagal untuk endpoint protected.
- Secret terdeteksi di diff.
- Critical vulnerability tidak memiliki waiver.
- Docker image tidak memiliki healthcheck.
- Migration tidak dapat dijalankan di staging.

## Migration Gate

Migration database harus melewati tahapan:

1. Validate migration syntax.
2. Run migration pada database ephemeral atau staging snapshot.
3. Jalankan smoke test domain terkait.
4. Catat rollback atau forward-fix plan.
5. Jalankan production migration hanya pada deploy window yang disetujui.

## Observability Gate

Setelah deploy, pipeline atau operator wajib memantau:

- Error rate API.
- Latency p95/p99.
- Frontend availability.
- Queue backlog dan failed jobs.
- Database connection dan slow query.
- Authentication failure spike.
- OTA sync failure spike.

## Rollback Rule

Rollback boleh dilakukan jika:

- Smoke test production gagal.
- Error rate melewati threshold.
- Migration tidak menyentuh perubahan destructive yang tidak reversible.
- Feature flag dapat dimatikan untuk mengisolasi perubahan.

Jika migration sudah irreversible, gunakan forward-fix dengan approval incident commander.

## Completion Criteria

CI/CD dianggap siap jika:

- Semua quality gate otomatis berjalan pada pull request.
- Staging deploy dapat dilakukan tanpa langkah manual.
- Production deploy membutuhkan approval dan menghasilkan release note.
- Image tag, commit SHA, migration, dan rollback plan tercatat.
- Post-deploy monitoring menjadi bagian dari release process.
