# API Standard (Operational)

## Purpose

Ringkasan wajib untuk desain dan review API ChannelHub.

## Rules

- **Kontrak mesin (sumber kebenaran):** [contracts/openapi/channelhub.v1.yaml](../contracts/openapi/channelhub.v1.yaml)
- Katalog endpoint: [docs/16-api-contract/009-api-endpoint-specification.md](../docs/16-api-contract/009-api-endpoint-specification.md)
- Kode error: [docs/16-api-contract/010-error-code-catalog.md](../docs/16-api-contract/010-error-code-catalog.md)
- REST + envelope response: [docs/16-api-contract/002-api-response-format.md](../docs/16-api-contract/002-api-response-format.md)
- Auth & tenant: [docs/16-api-contract/005-api-authentication-standard.md](../docs/16-api-contract/005-api-authentication-standard.md)
- Versioning: [docs/16-api-contract/007-api-versioning-strategy.md](../docs/16-api-contract/007-api-versioning-strategy.md)
- Rate limit: [docs/16-api-contract/006-api-rate-limit-standard.md](../docs/16-api-contract/006-api-rate-limit-standard.md)
- Webhook OTA: [docs/16-api-contract/003-webhook-architecture.md](../docs/16-api-contract/003-webhook-architecture.md)
- OpenAPI: [docs/16-api-contract/008-api-documentation-standard.md](../docs/16-api-contract/008-api-documentation-standard.md)

## Testing

- Contract test untuk breaking change.
- Checklist: [checklists/testing-release.md](../checklists/testing-release.md)

## ADR

- [ADR-008-api-first-rest-contract.md](../adr/ADR-008-api-first-rest-contract.md)
