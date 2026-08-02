# Event Flow Diagram

## Purpose

Integrasi event-driven antar modul.

```mermaid
flowchart LR
  RS[Reservation Service]
  PS[Property Service]
  OS[OTA Sync Service]
  NS[Notification Service]
  Bus[Event Bus / Queue]

  RS -->|ReservationCreated| Bus
  Bus --> NS
  RS -->|InventoryChanged| Bus
  Bus --> PS
  PS -->|AvailabilityUpdated| Bus
  Bus --> OS
  OS -->|SyncCompleted| Bus
  Bus --> RS
```

## References

- docs/09-database-api-contract/006-event-contract-design.md
- docs/17-core-services/008-event-driven-architecture.md
- adr/ADR-007-event-driven-integration.md
