# ChannelHub Frontend Folder Design Blueprint

## Purpose

Mendefinisikan struktur folder frontend agar konsisten.

---

## AI TRIGGER

### Tujuan Task
Memahami struktur folder frontend untuk organisasi kode yang konsisten.

### Konteks yang Perlu Dipahami AI
- Structure: frontend/ dengan app/, components/, features/, services/, hooks/, utils/, tests/
- Feature Principle: Setiap feature dapat memiliki Components, Hooks, API integration, Validation, Tests
- Rule: Folder harus mudah ditemukan AI agent, hindari duplicate component, pisahkan reusable dan feature code

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/14-frontend-foundation/009-frontend-project-structure.md (struktur project detail)

### File/Folder yang Perlu Diperiksa
- docs/14-frontend-foundation/001-nextjs-architecture-standard.md (arsitektur)
- docs/14-frontend-foundation/003-design-system-foundation.md (design system)

### Langkah Implementasi
1. Ikuti struktur folder yang didefinisikan
2. Pisahkan komponen reusable (components/) dan feature-specific (features/)
3. Pastikan setiap feature memiliki struktur yang konsisten
4. Hindari duplicate component dengan proper organization

### Kriteria Keberhasilan (Definition of Done)
- Struktur folder mengikuti pattern yang didefinisikan
- Komponen reusable di components/, feature code di features/
- Setiap feature memiliki struktur yang konsisten
- Tidak ada duplicate component

### Prompt Implementasi
```
Anda akan mengorganisasi struktur folder frontend ChannelHub.

Baca docs/14-frontend-foundation/002-frontend-folder-design.md untuk memahami struktur folder.

Structure:
frontend/
├── app/          # Next.js App Router
├── components/   # Komponen reusable lintas fitur
├── features/     # Kode fitur spesifik per domain
├── services/     # API services
├── hooks/        # Custom React hooks
├── utils/        # Utility functions
└── tests/        # Test files

Feature Principle:
Setiap feature di features/<domain>/ dapat memiliki:
- Components/ (komponen spesifik fitur)
- Hooks/ (custom hooks untuk fitur)
- API integration (panggilan API)
- Validation (schema validasi)
- Tests (test untuk fitur)

RULES (WAJIB):
- Folder harus mudah ditemukan AI agent
- Hindari duplicate component
- Pisahkan reusable (components/) dan feature code (features/)
- Komponen reusable HANYA di components/
- Kode fitur spesifik HANYA di features/<domain>/

Pastikan struktur folder konsisten dan mudah dinavigasi.
```

---

---

# Structure

```
frontend/
 |
 +-- app/
 +-- components/
 +-- features/
 +-- services/
 +-- hooks/
 +-- utils/
 +-- tests/
```

---

# Feature Principle

Setiap feature dapat memiliki:

- Components.
- Hooks.
- API integration.
- Validation.
- Tests.

---

# Rule

Folder harus:

- Mudah ditemukan AI agent.
- Menghindari duplicate component.
- Memisahkan reusable dan feature code.
