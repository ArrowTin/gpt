# Container & Deployment Diagram

## Purpose

Topology production selaras Phase 21.

```mermaid
flowchart LR
  subgraph edge [Edge]
    RP[Reverse Proxy / TLS]
  end

  subgraph app [Application Tier]
    FE[Next.js]
    BE[NestJS API]
    WK[BullMQ Workers]
  end

  subgraph data [Data Tier]
    PG[(PostgreSQL)]
    RD[(Redis)]
  end

  User --> RP
  RP --> FE
  RP --> BE
  FE --> BE
  BE --> PG
  BE --> RD
  WK --> RD
  WK --> PG
```

## References

- docs/21-integration-deployment/002-docker-compose-production.md
- docs/02-product-architecture/005-deployment-architecture.md
- standards/deployment.md
