# Micro-Prompt Template

Gunakan template ini untuk setiap increment vibe code. Salin ke `prompts/phases/` atau file lifecycle.

---

## Micro-Prompt ID

`phase-NN-XXX-short-name`

## Role

[Senior X Engineer — ChannelHub Enterprise]

## Context (wajib dibaca)

1. README.md
2. .channelhub/START.md
3. .channelhub/STATE.yml
4. [docs path spesifik]
5. [adr/ jika relevan]
6. [standards/ layer terkait]

## Goal

**Satu deliverable kecil saja:** [contoh: "selesaikan docs/22-security/001-…" ATAU "implementasi modul auth step 1 di repo app"]

## Constraints

- Jangan ubah arsitektur utama tanpa ADR.
- Selaras multi-tenant, API First, observability.
- [Constraint spesifik fase]

## Implementation Detail

- Input: …
- Output files: …
- Integration points: …
- Out of scope: …

## Validation

- [ ] Checklist: checklists/…
- [ ] Tests / doc review criteria
- [ ] Cross-links docs ↔ adr ↔ standards

## Output Requirement

Ringkasan: files changed, risks, next micro-prompt ID.

## Next

`phase-NN-(XXX+1)-…`
