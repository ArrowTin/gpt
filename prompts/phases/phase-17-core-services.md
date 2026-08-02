# Phase 17 — Core Services

## Standards

`standards/backend.md` · `adr/ADR-007-event-driven-integration.md`

## Micro-prompts

| MP | Doc | Blueprint goal | App repo goal |
| --- | --- | --- | --- |
| MP-001 | docs/17-core-services/001-user-service-design.md | Identity boundary | User service slice |
| MP-002 | docs/17-core-services/002-property-service-design.md | Property boundary | Property service slice |
| MP-003 | docs/17-core-services/003-reservation-service-design.md | Lifecycle & transaksi | Reservation transaction |
| MP-004 | docs/17-core-services/004-channel-sync-service-design.md | Orkestrasi sync | Sync job producer |
| MP-005 | docs/17-core-services/005-notification-service-design.md | Notification flow | Notification worker |
| MP-006 | docs/17-core-services/006-payment-service-design.md | Billing boundary | Payment integration |
| MP-007 | docs/17-core-services/007-reporting-service-design.md | Reporting boundary | Report query |
| MP-008 | docs/17-core-services/008-event-driven-architecture.md | Event contract | Event publisher/consumer |

**Validation:** code-review · testing-release

## Rule

Service wajib menegakkan invariant pada `docs/15-database-implementation/002-domain-entity-design.md`; jangan menduplikasi aturan bisnis di controller.
