# ADR-002: Next.js sebagai Frontend Framework

## Status

Accepted

## Date

2026-08-02

## Context

ChannelHub memerlukan dashboard multi-tenant, SSR/SSG fleksibel, routing modern, dan integrasi API typed.

## Decision

Menggunakan **Next.js (App Router)** sebagai frontend utama admin, subscriber, dan white-label surfaces.

## Alternatives

- SPA Vite-only — ditolak: kebutuhan SEO/landing dan pola routing enterprise.
- Micro-frontends day one — ditolak: premature; modular internal dulu.

## Consequences

- Positive: selaras product spec dashboard & onboarding.
- Negative: kompleksitas server/client boundary.
- Mitigation: docs/14-frontend-foundation/, permission-driven rendering.

## Implementation Notes

- docs/20-frontend-application/001-nextjs-bootstrap.md
- prompts/phases/phase-14-frontend-foundation.md

## References

- docs/14-frontend-foundation/001-nextjs-architecture-standard.md
- standards/frontend.md
