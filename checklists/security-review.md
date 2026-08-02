# Security Review Checklist

## Purpose

Review keamanan untuk Phase 22 dan setiap perubahan sensitif.

## Authentication

- [ ] Token lifetime & refresh policy
- [ ] Password / MFA policy jika applicable
- [ ] Brute force protection

## Authorization

- [ ] RBAC + permission engine
- [ ] Tenant isolation verified (ADR-006)
- [ ] Least privilege service accounts

## Data

- [ ] Encryption in transit (TLS)
- [ ] Encryption at rest policy documented
- [ ] PII handling & audit log

## Operations

- [ ] Secret rotation procedure
- [ ] Security scan in CI
- [ ] Incident runbook linked

## References

- docs/22-security/
- standards/security.md
- prompts/phases/phase-22-security.md
