# ADR-001: NestJS sebagai Backend Framework

## Status

Accepted

## Date

2026-08-02

## Context

ChannelHub membutuhkan backend modular, testable, microservice-ready, dengan dukungan DI, validation, dan ecosystem Node.js yang matang.

## Decision

Menggunakan **NestJS** sebagai framework backend utama untuk API, modul domain, gateway, worker, dan integrasi BullMQ.

## Alternatives

- Express minimal — ditolak: struktur modul kurang terstandar untuk tim besar/AI.
- Fastify standalone — ditolak: NestJS sudah mendukung adapter dan pola modul ChannelHub.
- Language split (Go/Java) — ditolak untuk fase awal: meningkatkan kompleksitas operasi.

## Consequences

- Positive: konsistensi dengan docs/13, 18, 19; mudah dipisah per bounded context.
- Negative: learning curve decorator & module graph.
- Mitigation: docs/18-backend-implementation/002-module-generation-standard.md.

## Implementation Notes

- Bootstrap: docs/19-backend-application/001-nestjs-bootstrap.md
- Testing: docs/18-backend-implementation/004-service-testing-strategy.md
- Prompt: prompts/phases/phase-13-backend-foundation.md

## References

- docs/13-backend-foundation/001-nestjs-architecture-standard.md
- standards/backend.md
