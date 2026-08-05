# ChannelHub Backend Module Design Blueprint

## Purpose

Mendefinisikan pembagian module backend agar mudah dikembangkan.

---

## AI TRIGGER

### Tujuan Task
Memahami pembagian module backend dan boundary rule untuk implementasi yang modular.

### Konteks yang Perlu Dipahami AI
- Core Modules: Auth, Organization, Property, Reservation, OTA, Notification
- Setiap module memiliki: Controller, Service, DTO, Entity/model, Test
- Module boundary: tidak boleh akses internal module lain langsung
- Goal: backend dapat berkembang menjadi microservice

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/13-backend-foundation/001-nestjs-architecture-standard.md (arsitektur NestJS)
- docs/13-backend-foundation/009-backend-project-structure.md (struktur project)

### File/Folder yang Perlu Diperiksa
- docs/15-database-implementation/002-domain-entity-design.md (domain entity)
- docs/17-core-services/ (service design)

### Langkah Implementasi
1. Pahami core modules yang didefinisikan
2. Pastikan setiap module memiliki komponen wajib
3. Ikuti boundary rule: jangan akses internal module lain langsung
4. Design dengan mindset microservice ready

### Kriteria Keberhasilan (Definition of Done)
- Module boundary jelas dan terdefinisi
- Setiap module memiliki komponen wajib
- Tidak ada direct access ke internal module lain
- Module dapat diekstraksi menjadi microservice jika needed

### Prompt Implementasi
```
Anda akan mendesain atau mengimplementasikan module backend ChannelHub.

Baca docs/13-backend-foundation/002-backend-module-design.md untuk memahami module design.

Core Modules:
- Auth Module
- Organization Module
- Property Module
- Reservation Module
- OTA Module
- Notification Module

Setiap module WAJIB memiliki:
- Controller
- Service
- DTO
- Entity/model
- Test

BOUNDARY RULE (WAJIB diikuti):
- Module TIDAK BOLEH mengakses internal module lain secara langsung
- Module TIDAK BOLEH membuat duplicate business logic
- Komunikasi antar module HANYA melalui service publik
- Design dengan mindset microservice ready

Jika perlu komunikasi antar module:
- Gunakan service call antar module
- Atau gunakan event-driven pattern (BullMQ)
- JANGAN import file internal dari module lain

Pastikan setiap module dapat berdiri sendiri dan siap untuk ekstraksi microservice.
```

---

---

# Core Modules

```
Auth Module
Organization Module
Property Module
Reservation Module
OTA Module
Notification Module
```

---

# Module Principle

Setiap module memiliki:

- Controller.
- Service.
- DTO.
- Entity/model.
- Test.

---

# Boundary Rule

Module tidak boleh:

- Mengakses internal module lain secara langsung.
- Membuat duplicate business logic.

---

# Goal

Backend dapat berkembang menjadi microservice bila diperlukan.
