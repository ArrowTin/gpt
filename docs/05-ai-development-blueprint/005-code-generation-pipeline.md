# ChannelHub Code Generation Pipeline

## Purpose

Mendefinisikan proses perubahan blueprint menjadi source code secara aman dan terstruktur.

---

# 1. Pipeline

```
Requirement
    |
Architecture Reference
    |
Implementation Plan
    |
Code Generation
    |
Testing
    |
Review
    |
Integration
```

---

# 2. Before Coding

AI wajib memahami:

- Module responsibility.
- Existing code.
- Dependency.
- API contract.
- Database ownership.

---

# 3. Generation Rule

AI membuat perubahan kecil dan terukur.

Tidak membuat:

- File tidak diperlukan.
- Dependency tanpa alasan.
- Perubahan arsitektur mendadak.

---

# 4. Validation

Setiap hasil generate harus melalui:

- Build check.
- Test check.
- Architecture review.
- Security review.

---

# 5. Documentation Update

Source code selesai hanya jika dokumentasi ikut diperbarui.
