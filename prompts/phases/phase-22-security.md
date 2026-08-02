# Phase 22 — Security (Active)

## Phase context

- Docs: `docs/22-security/`
- Standards: `standards/security.md`
- ADR: `adr/ADR-006-multi-tenant-isolation.md`
- Checklists: `checklists/security-review.md`, `checklists/documentation-quality.md`

## Micro-prompts

### MP-001 — Security baseline

- **Doc:** `docs/22-security/001-security-baseline-platform.md`
- **Goal (blueprint):** Pastikan baseline lengkap, cross-link Phase 21 & ADR, tidak kontradiksi `docs/02-product-architecture/009-security-architecture.md`.
- **Goal (app):** N/A di blueprint repo.
- **Validation:** documentation-quality, security-review (architecture section)

### MP-002 — Authentication hardening

- **Doc:** `docs/22-security/002-authentication-hardening.md`
- **Goal:** Selaraskan token/refresh/brute-force dengan `docs/21-integration-deployment/001-frontend-backend-integration.md`.
- **Validation:** security-review

### MP-003 — Authorization & tenant isolation

- **Doc:** `docs/22-security/003-authorization-tenant-isolation.md`
- **Goal:** Verifikasi aturan tenant + RBAC vs product spec & ADR-006; tambah skenario negative test di doc 005 jika missing.
- **Validation:** security-review, testing-release (doc matrix)

### MP-004 — Secrets management

- **Doc:** `docs/22-security/004-secrets-and-credential-management.md`
- **Goal:** Sync dengan env deployment Phase 21; pastikan rotation procedure lengkap.
- **Validation:** deployment-production, security-review

### MP-005 — Security testing & audit

- **Doc:** `docs/22-security/005-security-testing-and-audit.md`
- **Goal:** Hubungkan CI security scan Phase 21 + audit log product spec.
- **Validation:** testing-release, security-review

### MP-006 — Incident runbook

- **Doc:** `docs/22-security/006-incident-response-runbook.md`
- **Goal:** Runbook operasional lengkap; link maintenance checklist & lifecycle/04.
- **Validation:** maintenance-operations, documentation-quality

## Phase completion

Update `.channelhub/STATE.yml` (phase 22 → completed), `CHANGELOG.md`, then plan Phase 23 in roadmap docs.

## Next phase

Setelah MP-001–006 selesai: definisi Phase 23 di `docs/06-implementation-roadmap/` (increment baru) + `prompts/phases/phase-23-*.md`.
