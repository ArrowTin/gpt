# ChannelHub Database Integration Pattern Blueprint

## Purpose

Mendefinisikan standar komunikasi backend dengan database.

---

## AI TRIGGER

### Tujuan Task
Memahami pattern integrasi database untuk implementasi repository layer.

### Konteks yang Perlu Dipahami AI
- Data Flow: Domain Service → Repository Layer → ORM/Query Builder → Database
- Database access harus melalui abstraction layer (repository)
- Service tidak boleh langsung melakukan query database
- Repository menangani: Query operation, Data mapping, Transaction support, Performance optimization

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/15-database-implementation/009-canonical-erd.md (database schema)
- docs/15-database-implementation/010-postgresql-ddl-reference.md (DDL reference)

### File/Folder yang Perlu Diperiksa
- docs/13-backend-foundation/009-backend-project-structure.md (struktur project)
- docs/15-database-implementation/003-database-migration-strategy.md (migration strategy)

### Langkah Implementasi
1. Buat repository layer untuk setiap module
2. Gunakan ORM (TypeORM/Prisma) atau query builder
3. Implementasikan query operation di repository
4. Pastikan service tidak langsung akses database

### Kriteria Keberhasilan (Definition of Done)
- Seluruh database access melalui repository
- Service tidak ada query database langsung
- Repository memiliki fungsi CRUD dan query spesifik
- Transaction support tersedia

### Prompt Implementasi
```
Anda akan mengimplementasikan integrasi database backend ChannelHub.

Baca docs/13-backend-foundation/005-database-integration-pattern.md untuk memahami pattern.

Data Flow:
Domain Service → Repository Layer → ORM/Query Builder → Database

PRINCIPLE (WAJIB):
- Database access HARUS melalui abstraction layer (repository)
- Service TIDAK BOLEH langsung melakukan query database
- JANGAN gunakan query langsung di service atau controller

Repository Responsibility:
- Query operation (CRUD, complex query)
- Data mapping (entity ↔ DTO)
- Transaction support
- Performance optimization (index, query optimization)

Implementasikan:
1. Repository untuk setiap module mengikuti struktur di docs/13-backend-foundation/009-backend-project-structure.md
2. Gunakan ORM (TypeORM/Prisma) sesuai setup di Phase 15
3. Repository method untuk query yang sering digunakan
4. Transaction support di repository layer
5. Tenant filtering otomatis di repository (multi-tenant)

Pastikan backend mudah diuji dan dikembangkan dengan pattern ini.
```

---

---

# Data Flow

```
Domain Service
      |
Repository Layer
      |
ORM / Query Builder
      |
Database
```

---

# Principle

Database access harus melalui abstraction layer.

Service tidak boleh langsung melakukan query database.

---

# Repository Responsibility

Repository menangani:

- Query operation.
- Data mapping.
- Transaction support.
- Performance optimization.

---

# Goal

Menciptakan backend yang mudah diuji dan dikembangkan.
