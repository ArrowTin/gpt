# ADR-010: Monorepo Blueprint sebagai Single Source of Truth

## Status

Accepted

## Date

2026-08-02

## Context

Repository `ArrowTin/gpt` berisi blueprint, bukan seluruh source production. AI dan tim harus punya satu sumber kebenaran.

## Decision

- Blueprint repo (ini): docs, adr, prompts, standards, templates, diagrams, checklists.
- Application repo (terpisah): source code mengikuti blueprint; perubahan arsitektur besar kembali ke ADR di repo ini.

## Alternatives

- Docs scattered in app repo only — ditolak: drift antara tim/AI.

## Consequences

- Positive: vibe code prompt selalu punya context stabil.
- Negative: wajib sync STATE.yml & CHANGELOG setelah milestone.

## References

- README.md
- .channelhub/START.md
- docs/12-project-foundation/001-monorepo-architecture.md
