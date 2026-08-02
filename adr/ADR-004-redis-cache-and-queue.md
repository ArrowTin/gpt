# ADR-004: Redis untuk Cache dan Queue Backing

## Status

Accepted

## Date

2026-08-02

## Context

ChannelHub membutuhkan session/cache, rate limit backing, dan queue infrastructure untuk sync OTA.

## Decision

**Redis** digunakan untuk cache, distributed lock ringan, dan backing store BullMQ.

## Alternatives

- In-memory only — ditolak: tidak cocok multi-instance production.
- RabbitMQ only — ditolak untuk fase awal; BullMQ + Redis sudah distandardkan.

## Consequences

- Positive: satu komponen infra untuk cache + queue.
- Negative: Redis menjadi dependency kritis.
- Mitigation: healthcheck, persistence policy, docs/21-integration-deployment/002-docker-compose-production.md

## References

- docs/13-backend-foundation/006-redis-cache-queue-foundation.md
- adr/ADR-005-bullmq-job-processing.md
