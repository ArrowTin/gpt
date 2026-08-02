# Phase 20 — Frontend Application

## Standards

`standards/frontend.md` · `adr/ADR-002-nextjs-frontend-framework.md`

## Micro-prompts

| MP | Doc | Blueprint goal | App repo goal |
| --- | --- | --- | --- |
| MP-001 | docs/20-frontend-application/001-nextjs-bootstrap.md | Urutan bootstrap | Scaffold `apps/frontend` |
| MP-002 | docs/20-frontend-application/002-ui-component-system.md | Design system | `components/ui` primitif |
| MP-003 | docs/20-frontend-application/003-authentication-ui-flow.md | Alur login & refresh | Halaman login + proteksi route |
| MP-004 | docs/20-frontend-application/004-dashboard-layout.md | Shell & menu dinamis | `AppShell` dari `GET /menus` |
| MP-005 | docs/20-frontend-application/005-property-ui-module.md | UI property | Feature `properties` |
| MP-006 | docs/20-frontend-application/006-reservation-ui-module.md | UI reservasi | Feature `reservations` |
| MP-007 | docs/20-frontend-application/007-channel-sync-dashboard.md | UI channel | Feature `channels` |
| MP-008 | docs/20-frontend-application/008-state-management-standard.md | Aturan state | Query cache + store minimal |

**Validation:** code-review · testing-release

## Rule

Tipe API digenerate dari `contracts/openapi/channelhub.v1.yaml`; struktur file mengikuti `docs/14-frontend-foundation/009-frontend-project-structure.md`. Menu dan permission tidak boleh hardcode.
