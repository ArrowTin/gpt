# CHANNELHUB AI EXECUTION CONTRACT

===========================================================

MISSION

===========================================================

Repository:

ArrowTin/gpt

Project:

ChannelHub Enterprise Blueprint

Repository adalah Single Source of Truth.

Chat BUKAN sumber context.

===========================================================

BOOTSTRAP

===========================================================

AI WAJIB melakukan langkah berikut:

1.
Baca README.md pada root repository.

2.
Baca file START.md ini.

3.
Baca .channelhub/STATE.yml untuk fase aktif.

4.
Baca docs/00-foundation/009-global-implementation-rules.md untuk aturan global implementasi (WAJIB).

5.
Baca monitoring.md untuk melihat task numbering dan progress.

6.
Jalankan SATU subtask per sesi sesuai sistem task numbering (DEV-001-A, TEST-001-A, dll).

Sistem task menggunakan 3 level:
- Level 1: DEV-001 (Task utama)
- Level 2: DEV-001-A, DEV-001-B (Subtask)
- Level 3: DEV-001-A-1, DEV-001-A-2 (Sub-subtask jika perlu)

Total ada 175 subtasks yang dibagi menjadi manageable chunks.

Jika AI tidak dapat membaca file selain README.md dan START.md,
lihat bagian FAILSAFE.

===========================================================

AUTHORITY

===========================================================

AI DIBERIKAN KEWENANGAN UNTUK:

✓ membuat file baru

✓ mengubah file

✓ memperbarui dokumen

✓ membuat source code

✓ membuat diagram

✓ membuat docker

✓ membuat CI/CD

✓ membuat testing

✓ membuat infrastructure as code

✓ membuat monitoring setup

✓ membuat operational runbook

SELAMA:

- tidak mengubah arsitektur utama tanpa ADR baru

- mengikuti README

- mengikuti folder docs

- mengikuti milestone aktif

- tidak mengubah kontrak tanpa memperbarui
  contracts/openapi/channelhub.v1.yaml dan
  docs/15-database-implementation/010-postgresql-ddl-reference.md

- semua output source code ditempatkan di folder: channelhub-app/

===========================================================

PROJECT SUMMARY

===========================================================

Nama:

ChannelHub Enterprise

Jenis:

Hospitality Operating Platform

Architecture:

DDD

Event Driven

API First

Configuration Driven

Metadata Driven

Microservice Ready

Multi Tenant

Backend:

NestJS

Frontend:

NextJS

Database:

PostgreSQL

Cache:

Redis

Queue:

BullMQ

Testing:

Jest, Playwright, k6, Lighthouse CI

Deployment:

Docker, Kubernetes, Terraform, AWS

Monitoring:

Datadog, ELK Stack, CloudWatch

===========================================================

CURRENT STATE

===========================================================

Current Phase:

COMPLETE

Current Milestone:

Documentation Enhancement Complete

Status:

READY FOR IMPLEMENTATION

Sumber kebenaran status:

.channelhub/STATE.yml

Documentation Status:

95% - Production Ready

===========================================================

OUTPUT DIRECTORY

===========================================================

Seluruh implementasi source code WAJIB ditempatkan di:

channelhub-app/

Struktur output:
channelhub-app/
├── apps/
│   ├── backend/          # NestJS application
│   └── frontend/         # NextJS application
├── services/             # Microservices (jika diperlukan)
├── packages/             # Shared packages
├── infrastructure/       # Terraform, Docker, K8s configs
├── tests/                # Test files
└── docs/                 # Generated documentation

JANGAN membuat source code di folder lain selain channelhub-app/

===========================================================

CONTRACT ARTIFACT

===========================================================

Lima dokumen berikut adalah kontrak teknis.

Implementasi WAJIB mengikutinya
dan tidak boleh menebak isi yang berbeda:

contracts/openapi/channelhub.v1.yaml

docs/15-database-implementation/009-canonical-erd.md

docs/15-database-implementation/010-postgresql-ddl-reference.md

docs/13-backend-foundation/009-backend-project-structure.md

docs/14-frontend-foundation/009-frontend-project-structure.md

Perubahan kontrak dilakukan pada file kontrak lebih dulu,
baru kode mengikutinya.

===========================================================

REFERENCES

===========================================================

Peta seluruh dokumen:

docs/README.md

Registry prompt per fase:

prompts/index-by-phase.md

Referensi wajib saat implementasi:

README.md

.channelhub/OUTPUT-CONFIG.md (output directory structure)

docs/00-foundation/009-global-implementation-rules.md (WAJIB)

docs/00-foundation/011-documentation-completeness-assessment.md

Pembangunan/Development:
docs/00-foundation/
docs/13-backend-foundation/
docs/14-frontend-foundation/
docs/15-database-implementation/
docs/16-api-contract/
docs/17-core-services/
docs/18-backend-implementation/
docs/19-backend-application/
docs/20-frontend-application/

Testing:
docs/02-product-architecture/010-testing-strategy.md
docs/02-product-architecture/011-test-automation-setup.md
docs/02-product-architecture/012-performance-testing-strategy.md
docs/02-product-architecture/013-test-coverage-implementation.md

Deployment:
docs/12-project-foundation/
docs/21-integration-deployment/005-infrastructure-as-code.md
docs/21-integration-deployment/007-monitoring-alerting-strategy.md

Maintenance:
docs/22-security/007-operational-maintenance-runbook.md
docs/21-integration-deployment/007-monitoring-alerting-strategy.md

Backup & Disaster Recovery:
docs/15-database-implementation/008-database-backup-recovery-standard.md
docs/21-integration-deployment/006-disaster-recovery-plan.md

Security:
docs/22-security/

adr/

standards/

===========================================================

IMPLEMENTATION WORKFLOW

===========================================================

1. PEMBANGUNAN/DEVELOPMENT:
   - Baca foundation documents
   - Baca backend/frontend foundation
   - Baca database implementation
   - Baca API contract
   - Implementasi di channelhub-app/

2. TESTING:
   - Baca test automation setup
   - Baca performance testing strategy
   - Baca test coverage implementation
   - Implementasi test di channelhub-app/tests/

3. DEPLOYMENT:
   - Baca infrastructure as code
   - Baca monitoring & alerting strategy
   - Implementasi di channelhub-app/infrastructure/

4. MAINTENANCE:
   - Baca operational maintenance runbook
   - Baca monitoring & alerting strategy
   - Implementasi monitoring di channelhub-app/

5. BACKUP & DISASTER RECOVERY:
   - Baca backup recovery standard
   - Baca disaster recovery plan
   - Implementasi di channelhub-app/infrastructure/

===========================================================

AI TRIGGER DOCUMENTATION

===========================================================

Dokumen dengan AI Trigger (siap dieksekusi AI):

Foundation (docs/00-foundation/):
- 001-overview.md
- 002-vision.md
- 003-mission.md
- 004-core-principles.md
- 005-architecture-principles.md
- 008-documentation-standard.md
- 009-global-implementation-rules.md (WAJIB)

Backend Foundation (docs/13-backend-foundation/):
- 001-009 (semua dokumen)

Frontend Foundation (docs/14-frontend-foundation/):
- 001-009 (semua dokumen)

Database Implementation (docs/15-database-implementation/):
- 001-010 (semua dokumen)

API Contract (docs/16-api-contract/):
- 009-api-endpoint-specification.md

Testing (docs/02-product-architecture/):
- 011-test-automation-setup.md
- 012-performance-testing-strategy.md
- 013-test-coverage-implementation.md

Deployment (docs/21-integration-deployment/):
- 005-infrastructure-as-code.md
- 006-disaster-recovery-plan.md
- 007-monitoring-alerting-strategy.md

Maintenance (docs/22-security/):
- 007-operational-maintenance-runbook.md

===========================================================

DEFINITION OF DONE

===========================================================

Setiap task dianggap selesai jika:

✓ specification terpenuhi

✓ struktur sesuai CONTRACT ARTIFACT

✓ source code ditempatkan di channelhub-app/

✓ testing terimplementasi dengan coverage target terpenuhi

✓ documentation terupdate

✓ cross reference benar (tidak ada link rusak)

✓ nama tabel, enum, endpoint konsisten dengan CONTRACT ARTIFACT

✓ README tidak dilanggar

===========================================================

UPDATE

===========================================================

Setelah selesai setiap task:

Update:

.channelhub/STATE.yml (jika ada perubahan phase)

Tambahkan entry baru ke:

.channelhub/CHANGELOG.md

===========================================================

FAILSAFE

===========================================================

Jika AI tidak dapat:

- scan repository

- list directory

- membaca file selain README dan START.md

MAKA:

Gunakan START.md sebagai working context.

JANGAN berhenti bekerja.

Kerjakan HANYA satu task pada AI Trigger yang relevan.

Tandai asumsi yang dipakai pada bagian akhir dokumen
yang dihasilkan agar dapat diverifikasi kemudian.

CATATAN:

Failsafe adalah kondisi darurat.

Pada kondisi normal AI WAJIB membaca
CONTRACT ARTIFACT sebelum menulis kode,
karena skema database dan kontrak API
TIDAK boleh ditebak.

END
