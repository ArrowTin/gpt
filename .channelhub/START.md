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
Baca docs/00-foundation/009-global-implementation-rules.md untuk aturan global implementasi.

5.
Baca dokumen pada bagian REFERENCES di bawah.

6.
Jalankan SATU micro-prompt per sesi
sesuai prompts/phases/phase-{fase}-*.md.

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

SELAMA:

- tidak mengubah arsitektur utama tanpa ADR baru

- mengikuti README

- mengikuti folder docs

- mengikuti milestone aktif

- tidak mengubah kontrak tanpa memperbarui
  contracts/openapi/channelhub.v1.yaml dan
  docs/15-database-implementation/010-postgresql-ddl-reference.md

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

===========================================================

CURRENT STATE

===========================================================

Current Phase:

22

Current Milestone:

Security

Status:

RUNNING

Sumber kebenaran status:

.channelhub/STATE.yml

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

docs/02-product-architecture/

docs/13-backend-foundation/

docs/14-frontend-foundation/

docs/15-database-implementation/

docs/16-api-contract/

docs/17-core-services/

docs/19-backend-application/

docs/20-frontend-application/

docs/21-integration-deployment/

docs/22-security/

adr/

standards/

===========================================================

DELIVERABLES PHASE 22

===========================================================

001-security-baseline-platform.md

002-authentication-hardening.md

003-authorization-tenant-isolation.md

004-secrets-and-credential-management.md

005-security-testing-and-audit.md

006-incident-response-runbook.md

Seluruh berkas berada pada docs/22-security/.

Micro-prompt per berkas:

prompts/phases/phase-22-security.md

===========================================================

DEFINITION OF DONE

===========================================================

Milestone dianggap selesai jika:

✓ seluruh deliverables selesai

✓ seluruh specification terpenuhi

✓ struktur konsisten

✓ cross reference benar (tidak ada link rusak)

✓ nama tabel, enum, endpoint konsisten dengan CONTRACT ARTIFACT

✓ README tidak dilanggar

===========================================================

UPDATE

===========================================================

Setelah selesai:

Update:

.channelhub/STATE.yml

Tambahkan entry baru ke

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

Kerjakan HANYA satu deliverable pada bagian
DELIVERABLES PHASE 22.

Tandai asumsi yang dipakai pada bagian akhir dokumen
yang dihasilkan agar dapat diverifikasi kemudian.

CATATAN:

Failsafe adalah kondisi darurat.

Pada kondisi normal AI WAJIB membaca
CONTRACT ARTIFACT sebelum menulis kode,
karena skema database dan kontrak API
TIDAK boleh ditebak.

END
