# CHANNELHUB AI BOOTSTRAP


==================================================

BOOT MODE

Repository adalah Single Source of Truth.

Jangan gunakan isi chat sebagai sumber context.

==================================================

RULES

1.

Jangan melakukan repository scan secara default.

2.

Jangan melakukan directory listing.

3.

Gunakan file yang disebutkan START.md.

4.

Jika file yang disebutkan tidak bisa dibaca,
lanjutkan menggunakan informasi START.md.

5.

Jangan berhenti bekerja hanya karena
repository tidak dapat di-list.

6.

Jika operasi CREATE gagal
karena file sudah ada,
gunakan UPDATE.

7.

Jika UPDATE gagal
karena file tidak ada,
gunakan CREATE.

8.

Jika informasi pada chat
bertentangan dengan repository,
ikuti repository.

==================================================

PROJECT

Name:

ChannelHub Enterprise

Repository:

ArrowTin/gpt

==================================================

ARCHITECTURE

Backend

NestJS

Frontend

NextJS

Database

PostgreSQL

Cache

Redis

Queue

BullMQ

Pattern

DDD

Event Driven

API First

Configuration Driven

==================================================

CURRENT EXECUTION

Phase

21

Milestone

Integration & Deployment

==================================================

EXECUTION PLAN

STEP 1

CREATE

docs/21-integration-deployment/

001-frontend-backend-integration.md

STEP 2

CREATE

002-docker-compose-production.md

STEP 3

CREATE

003-ci-cd-pipeline.md

STEP 4

CREATE

004-environment-deployment.md

STEP 5

UPDATE

.channelhub/STATE.yaml

STEP 6

APPEND

.channelhub/CHANGELOG.md

==================================================

SUCCESS CONDITION

Semua file pada EXECUTION PLAN selesai.

STATE.yaml terupdate.

CHANGELOG.md terupdate.

==================================================

END
