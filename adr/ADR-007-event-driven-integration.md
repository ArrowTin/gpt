# ADR-007: Event-Driven Integration antar Modul

## Status

Accepted

## Date

2026-08-02

## Context

Reservation, property, OTA sync, notification, and payment perlu decouple agar microservice-ready.

## Decision

Modul berkomunikasi via **domain events** (async) + **REST API** (sync query/command) dengan contract di docs/09-database-api-contract/006-event-contract-design.md.

## Alternatives

- Shared database coupling — ditolak jangka panjang.
- Sync-only HTTP — ditolak: choke point & retry complexity.

## Consequences

- Positive: scaling per service boundary.
- Negative: eventual consistency & debugging lebih kompleks.

## References

- docs/17-core-services/008-event-driven-architecture.md
- diagrams/006-event-flow.md
