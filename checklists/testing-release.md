# Testing & Release Checklist

## Purpose

Gate sebelum promote ke staging/production.

## Automated

- [ ] Unit tests pass
- [ ] Integration tests pass untuk path kritis
- [ ] CI pipeline green (docs/21-integration-deployment/003-ci-cd-pipeline.md)

## Scenarios

- [ ] Auth refresh & 401 handling
- [ ] Tenant isolation negative test
- [ ] OTA/webhook retry/idempotency jika disentuh

## Release

- [ ] Changelog / release note
- [ ] Rollback plan documented
- [ ] Migration backward-compatible atau reversible

## References

- standards/testing.md
- prompts/lifecycle/02-testing-increment.md
