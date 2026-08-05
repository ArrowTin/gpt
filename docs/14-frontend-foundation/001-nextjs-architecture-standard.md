# ChannelHub Next.js Architecture Standard Blueprint

## Purpose

Menetapkan standar arsitektur frontend menggunakan Next.js.

---

## AI TRIGGER

### Tujuan Task
Memahami arsitektur Next.js yang digunakan ChannelHub untuk implementasi frontend.

### Konteks yang Perlu Dipahami AI
- Layered architecture: Application Layer → Feature Modules → UI Components → API Services → Backend Gateway
- App Layer: Routing, Layout, Page composition
- Feature Layer: Business feature, User interaction, Feature state
- Component Layer: Reusable UI, Design consistency
- Frontend harus modular, maintainable, reusable, konsisten dengan backend contract

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/14-frontend-foundation/009-frontend-project-structure.md (struktur project)
- contracts/openapi/channelhub.v1.yaml (API contract)

### File/Folder yang Perlu Diperiksa
- docs/14-frontend-foundation/005-api-client-architecture.md (API client)
- docs/14-frontend-foundation/004-state-management-pattern.md (state management)

### Langkah Implementasi
1. Pahami layered architecture yang didefinisikan
2. Terapkan separation of concern pada setiap layer
3. Pastikan frontend konsisten dengan backend contract
4. Gunakan Next.js App Router dengan proper structure

### Kriteria Keberhasilan (Definition of Done)
- Implementasi mengikuti layered architecture
- App layer berisi routing dan layout
- Feature layer berisi business logic
- Component layer berisi reusable UI
- Konsisten dengan backend contract

### Prompt Implementasi
```
Anda akan mengimplementasikan frontend Next.js ChannelHub.

Baca docs/14-frontend-foundation/001-nextjs-architecture-standard.md untuk memahami arsitektur.

Ikuti layered architecture:
Application Layer → Feature Modules → UI Components → API Services → Backend Gateway

Responsibility setiap layer:
- App Layer: Routing, Layout, Page composition
- Feature Layer: Business feature, User interaction, Feature state
- Component Layer: Reusable UI, Design consistency

RULES:
- Frontend harus modular dan maintainable
- Komponen harus reusable lintas fitur
- Kode fitur di features/<domain>, komponen reusable di components/
- SELALU konsisten dengan backend contract (OpenAPI)
- Gunakan Next.js App Router dengan server component sebagai default

Pastikan implementasi mengikuti struktur di docs/14-frontend-foundation/009-frontend-project-structure.md.
```

---

---

# Frontend Architecture

```
Application Layer
        |
Feature Modules
        |
UI Components
        |
API Services
        |
Backend Gateway
```

---

# Layer Responsibility

## App Layer

Menangani:

- Routing.
- Layout.
- Page composition.

## Feature Layer

Menangani:

- Business feature.
- User interaction.
- Feature state.

## Component Layer

Menangani:

- Reusable UI.
- Design consistency.

---

# Rule

Frontend wajib:

- Modular.
- Maintainable.
- Reusable.
- Konsisten dengan backend contract.
