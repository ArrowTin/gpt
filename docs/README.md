# ChannelHub Documentation Index

## Purpose

Peta resmi seluruh dokumentasi blueprint ChannelHub Enterprise. Semua folder di root repository (`/docs`, `/adr`, `/prompts`, `/standards`, `/templates`, `/diagrams`, `/checklists`) saling merujuk dan tidak boleh bertentangan.

## Scope

Repository ini adalah **Single Source of Truth** untuk business, product, architecture, standar engineering, prompt AI, dan checklist kualitas. Implementasi source code dilakukan di repository aplikasi terpisah dengan mengikuti blueprint ini.

## Contract Artifact

Lima dokumen berikut adalah kontrak teknis yang **tidak boleh ditebak** saat implementasi. Kode mengikuti kontrak, bukan sebaliknya.

| Artefak | Isi |
| --- | --- |
| [contracts/openapi/channelhub.v1.yaml](../contracts/openapi/channelhub.v1.yaml) | Seluruh endpoint REST v1, request/response schema |
| [15-database-implementation/009-canonical-erd.md](./15-database-implementation/009-canonical-erd.md) | Daftar tabel kanonik, relasi, enum status |
| [15-database-implementation/010-postgresql-ddl-reference.md](./15-database-implementation/010-postgresql-ddl-reference.md) | DDL eksekusi, index, urutan migration |
| [13-backend-foundation/009-backend-project-structure.md](./13-backend-foundation/009-backend-project-structure.md) | Struktur file NestJS, peta module ↔ tabel |
| [14-frontend-foundation/009-frontend-project-structure.md](./14-frontend-foundation/009-frontend-project-structure.md) | Struktur file Next.js, peta route ↔ endpoint |

## Reading Order

1. [README.md](../README.md) — prinsip dan alur development.
2. [.channelhub/START.md](../.channelhub/START.md) — kontrak eksekusi AI dan fase aktif.
3. [.channelhub/STATE.yml](../.channelhub/STATE.yml) — phase, milestone, completed phases.
4. [00-foundation/009-global-implementation-rules.md](./00-foundation/009-global-implementation-rules.md) — aturan global implementasi (WAJIB dibaca sebelum task apapun).
5. [00-foundation/001-overview.md](./00-foundation/001-overview.md) — fondasi proyek.
6. [05-ai-development-blueprint/002-vibe-code-workflow.md](./05-ai-development-blueprint/002-vibe-code-workflow.md) — vibe code bertahap.
7. [prompts/README.md](../prompts/README.md) — micro-prompt per increment.

## Phase Map

| Phase | Folder | Fokus | Prompt |
| --- | --- | --- | --- |
| 00 | [00-foundation](./00-foundation/) | Visi, prinsip, glossary, ADR system | [phase-00](../prompts/phases/phase-00-foundation.md) |
| 01 | [01-business](./01-business/) | Business model, stakeholder, revenue | [phase-01](../prompts/phases/phase-01-business.md) |
| 02 | [02-product-architecture](./02-product-architecture/) | System, domain, deployment, security | [phase-02](../prompts/phases/phase-02-product-architecture.md) |
| 03 | [03-product-specification](./03-product-specification/) | Fitur produk, role, workflow | [phase-03](../prompts/phases/phase-03-product-specification.md) |
| 04 | [04-technical-blueprint](./04-technical-blueprint/) | Stack, API, DB, observability | [phase-04](../prompts/phases/phase-04-technical-blueprint.md) |
| 05 | [05-ai-development-blueprint](./05-ai-development-blueprint/) | AI workflow, prompt standard | [phase-05](../prompts/phases/phase-05-ai-development.md) |
| 06 | [06-implementation-roadmap](./06-implementation-roadmap/) | Milestone, bootstrap order | [phase-06](../prompts/phases/phase-06-implementation-roadmap.md) |
| 07 | [07-source-code-generation](./07-source-code-generation/) | Pola generasi kode (repo app) | [phase-07](../prompts/phases/phase-07-source-code-generation.md) |
| 08 | [08-core-application-modules](./08-core-application-modules/) | Desain modul inti | [phase-08](../prompts/phases/phase-08-core-modules.md) |
| 09 | [09-database-api-contract](./09-database-api-contract/) | Schema & kontrak API | [phase-09](../prompts/phases/phase-09-database-api-contract.md) |
| 10 | [10-ai-agent-orchestration](./10-ai-agent-orchestration/) | Peran agent AI | [phase-10](../prompts/phases/phase-10-ai-agent-orchestration.md) |
| 11 | [11-autonomous-development-workflow](./11-autonomous-development-workflow/) | Lifecycle otonom | [phase-11](../prompts/phases/phase-11-autonomous-workflow.md) |
| 12 | [12-project-foundation](./12-project-foundation/) | Monorepo, Docker, CI dasar | [phase-12](../prompts/phases/phase-12-project-foundation.md) |
| 13 | [13-backend-foundation](./13-backend-foundation/) | NestJS foundation | [phase-13](../prompts/phases/phase-13-backend-foundation.md) |
| 14 | [14-frontend-foundation](./14-frontend-foundation/) | Next.js foundation | [phase-14](../prompts/phases/phase-14-frontend-foundation.md) |
| 15 | [15-database-implementation](./15-database-implementation/) | PostgreSQL implementasi | [phase-15](../prompts/phases/phase-15-database-implementation.md) |
| 16 | [16-api-contract](./16-api-contract/) | REST, auth, webhook, OTA API | [phase-16](../prompts/phases/phase-16-api-contract.md) |
| 17 | [17-core-services](./17-core-services/) | Service design | [phase-17](../prompts/phases/phase-17-core-services.md) |
| 18 | [18-backend-implementation](./18-backend-implementation/) | NestJS patterns | [phase-18](../prompts/phases/phase-18-backend-implementation.md) |
| 19 | [19-backend-application](./19-backend-application/) | Modul backend aplikasi | [phase-19](../prompts/phases/phase-19-backend-application.md) |
| 20 | [20-frontend-application](./20-frontend-application/) | Modul UI aplikasi | [phase-20](../prompts/phases/phase-20-frontend-application.md) |
| 21 | [21-integration-deployment](./21-integration-deployment/) | Integrasi & deploy | [phase-21](../prompts/phases/phase-21-integration-deployment.md) |
| 22 | [22-security](./22-security/) | Security hardening (aktif) | [phase-22](../prompts/phases/phase-22-security.md) |

## Cross-Repository Links

| Folder | Isi | Standar penulisan |
| --- | --- | --- |
| [adr/](../adr/) | Keputusan arsitektur resmi | [templates/adr-template.md](../templates/adr-template.md) |
| [standards/](../standards/) | Ringkasan engineering standard | [00-foundation/008-documentation-standard.md](./00-foundation/008-documentation-standard.md) |
| [prompts/](../prompts/) | Micro-prompt vibe code | [05-ai-development-blueprint/003-prompt-engineering-standard.md](./05-ai-development-blueprint/003-prompt-engineering-standard.md) |
| [templates/](../templates/) | Template dokumen & prompt | — |
| [diagrams/](../diagrams/) | Diagram arsitektur | — |
| [checklists/](../checklists/) | Quality gate | [05-ai-development-blueprint/006-ai-review-checklist.md](./05-ai-development-blueprint/006-ai-review-checklist.md) |
| [contracts/](../contracts/) | Kontrak machine-readable (OpenAPI) | [16-api-contract/008-api-documentation-standard.md](./16-api-contract/008-api-documentation-standard.md) |

## Lifecycle Consistency

Seluruh fase mengikuti alur yang sama:

```text
Business / Product (01–03)
  → Architecture (02, 04, 08, 17)
  → Contract (09, 16)
  → Foundation (12–15)
  → Application (18–20)
  → Integration & Deploy (21)
  → Security & Hardening (22)
  → Testing & Maintenance (checklists + lifecycle prompts)
```

## Rules

- Perubahan arsitektur besar wajib [ADR](../adr/README.md).
- Prompt tidak boleh membangun seluruh aplikasi dalam satu perintah; gunakan [micro-prompt](../prompts/README.md).
- Setelah menyelesaikan milestone, update [.channelhub/STATE.yml](../.channelhub/STATE.yml) dan [.channelhub/CHANGELOG.md](../.channelhub/CHANGELOG.md).

## References

- [Architecture principles](./00-foundation/005-architecture-principles.md)
- [Development milestone plan](./06-implementation-roadmap/009-development-milestone-plan.md)
- [Testing strategy](./02-product-architecture/010-testing-strategy.md)
- [Deployment architecture](./02-product-architecture/005-deployment-architecture.md)
