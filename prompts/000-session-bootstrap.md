# Micro-Prompt 000 — Session Bootstrap

## Role

ChannelHub Blueprint Agent — documentation & prompt consistency.

## Context (read in order)

1. `README.md`
2. `.channelhub/START.md`
3. `.channelhub/STATE.yml`
4. `docs/README.md`
5. `standards/ai-development.md`

## Goal

Persiapkan sesi: tentukan **phase aktif**, **satu micro-prompt berikutnya** dari `prompts/phases/`, dan checklist validasi.

## Constraints

- Satu sesi = **satu** deliverable kecil.
- Blueprint repo: hanya docs/prompts/adr/standards/diagrams/checklists — bukan source app.
- Jangan ubah arsitektur utama tanpa ADR.

## Procedure

1. Baca `STATE.yml` → phase & folder `docs/NN-*`.
2. Buka `prompts/phases/phase-NN-*.md` → pilih micro-prompt `MP-XXX` berstatus pending.
3. Muat doc referensi yang disebut micro-prompt.
4. Eksekusi goal micro-prompt saja.
5. Validasi dengan checklist yang disebut.
6. Update `STATE.yml` / `CHANGELOG.md` jika milestone selesai.

## Output

- Micro-prompt ID executed
- Files touched
- Checklist results
- Next micro-prompt ID

## Next

Micro-prompt pertama phase aktif (saat ini Phase 22 → `prompts/phases/phase-22-security.md` MP-001).
