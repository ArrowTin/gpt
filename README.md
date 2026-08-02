# ChannelHub Enterprise Blueprint

> Single Source of Truth untuk membangun ekosistem ChannelHub.

## Overview

Repository ini menjadi pusat dokumentasi resmi pembangunan ChannelHub.

ChannelHub bukan hanya aplikasi channel manager, tetapi Hospitality Operating Platform yang menghubungkan properti, OTA, reservasi, distribusi channel, pembayaran, analitik, automation, dan AI.

## Core Principles

- Configuration Driven Architecture
- Metadata Driven Platform
- Domain Driven Design
- Modular Architecture
- Microservice Ready
- API First
- Event Driven Architecture
- Multi Tenant
- White Label Ready
- Observability First
- Security by Design
- AI Assisted Development

## Architecture Philosophy

Hal yang dapat berubah harus menjadi konfigurasi.

Contoh:

- Landing page
- Menu
- Dashboard
- Role
- Permission
- Subscription
- Pricing
- Workflow
- Notification
- Connector OTA
- Report

## Development Flow

```
Business Requirement
        ↓
Blueprint
        ↓
Architecture Decision
        ↓
Implementation
        ↓
Testing
        ↓
Deployment
        ↓
Monitoring
```

## Repository

```
/docs        Business, Product, Architecture
/adr         Architecture Decision Record
/prompts     AI Development Prompt
/standards   Engineering Standard
/templates   Document Template
/diagrams    Architecture Diagram
/checklists  Quality Checklist
/contracts   Machine Readable Contract (OpenAPI)
```

## Contract Artifact

Kontrak teknis yang wajib diikuti implementasi dan tidak boleh ditebak:

- `contracts/openapi/channelhub.v1.yaml`
- `docs/15-database-implementation/009-canonical-erd.md`
- `docs/15-database-implementation/010-postgresql-ddl-reference.md`
- `docs/13-backend-foundation/009-backend-project-structure.md`
- `docs/14-frontend-foundation/009-frontend-project-structure.md`

## Cara Menjalankan Blueprint Ini ke Vibe Code

Dokumentasi ini adalah dokumentasi hidup: dieksekusi **per modul/task**, bukan sekali jalan. Satu sesi AI = satu increment, dan increment berikutnya baru dimulai setelah test increment sebelumnya hijau.

### Persiapan sekali saja

1. Siapkan repository aplikasi terpisah (monorepo `apps/backend` NestJS + `apps/frontend` Next.js). Repository ini tetap berisi dokumentasi saja.
2. Beri AI coding agent akses ke kedua repository, atau salin folder `contracts/`, `docs/`, `standards/`, `adr/` ke dalam repository aplikasi.
3. Siapkan Docker Compose untuk PostgreSQL dan Redis sesuai `docs/12-project-foundation/004-docker-infrastructure-design.md`.

### Siklus per task

```
Pilih task  →  Tempel konteks  →  Setujui rencana file  →  Implementasi
                                                              ↓
                       Deploy  ←   Review   ←   Test   ←   Commit/PR
```

| Langkah | Yang kamu lakukan | File rujukan |
| --- | --- | --- |
| 1. Pilih task | Ambil **satu** micro-prompt (MP) sesuai urutan increment | `docs/06-implementation-roadmap/008-core-service-development-order.md`, `prompts/index-by-phase.md` |
| 2. Tempel konteks | Master prompt + satu MP tersebut | `docs/05-ai-development-blueprint/010-final-vibe-code-master-prompt.md` |
| 3. Rencana file | Minta daftar file yang akan dibuat/diubah, periksa, baru setujui | `docs/05-ai-development-blueprint/007-module-execution-template.md` |
| 4. Implementasi | Kode + migration + DTO mengikuti kontrak | `contracts/openapi/channelhub.v1.yaml`, `docs/15-database-implementation/010-postgresql-ddl-reference.md` |
| 5. Test | Unit, integrasi, uji negatif tenant, contract test | `prompts/lifecycle/02-testing-increment.md`, `docs/22-security/005-security-testing-and-audit.md` |
| 6. Review | Jalankan checklist sebelum merge | `checklists/code-review.md`, `checklists/security-review.md` |
| 7. Deploy | Build image, migrasi, smoke test per environment | `prompts/lifecycle/03-deployment-increment.md`, `docs/21-integration-deployment/` |
| 8. Catat | Perbarui status dan changelog | `.channelhub/STATE.yml`, `.channelhub/CHANGELOG.md` |

### Urutan increment (ringkas)

```
1  Fondasi: config, database, observability, health
2  Cross-cutting: guard, interceptor, exception filter, envelope
3  Identity: user, role, permission, session
4  Organization: tenant, membership, setting
5  Property: property, room type, room
6  Inventory & rate
7  Reservation (transaksional, anti oversell)
8  OTA: koneksi, mapping, sync job, webhook
9  Subscription & billing
10 Notification & platform (menu, audit, feature flag)
11 Frontend: shell, auth, lalu feature mengikuti 5–10
12 Integrasi & deployment
```

Rincian prasyarat, migration, dan endpoint tiap increment ada di `docs/06-implementation-roadmap/008-core-service-development-order.md`.

### Aturan yang membuat hasilnya rapi

- Satu task per sesi. Perintah "kerjakan semuanya" menghasilkan kode tebakan.
- AI wajib membaca Contract Artifact sebelum menulis kode; jangan biarkan ia mengarang tabel atau endpoint.
- Perubahan perilaku dimulai dari file kontrak, baru kode.
- Increment tidak dianggap selesai sebelum test hijau dan checklist review terisi.
- Keputusan arsitektur baru dihentikan dan diangkat menjadi ADR di `adr/`.

Penjelasan lengkap alur ini ada di `docs/05-ai-development-blueprint/002-vibe-code-workflow.md`.

## Status

Phase: 22 — Security (lihat `.channelhub/STATE.yml`)

Project: ChannelHub Enterprise Blueprint

# AI ENTRY

Seluruh AI Agent (GPT, Claude, Gemini, Cursor, Copilot, Codex, OpenHands, dll)

WAJIB memulai dari

.channelhub/START.md

START.md merupakan AI Execution Contract.

START.md mengandung informasi minimum untuk melanjutkan project dan menunjuk seluruh kontrak teknis.

Sebelum menulis kode, AI WAJIB membaca Contract Artifact di atas. Skema database dan kontrak API TIDAK boleh ditebak.

AI TIDAK BOLEH menghentikan pekerjaan hanya karena:

- tidak bisa list directory

- tidak bisa scan repository

- tidak bisa membaca file lain

Jika kondisi tersebut terjadi,

AI WAJIB menggunakan START.md sebagai working context, mengerjakan satu deliverable saja, dan menandai asumsi yang dipakai.

