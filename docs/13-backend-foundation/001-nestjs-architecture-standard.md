# ChannelHub NestJS Architecture Standard Blueprint

## Purpose

Menetapkan standar arsitektur backend menggunakan NestJS.

---

## AI TRIGGER

### Tujuan Task
Memahami arsitektur NestJS yang digunakan ChannelHub untuk implementasi backend.

### Konteks yang Perlu Dipahami AI
- Layered architecture: API Gateway → Application Modules → Domain Services → Data Access Layer → Database
- Controller: HTTP request, validation input, response formatting
- Service: Business logic, transaction flow, domain operation
- Repository: Database access, query abstraction
- Backend harus modular, testable, scalable, mengikuti domain boundary

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/13-backend-foundation/009-backend-project-structure.md (struktur project)
- docs/13-backend-foundation/002-backend-module-design.md (module design)

### File/Folder yang Perlu Diperiksa
- docs/15-database-implementation/ (database layer)
- docs/16-api-contract/ (API contract)
- docs/18-backend-implementation/ (pola implementasi)

### Langkah Implementasi
1. Pahami layered architecture yang didefinisikan
2. Terapkan separation of concern pada setiap layer
3. Pastikan business logic tidak ada di controller
4. Pastikan database access hanya melalui repository

### Kriteria Keberhasilan (Definition of Done)
- Implementasi mengikuti layered architecture
- Controller tipis, hanya handling HTTP
- Service berisi business logic
- Repository berisi database access
- Tidak ada cross-layer violation

### Prompt Implementasi
```
Anda akan mengimplementasikan backend NestJS ChannelHub.

Baca docs/13-backend-foundation/001-nestjs-architecture-standard.md untuk memahami arsitektur.

Ikuti layered architecture:
API Gateway → Application Modules → Domain Services → Data Access Layer → Database

Responsibility setiap layer:
- Controller: HTTP request, validation input, response formatting
- Service: Business logic, transaction flow, domain operation
- Repository: Database access, query abstraction

RULES:
- Controller TIPIS: hanya handling HTTP, jangan taruh business logic di sini
- Service: tempat business logic dan transaction flow
- Repository: HANYA database access, gunakan ORM/query builder
- Jangan lompati layer (misal: controller langsung ke repository)
- Business logic HANYA di service atau domain layer

Pastikan implementasi modular, testable, scalable, dan mengikuti domain boundary.
```

---

---

# Backend Architecture

```
API Gateway
     |
Application Modules
     |
Domain Services
     |
Data Access Layer
     |
Database
```

---

# Layer Responsibility

## Controller

Menangani:

- HTTP request.
- Validation input.
- Response formatting.

## Service

Menangani:

- Business logic.
- Transaction flow.
- Domain operation.

## Repository

Menangani:

- Database access.
- Query abstraction.

---

# Rule

Backend wajib:

- Modular.
- Testable.
- Scalable.
- Mengikuti domain boundary.
