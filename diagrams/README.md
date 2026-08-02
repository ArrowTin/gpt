# ChannelHub Architecture Diagrams

## Purpose

Diagram resmi yang diselaraskan dengan `docs/02-product-architecture/` dan fase deploy/security.

## Index

| ID | Diagram | File |
| --- | --- | --- |
| 001 | System context | [001-system-context.md](./001-system-context.md) |
| 002 | Container & deployment | [002-container-deployment.md](./002-container-deployment.md) |
| 003 | Domain modules | [003-domain-modules.md](./003-domain-modules.md) |
| 004 | Authentication flow | [004-auth-flow.md](./004-auth-flow.md) |
| 005 | CI/CD lifecycle | [005-cicd-lifecycle.md](./005-cicd-lifecycle.md) |
| 006 | Event flow | [006-event-flow.md](./006-event-flow.md) |

## Usage

- Perbarui diagram saat ADR Accepted mengubah topology.
- Referensikan dari docs terkait di bagian **References**.
- Prompt sync: `prompts/phases/phase-02-product-architecture.md` micro-prompt diagram review.

## Source of truth order

1. ADR
2. docs/02-product-architecture/
3. diagrams/ (visualisasi)
4. standards/
