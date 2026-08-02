# Authentication Hardening

## Purpose

Memperketat authentication end-to-end selaras integrasi frontend-backend.

## Scope

Login, token issuance, refresh, session revocation, dan proteksi brute force.

## Context

Alur dasar ada di docs/21-integration-deployment/001-frontend-backend-integration.md dan docs/16-api-contract/005-api-authentication-standard.md.

## Rules

- Access token berdurasi pendek; refresh token rotatable.
- Refresh hanya via endpoint dedicated; satu retry policy di API client.
- Failed login rate limited per IP + account (Redis-backed).
- Password policy configurable via metadata (super admin).
- MFA hooks documented for future phase; placeholder interface allowed in docs only.

## Technical Details

### Token claims minimum

- `sub`, `tenant_id`, `roles`, `permissions` (or permission version)
- `jti` for revocation tracking

### Frontend storage

- Production: httpOnly cookie atau secure storage pattern per deployment doc Phase 21.
- Never store refresh token in localStorage for production baseline.

### Backend

- Constant-time comparison for secrets.
- Audit log entry for login success/failure (no password in logs).

## Impact

- Prompt: prompts/phases/phase-22-security.md → micro-prompt 002
- Checklist: checklists/security-review.md

## References

- docs/22-security/001-security-baseline-platform.md
- diagrams/004-auth-flow.md
- standards/api.md
