# ChannelHub State Management Pattern Blueprint

## Purpose

Mendefinisikan pengelolaan state frontend.

---

## AI TRIGGER

### Tujuan Task
Memahami pattern state management untuk implementasi yang efisien dan maintainable.

### Konteks yang Perlu Dipahami AI
- State Category: Server State → Application State → UI State
- Principle: Server data melalui API layer, Global state untuk kebutuhan bersama, Local state untuk komponen
- Rule: Hindari state duplikasi, Pisahkan data dan UI state, Jaga predictable flow
- Goal: Frontend mudah dikembangkan dan diuji

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/14-frontend-foundation/005-api-client-architecture.md (API client)

### File/Folder yang Perlu Diperiksa
- docs/14-frontend-foundation/009-frontend-project-structure.md (struktur project)

### Langkah Implementasi
1. Gunakan React Query atau SWR untuk server state
2. Gunakan Zustand atau Context API untuk global state minimal
3. Gunakan useState untuk local state komponen
4. Pisahkan data state dan UI state

### Kriteria Keberhasilan (Definition of Done)
- Server state dikelola dengan proper data fetching library
- Global state minimal dan hanya untuk cross-component data
- Local state untuk komponen-specific data
- Tidak ada state duplikasi

### Prompt Implementasi
```
Anda akan mengimplementasikan state management frontend ChannelHub.

Baca docs/14-frontend-foundation/004-state-management-pattern.md untuk memahami pattern.

State Category:
Server State → Application State → UI State

Principle (WAJIB):
- Server data melalui API layer (gunakan React Query/SWR)
- Global state untuk kebutuhan bersama (gunakan Zustand/Context API)
- Local state untuk komponen (gunakan useState)

Rules:
- Hindari state duplikasi
- Pisahkan data dan UI state
- Jaga predictable flow (unidirectional data flow)

Implementasikan:
1. Server state dengan React Query atau SWR:
   - Query untuk data fetching (GET)
   - Mutation untuk data update (POST, PUT, DELETE)
   - Cache invalidation yang proper
   - Optimistic update untuk UX yang baik

2. Global state minimal dengan Zustand atau Context API:
   - Tenant context (tenant aktif)
   - User session (user info, permissions)
   - UI state global (sidebar collapsed, theme)

3. Local state dengan useState:
   - Form state
   - UI state komponen-specific (modal open/close, dropdown open)

Pastikan frontend mudah dikembangkan dan diuji dengan pattern ini.
```

---

---

# State Category

```
Server State
      |
Application State
      |
UI State
```

---

# Principle

Gunakan state sesuai kebutuhan:

- Server data melalui API layer.
- Global state untuk kebutuhan bersama.
- Local state untuk komponen.

---

# Rule

Frontend harus:

- Menghindari state duplikasi.
- Memisahkan data dan UI state.
- Menjaga predictable flow.

---

# Goal

Membuat frontend mudah dikembangkan dan diuji.
