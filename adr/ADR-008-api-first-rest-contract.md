# ADR-008: API First dengan REST Contract

## Status

Accepted

## Date

2026-08-02

## Context

Frontend, partner OTA, webhook, dan future mobile clients harus share kontrak stabil.

## Decision

**API First**: spesifikasi REST, response envelope, versioning, dan OpenAPI wajib sebelum implementasi feature besar.

## Alternatives

- Code-first tanpa doc — ditolak.
- GraphQL primary — ditolak untuk fase blueprint (REST sudah di seluruh docs/16).

## Consequences

- Positive: vibe code incremental per endpoint group.
- Negative: overhead dokumentasi — mitigasi via templates & prompts.

## References

- docs/16-api-contract/001-rest-api-standard.md
- standards/api.md
