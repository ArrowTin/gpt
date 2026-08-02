# Incident Response Runbook

## Purpose

Runbook operasional untuk insiden keamanan ChannelHub.

## Scope

Deteksi, containment, eradication, recovery, postmortem.

## Context

Observability: docs/02-product-architecture/006-observability-architecture.md. Security baseline: docs/22-security/001-security-baseline-platform.md.

## Rules

- Severity levels: SEV1 (data breach suspected) … SEV4 (low noise).
- First action: preserve logs and correlation ids.
- Communication template for subscribers affected by tenant scope.

## Procedure

1. **Detect** — alert from failed auth spike, WAF, anomaly monitoring.
2. **Triage** — identify tenant scope, timeline, blast radius.
3. **Contain** — revoke tokens, disable OTA credential, block IP, scale read-only if needed.
4. **Eradicate** — patch vulnerability, rotate secrets (docs/22-security/004-secrets-and-credential-management.md).
5. **Recover** — restore service, validate smoke tests (checklists/deployment-production.md).
6. **Postmortem** — document root cause, ADR if architecture change required.

## Rollback

- Prefer rollback release if exploit tied to recent deploy; otherwise hotfix forward.

## Verification

- Security tests pass; audit log complete for response actions.

## References

- templates/runbook-template.md
- checklists/maintenance-operations.md
- prompts/lifecycle/04-maintenance-increment.md
