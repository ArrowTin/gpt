# ChannelHub AI Prompt Library

## Purpose

Micro-prompt untuk **vibe code bertahap**: satu prompt = satu increment kecil, konteks global ChannelHub tetap sama dari development → testing → deployment → maintenance.

## Repository mode

| Repo | Peran prompt |
| --- | --- |
| **Blueprint (ArrowTin/gpt)** | Lengkapi/sync docs, adr, standards, diagrams, checklists |
| **Application (monorepo app)** | Implementasi code mengacu docs + standards + ADR |

Di blueprint repo: **jangan** generate source code aplikasi; fokus dokumentasi dan prompt.

## Start here

1. [000-session-bootstrap.md](./000-session-bootstrap.md) — setiap sesi AI
2. [.channelhub/START.md](../.channelhub/START.md) — kontrak fase aktif
3. [index-by-phase.md](./index-by-phase.md) — registry lengkap doc ↔ prompt
4. Satu file phase: [phases/](./phases/) → jalankan **satu** micro-prompt per sesi

## Lifecycle prompts (lintas fase)

| File | Fokus |
| --- | --- |
| [01-development-increment.md](./lifecycle/01-development-increment.md) | Implementasi / doc increment |
| [02-testing-increment.md](./lifecycle/02-testing-increment.md) | Test & validasi |
| [03-deployment-increment.md](./lifecycle/03-deployment-increment.md) | Deploy & env |
| [04-maintenance-increment.md](./lifecycle/04-maintenance-increment.md) | Ops & incident |

## Phase prompts

| Phase | File |
| --- | --- |
| 00–22 | [phases/phase-NN-*.md](./phases/) |

## Legacy

- [channelhub-integration-deployment-master-prompt.md](./channelhub-integration-deployment-master-prompt.md) — **deprecated**: gunakan [phase-21-integration-deployment.md](./phases/phase-21-integration-deployment.md) micro-prompts per dokumen.

## Template

[templates/prompt-micro-template.md](../templates/prompt-micro-template.md)

## Standards

- [standards/ai-development.md](../standards/ai-development.md)
- [docs/05-ai-development-blueprint/003-prompt-engineering-standard.md](../docs/05-ai-development-blueprint/003-prompt-engineering-standard.md)
