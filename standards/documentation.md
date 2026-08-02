# Documentation Standard (Operational)

## Purpose

Aturan penulisan dan sync untuk semua folder dokumentasi.

## Scope

`docs/`, `adr/`, `standards/`, `prompts/`, `diagrams/`, `checklists/`.

## Rules

- Setiap dokumen: Title, Purpose, Scope (jika perlu), Rules/Technical Details, References.
- Istilah mengikuti [docs/00-foundation/006-glossary.md](../docs/00-foundation/006-glossary.md).
- Cross-link ke phase map: [docs/README.md](../docs/README.md).
- Perubahan arsitektur → ADR + update standards terkait.

## AI Compatibility

- Gunakan heading konsisten (`##` level).
- Cantumkan path file eksplisit saat merujuk blueprint.
- Satu micro-prompt = satu deliverable kecil.

## References

- [docs/00-foundation/008-documentation-standard.md](../docs/00-foundation/008-documentation-standard.md)
- [templates/architecture-doc-template.md](../templates/architecture-doc-template.md)
