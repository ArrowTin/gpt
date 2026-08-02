# ChannelHub Vibe Code Workflow

## Purpose

Menetapkan cara mengeksekusi blueprint ini menjadi source code memakai AI coding agent: satu task per sesi, dari implementasi sampai deploy.

## Scope

Berlaku pada repository aplikasi (monorepo `apps/backend` + `apps/frontend`). Pada repository blueprint, AI hanya menyunting dokumentasi.

## Rules

- Satu sesi AI = satu increment: satu module backend, satu feature frontend, atau satu cross-cutting concern.
- Setiap sesi wajib membaca contract artifact yang relevan sebelum menulis kode; skema database dan kontrak API tidak boleh ditebak.
- Perubahan kontrak dilakukan pada file kontrak lebih dulu, baru kode menyusul.
- Increment dianggap selesai hanya bila test hijau; increment gagal tidak boleh ditumpuk oleh increment berikutnya.
- Task yang membutuhkan keputusan arsitektur baru dihentikan dan diangkat menjadi ADR.

## Technical Details

### Alur satu increment

```text
Pilih task  →  Baca kontrak  →  Rencana file  →  Implementasi
                                                     |
                              Deploy  ←  Review  ←  Test
```

| Tahap | Isi | Referensi |
| --- | --- | --- |
| Pilih task | Satu MP dari `prompts/phases/` | [prompts/index-by-phase.md](../../prompts/index-by-phase.md) |
| Baca kontrak | OpenAPI, DDL/ERD, struktur folder | [docs/README.md](../README.md) bagian Contract Artifact |
| Rencana file | Daftar file yang akan dibuat/diubah, disetujui dulu | [007-module-execution-template.md](./007-module-execution-template.md) |
| Implementasi | Kode + migration + DTO sesuai kontrak | [prompts/lifecycle/01-development-increment.md](../../prompts/lifecycle/01-development-increment.md) |
| Test | Unit, integrasi, uji negatif tenant, contract test | [prompts/lifecycle/02-testing-increment.md](../../prompts/lifecycle/02-testing-increment.md) |
| Review | Checklist code review dan security | [checklists/code-review.md](../../checklists/code-review.md) |
| Deploy | Build image, migrasi, smoke test | [prompts/lifecycle/03-deployment-increment.md](../../prompts/lifecycle/03-deployment-increment.md) |

### Urutan pembangunan

Urutan increment backend dan frontend didefinisikan pada:

- [docs/13-backend-foundation/009-backend-project-structure.md](../13-backend-foundation/009-backend-project-structure.md)
- [docs/14-frontend-foundation/009-frontend-project-structure.md](../14-frontend-foundation/009-frontend-project-structure.md)
- [docs/06-implementation-roadmap/008-core-service-development-order.md](../06-implementation-roadmap/008-core-service-development-order.md)

### Kontinuitas antar sesi

Setiap sesi diawali dengan konteks berikut, bukan riwayat chat:

```text
- Fase aktif        : .channelhub/STATE.yml
- Kontrak           : contract artifact pada docs/README.md
- Increment terakhir: commit/PR terakhir pada repository aplikasi
- Task berikutnya   : satu MP dari prompts/phases/
```

### Kriteria selesai per increment

- Kode berjalan dan sesuai kontrak.
- Test tersedia dan hijau, termasuk uji negatif tenant untuk modul CRUD.
- Dokumentasi atau kontrak diperbarui bila perilaku berubah.
- Tidak ada nilai bisnis yang di-hardcode.

## Impact

- [prompts/README.md](../../prompts/README.md) — pustaka micro-prompt
- [010-final-vibe-code-master-prompt.md](./010-final-vibe-code-master-prompt.md) — konteks global sesi
- [README.md](../../README.md) — panduan eksekusi ringkas

## References

- [001-ai-coding-principle.md](./001-ai-coding-principle.md)
- [003-prompt-engineering-standard.md](./003-prompt-engineering-standard.md)
- [standards/ai-development.md](../../standards/ai-development.md)
