# ChannelHub Database Migration Generation Blueprint

## Purpose

Menjadi panduan AI untuk membuat perubahan database yang aman.

---

# 1. Migration Principle

Semua perubahan schema harus melalui migration.

Tidak boleh:

- Manual database modification.
- Perubahan tanpa version control.

---

# 2. Migration Flow

```
Schema Design
      |
Migration File
      |
Review
      |
Execute
      |
Validation
```

---

# 3. AI Requirement

AI harus mempertimbangkan:

- Existing schema.
- Relationship.
- Index.
- Data migration impact.

---

# 4. Safety Rule

Migration harus:

- Versioned.
- Reversible.
- Tested.

---

# 5. Validation

Database change selesai jika:

- Migration berhasil.
- Data integrity terjaga.
- Service tetap berjalan.
