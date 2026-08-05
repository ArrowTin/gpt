# Documentation Standard

## Purpose

Menentukan standar penulisan seluruh dokumentasi ChannelHub.

---

## AI TRIGGER

### Tujuan Task
Memastikan seluruh dokumentasi yang dibuat atau diperbarui mengikuti standar yang ditetapkan.

### Konteks yang Perlu Dipahami AI
- Dokumentasi harus jelas, terstruktur, dan dapat dipahami manusia maupun AI
- Setiap dokumen harus memiliki komponen standar: Title, Purpose, Scope, Context, Rules, Technical Details, Impact, References
- AI Compatibility adalah kunci - gunakan istilah konsisten

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- templates/ (template dokumen)

### File/Folder yang Perlu Diperiksa
- templates/adr-template.md
- templates/prompt-micro-template.md
- docs/ (struktur dokumentasi yang ada)

### Langkah Implementasi
1. Gunakan format standar untuk setiap dokumen baru
2. Pastikan cross-reference valid
3. Gunakan istilah yang konsisten dengan glossary
4. Update changelog untuk perubahan besar

### Kriteria Keberhasilan (Definition of Done)
- Dokumen memiliki struktur standar
- Tidak ada broken link
- Istilah konsisten dengan glossary
- Dapat dipahami oleh AI (AI Compatibility)

### Prompt Implementasi
```
Anda akan membuat atau memperbarui dokumentasi ChannelHub.

SEBELUM membuat dokumen apapun, baca docs/00-foundation/008-documentation-standard.md.

Pastikan dokumen memiliki:
- Title yang jelas
- Purpose yang spesifik
- Scope yang terdefinisi
- Context yang relevan
- Rules yang mengikat
- Technical Details yang lengkap
- Impact yang dijelaskan
- References yang valid

Gunakan istilah yang KONSISTEN dengan docs/00-foundation/006-glossary.md.

Pastikan seluruh cross-reference VALID (tidak ada broken link).

Untuk dokumen yang membutuhkan AI Trigger, tambahkan section AI TRIGGER dengan format yang konsisten.
```

---

## Document Format

Setiap dokumen harus memiliki:

- Title
- Purpose
- Scope
- Context
- Rules
- Technical Details
- Impact
- References

## Writing Principle

Dokumentasi harus:

- Jelas.
- Terstruktur.
- Dapat dipahami manusia dan AI.
- Memiliki hubungan antar dokumen.

## Versioning

Perubahan besar harus dicatat melalui:

- Changelog
- ADR
- Version update

## AI Compatibility

Dokumen harus menggunakan istilah konsisten agar dapat digunakan sebagai konteks AI coding assistant.

## Quality Standard

Dokumen dianggap selesai jika:

- Memiliki tujuan jelas.
- Tidak bertentangan dengan ADR.
- Dapat menjadi referensi implementasi.
- Memiliki hubungan dengan modul terkait.
