# ChannelHub PostgreSQL Schema Standard Blueprint

## Purpose

Menetapkan standar perancangan database PostgreSQL untuk ChannelHub.

---

## AI TRIGGER

### Tujuan Task
Memahami standar perancangan database PostgreSQL untuk implementasi schema yang konsisten.

### Konteks yang Perlu Dipahami AI
- Database Architecture: Application → Repository Layer → ORM → PostgreSQL Schema
- Schema Principle: Terstruktur berdasarkan domain, Mendukung scaling, Memiliki constraint yang jelas, Menjaga integritas data
- Naming Standard: Lowercase table name, Snake_case column, Primary key konsisten, Foreign key terdokumentasi
- Goal: Membangun database yang stabil untuk sistem Channel Manager enterprise

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/15-database-implementation/009-canonical-erd.md (canonical ERD)
- docs/15-database-implementation/010-postgresql-ddl-reference.md (DDL reference)

### File/Folder yang Perlu Diperiksa
- docs/15-database-implementation/002-domain-entity-design.md (domain entity)
- docs/15-database-implementation/005-database-indexing-strategy.md (indexing)

### Langkah Implementasi
1. Ikuti naming standard yang didefinisikan
2. Strukturkan database berdasarkan domain
3. Implementasikan constraint yang jelas
4. Pastikan integritas data terjaga

### Kriteria Keberhasilan (Definition of Done)
- Table name menggunakan lowercase
- Column name menggunakan snake_case
- Primary key konsisten (uuid)
- Foreign key terdokumentasi dengan proper naming

### Prompt Implementasi
```
Anda akan merancang atau mengimplementasikan database schema ChannelHub.

Baca docs/15-database-implementation/001-postgresql-schema-standard.md untuk memahami standar schema.

Database Architecture:
Application → Repository Layer → ORM → PostgreSQL Schema

Schema Principle (WAJIB):
- Terstruktur berdasarkan domain
- Mendukung scaling
- Memiliki constraint yang jelas
- Menjaga integritas data

Naming Standard (WAJIB):
- Table name: lowercase (users, reservations, properties)
- Column name: snake_case (created_at, updated_at, organization_id)
- Primary key: konsisten (uuid dengan gen_random_uuid())
- Foreign key: terdokumentasi dengan proper naming (fk_table_reference)

Implementasikan:
1. Gunakan lowercase untuk seluruh table name
2. Gunakan snake_case untuk seluruh column name
3. Primary key menggunakan uuid dengan default gen_random_uuid()
4. Foreign key menggunakan proper naming: fk_{table}_{reference}
5. Index menggunakan proper naming: idx_{table}_{column}
6. Unique constraint menggunakan proper naming: uq_{table}_{column}
7. Check constraint menggunakan proper naming: ck_{table}_{condition}

Pastikan database stabil dan konsisten dengan standar ini.
```

---

---

# Database Architecture

```
Application
     |
Repository Layer
     |
ORM
     |
PostgreSQL Schema
```

---

# Schema Principle

Database harus:

- Terstruktur berdasarkan domain.
- Mendukung scaling.
- Memiliki constraint yang jelas.
- Menjaga integritas data.

---

# Naming Standard

Gunakan:

- Lowercase table name.
- Snake_case column.
- Primary key konsisten.
- Foreign key terdokumentasi.

---

# Goal

Membangun database yang stabil untuk sistem Channel Manager enterprise.
