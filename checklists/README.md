# ChannelHub Quality Checklists

## Purpose

Gate kualitas untuk development, testing, deployment, security, maintenance, dan dokumentasi. Selaras dengan `docs/05-ai-development-blueprint/006-ai-review-checklist.md`.

## Index

| Checklist | When | File |
| --- | --- | --- |
| Development ready | Sebelum merge feature | [development-ready.md](./development-ready.md) |
| Code review | Review PR / output AI | [code-review.md](./code-review.md) |
| Testing & release | Sebelum release staging/prod | [testing-release.md](./testing-release.md) |
| Deployment production | Sebelum go-live | [deployment-production.md](./deployment-production.md) |
| Security review | Setiap auth/tenant/secret change | [security-review.md](./security-review.md) |
| Maintenance operations | Rutin / post-incident | [maintenance-operations.md](./maintenance-operations.md) |
| Documentation quality | Setelah update docs/prompts | [documentation-quality.md](./documentation-quality.md) |

## Usage with prompts

Setiap micro-prompt di `prompts/phases/` wajib menyebut checklist validasi di bagian **Validation**.

## Lifecycle map

```text
development-ready → code-review → testing-release → deployment-production
security-review (parallel) → maintenance-operations (post-deploy)
documentation-quality (blueprint repo)
```
