# ADR-005: BullMQ untuk Background Job Processing

## Status

Accepted

## Date

2026-08-02

## Context

OTA sync, notification, reporting, dan retry membutuhkan job queue dengan observability dan idempotency.

## Decision

**BullMQ** sebagai job processor standar di NestJS worker.

## Alternatives

- Cron-only — ditolak: tidak scalable untuk retry OTA.
- Custom queue — ditolak: maintenance cost.

## Consequences

- Positive: selaras docs/19-backend-application/008-queue-worker-implementation.md
- Negative: wajib desain idempotency & DLQ policy.

## References

- docs/18-backend-implementation/008-background-job-processing.md
- checklists/deployment-production.md
