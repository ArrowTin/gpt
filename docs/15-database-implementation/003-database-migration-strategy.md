# ChannelHub Database Migration Strategy Blueprint

## Purpose

Menetapkan cara perubahan schema database dikelola.

---

## AI TRIGGER

### Tujuan Task
Memahami strategy migration database untuk perubahan schema yang aman dan dapat dilacak.

### Konteks yang Perlu Dipahami AI
- Migration Flow: Schema Change → Migration File → Review → Apply Migration → Validation
- Rule: Migration harus version controlled, Reversible jika memungkinkan, Teruji sebelum production
- Environment: Migration dipisahkan untuk Development, Staging, Production
- Goal: Perubahan database aman dan dapat dilacak

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/15-database-implementation/010-postgresql-ddl-reference.md (DDL reference)
- docs/15-database-implementation/009-canonical-erd.md (canonical ERD)

### File/Folder yang Perlu Diperiksa
- docs/12-project-foundation/008-cicd-pipeline-foundation.md (CI/CD)
- docs/15-database-implementation/001-postgresql-schema-standard.md (schema standard)

### Langkah Implementasi
1. Buat migration file untuk setiap perubahan schema
2. Pastikan migration reversible (down migration)
3. Test migration di development dan staging
4. Apply migration ke production dengan proper review

### Kriteria Keberhasilan (Definition of Done)
- Migration file version controlled
- Migration reversible dengan down migration
- Migration teruji di development dan staging
- Migration production tercatat dan dapat di-rollback

### Prompt Implementasi
```
Anda akan membuat atau mengelola database migration ChannelHub.

Baca docs/15-database-implementation/003-database-migration-strategy.md untuk memahami strategy migration.

Migration Flow:
Schema Change → Migration File → Review → Apply Migration → Validation

Rules (WAJIB):
- Migration harus version controlled (git)
- Migration harus reversible jika memungkinkan (down migration)
- Migration harus teruji sebelum production
- Migration harus memiliki proper naming dan timestamp

Environment:
- Development: migration dijalankan untuk development database
- Staging: migration dijalankan untuk staging database
- Production: migration dijalankan untuk production database dengan approval

Implementasikan:
1. Gunakan migration tool (TypeORM migration, Prisma migrate, atau custom)
2. Setiap perubahan schema WAJIB melalui migration file
3. Migration file naming: {timestamp}-{description}.sql atau .ts
4. Selalu sediakan up migration dan down migration
5. Test migration di development dan staging sebelum production
6. Document perubahan migration di CHANGELOG
7. JANGAN mengubah migration yang sudah di-apply ke production
8. Untuk perubahan pada migration yang sudah di-apply, buat migration baru

Pastikan perubahan database aman dan dapat dilacak.
```

---

---

# Migration Flow

```
Schema Change
      |
Migration File
      |
Review
      |
Apply Migration
      |
Validation
```

---

# Rule

Migration harus:

- Version controlled.
- Reversible jika memungkinkan.
- Teruji sebelum production.

---

# Environment

Migration dipisahkan untuk:

- Development.
- Staging.
- Production.

---

# Goal

Perubahan database aman dan dapat dilacak.
