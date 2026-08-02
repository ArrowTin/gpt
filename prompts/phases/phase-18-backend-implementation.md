# Phase 18 — Backend Implementation

## Standards

`standards/backend.md` · `adr/ADR-001-nestjs-backend-framework.md` · `adr/ADR-005-bullmq-job-processing.md`

## Micro-prompts

| MP | Doc | Blueprint goal | App repo goal |
| --- | --- | --- | --- |
| MP-001 | docs/18-backend-implementation/001-nestjs-project-structure.md | Sync dengan `docs/13-backend-foundation/009-backend-project-structure.md` | Scaffold struktur folder |
| MP-002 | docs/18-backend-implementation/002-module-generation-standard.md | Pola module | Generate satu module |
| MP-003 | docs/18-backend-implementation/003-dto-validation-standard.md | Aturan DTO | DTO dari skema OpenAPI |
| MP-004 | docs/18-backend-implementation/004-service-testing-strategy.md | Strategi test | Unit test satu service |
| MP-005 | docs/18-backend-implementation/005-error-handling-standard.md | Sync dengan error catalog | Exception filter |
| MP-006 | docs/18-backend-implementation/006-logging-standard.md | Struktur log | Logger + correlationId |
| MP-007 | docs/18-backend-implementation/007-configuration-management.md | Aturan konfigurasi | Typed config + validasi env |
| MP-008 | docs/18-backend-implementation/008-background-job-processing.md | Pola job | Satu BullMQ processor |

**Validation:** code-review · testing-release

## Rule

Satu MP = satu module atau satu lintas-potong (cross-cutting) saja; jangan generate seluruh backend dalam satu sesi.
