# System Context Diagram

## Purpose

Menunjukkan aktor eksternal dan platform ChannelHub.

```mermaid
flowchart TB
  subgraph actors [Actors]
    Guest[Guest / Traveler]
    Staff[Property Staff]
    Admin[Subscriber Admin]
    Super[Super Admin]
    OTA[OTA Partners]
  end

  subgraph channelhub [ChannelHub Platform]
    Web[Next.js Web Apps]
    API[NestJS API / Gateway]
    Workers[Queue Workers]
  end

  subgraph data [Data & Infra]
    PG[(PostgreSQL)]
    Redis[(Redis)]
  end

  Guest --> OTA
  Staff --> Web
  Admin --> Web
  Super --> Web
  Web --> API
  API --> PG
  API --> Redis
  Workers --> Redis
  Workers --> PG
  OTA <-->|Webhooks REST| API
```

## References

- docs/02-product-architecture/001-system-overview.md
- adr/ADR-010-monorepo-blueprint-repository.md
