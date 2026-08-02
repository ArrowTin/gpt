# Domain Modules Diagram

## Purpose

Bounded context utama ChannelHub.

```mermaid
flowchart TB
  Identity[Identity & Access]
  Org[Organization]
  Property[Property & Inventory]
  Reservation[Reservation]
  OTA[OTA / Channel Sync]
  Notify[Notification]
  Payment[Payment]
  Report[Reporting]

  Identity --> Org
  Org --> Property
  Property --> Reservation
  Property --> OTA
  Reservation --> OTA
  Reservation --> Notify
  Reservation --> Payment
  Property --> Report
  Reservation --> Report
```

## References

- docs/02-product-architecture/002-domain-architecture.md
- docs/02-product-architecture/003-microservice-boundary.md
- adr/ADR-007-event-driven-integration.md
