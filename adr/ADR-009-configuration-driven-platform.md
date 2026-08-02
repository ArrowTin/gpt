# ADR-009: Configuration Driven & Metadata Driven Platform

## Status

Accepted

## Date

2026-08-02

## Context

Landing, menu, dashboard, role, pricing, workflow, dan connector OTA sering berubah tanpa deploy code.

## Decision

Perilaku yang sering berubah dimodelkan sebagai **configuration/metadata** (bukan hardcode), dengan governance audit.

## Alternatives

- Hardcode per tenant — ditolak: tidak scalable white-label.

## Consequences

- Positive: selaras README Core Principles.
- Negative: perlu admin tooling & validation schema.

## References

- docs/00-foundation/004-core-principles.md
- docs/03-product-specification/008-landing-page-cms.md
