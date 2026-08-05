# ChannelHub Frontend Routing Standard Blueprint

## Purpose

Menetapkan standar navigasi dan routing frontend.

---

## AI TRIGGER

### Tujuan Task
Memahami dan mengimplementasikan routing standard untuk navigasi yang aman dan scalable.

### Konteks yang Perlu Dipahami AI
- Routing Structure: Public Routes → Authentication Routes → Application Routes
- Principle: Routing harus mudah dipahami, mendukung permission, konsisten dengan domain
- Protected Route: Akses halaman harus mempertimbangkan Authentication, Role, Permission
- Goal: Navigasi aplikasi tetap aman dan scalable

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/14-frontend-foundation/009-frontend-project-structure.md (struktur project)
- docs/14-frontend-foundation/006-frontend-authentication-flow.md (auth flow)

### File/Folder yang Perlu Diperiksa
- docs/03-product-specification/002-role-permission-system.md (role permission)
- docs/16-api-contract/009-api-endpoint-specification.md (endpoint spec)

### Langkah Implementasi
1. Implementasikan public routes (landing, login, pricing)
2. Implementasikan application routes dengan proper structure
3. Implementasikan protected route dengan permission check
4. Pastikan routing konsisten dengan domain dan API

### Kriteria Keberhasilan (Definition of Done)
- Public routes berfungsi tanpa authentication
- Application routes terproteksi dengan auth guard
- Permission check berfungsi untuk role-based access
- Routing structure konsisten dengan domain

### Prompt Implementasi
```
Anda akan mengimplementasikan routing standard frontend ChannelHub.

Baca docs/14-frontend-foundation/007-frontend-routing-standard.md untuk memahami routing standard.

Routing Structure:
Public Routes → Authentication Routes → Application Routes

Principle (WAJIB):
- Routing harus mudah dipahami (intuitive URL structure)
- Routing harus mendukung permission (role-based access)
- Routing harus konsisten dengan domain (mirip API structure)

Protected Route:
Akses halaman harus mempertimbangkan:
- Authentication (user harus login)
- Role (user harus memiliki role yang sesuai)
- Permission (user harus memiliki permission yang sesuai)

Implementasikan dengan Next.js App Router:
1. Public routes di app/(public)/:
   - / (landing page)
   - /login (login page)
   - /pricing (pricing page)
   - Tidak butuh authentication

2. Application routes di app/(app)/:
   - /dashboard (dashboard utama)
   - /properties (management properti)
   - /reservations (management reservasi)
   - /channels (management channel)
   - /billing (management billing)
   - /settings (pengaturan)
   - Butuh authentication

3. Admin routes di app/(admin)/:
   - /tenants (management tenant)
   - /plans (management subscription plan)
   - /feature-flags (management feature flags)
   - /audit-logs (audit logs)
   - Butuh super admin role

4. Protected route mechanism:
   - Middleware untuk auth check
   - Permission check dengan can() helper
   - Redirect proper jika tidak authorized
   - Loading state saat checking permission

Pastikan navigasi aplikasi aman dan scalable.
```

---

---

# Routing Structure

```
Public Routes
      |
Authentication Routes
      |
Application Routes
```

---

# Principle

Routing harus:

- Mudah dipahami.
- Mendukung permission.
- Konsisten dengan domain.

---

# Protected Route

Akses halaman harus mempertimbangkan:

- Authentication.
- Role.
- Permission.

---

# Goal

Navigasi aplikasi tetap aman dan scalable.
