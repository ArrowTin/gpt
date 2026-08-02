# Secrets and Credential Management

## Purpose

Standar pengelolaan secret untuk development, staging, dan production.

## Scope

API keys OTA, DB credentials, JWT signing keys, Redis password, CI secrets.

## Context

Environment deployment: docs/21-integration-deployment/004-environment-deployment.md. Docker secrets: docs/21-integration-deployment/002-docker-compose-production.md.

## Rules

- No real secrets in git (including `.env` committed).
- Separate secrets per environment.
- Rotation procedure documented with owner and frequency.
- OTA credentials encrypted at rest; access via least privilege.

## Technical Details

### Classification

| Type | Storage |
| --- | --- |
| JWT signing key | Secret manager / env inject |
| DB URL | Secret manager |
| OTA API key | Tenant-scoped vault table + encryption |
| CI deploy key | GitHub/GitLab secrets |

### Rotation

1. Generate new key
2. Dual-sign or overlap window
3. Deploy consumers
4. Revoke old key
5. Audit log entry

## Impact

- CI: docs/21-integration-deployment/003-ci-cd-pipeline.md security scan
- Maintenance: checklists/maintenance-operations.md

## References

- docs/22-security/001-security-baseline-platform.md
- standards/deployment.md
- adr/ADR-009-configuration-driven-platform.md
