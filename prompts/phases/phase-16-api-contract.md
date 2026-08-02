# Phase 16 — API Contract

## Standards

`standards/api.md` · `adr/ADR-008-api-first-rest-contract.md` · `contracts/openapi/channelhub.v1.yaml`

## Micro-prompts

| MP | Doc | Blueprint goal | App repo goal |
| --- | --- | --- | --- |
| MP-001 | docs/16-api-contract/001-rest-api-standard.md | REST rules | Controller convention |
| MP-002 | docs/16-api-contract/002-api-response-format.md | Envelope spec | Response interceptor |
| MP-003 | docs/16-api-contract/003-webhook-architecture.md | Webhook rules | Webhook endpoint + HMAC |
| MP-004 | docs/16-api-contract/004-ota-integration-contract.md | OTA contract | Connector client |
| MP-005 | docs/16-api-contract/005-api-authentication-standard.md | Token & tenant rules | Guard chain |
| MP-006 | docs/16-api-contract/006-api-rate-limit-standard.md | Limit policy | Throttler config |
| MP-007 | docs/16-api-contract/007-api-versioning-strategy.md | Version lifecycle | `/api/v1` prefix |
| MP-008 | docs/16-api-contract/008-api-documentation-standard.md | Doc standard | Swagger UI dari spec |
| MP-009 | docs/16-api-contract/009-api-endpoint-specification.md | Sync katalog endpoint dengan `contracts/openapi/channelhub.v1.yaml` | Implement **satu** endpoint sesuai spec |
| MP-010 | docs/16-api-contract/010-error-code-catalog.md | Tambah kode error baru bila ada domain baru | Exception filter + mapping kode |

**Validation:** documentation-quality · code-review · testing-release (contract test)

## Rule

Perubahan endpoint wajib mengubah `contracts/openapi/channelhub.v1.yaml` lebih dulu, baru kode. Spec harus lolos lint OpenAPI sebelum commit.
