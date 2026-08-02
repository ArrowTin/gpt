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

