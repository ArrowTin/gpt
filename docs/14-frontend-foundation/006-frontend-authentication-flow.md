# ChannelHub Frontend Authentication Flow Blueprint

## Purpose

Mendefinisikan alur autentikasi pada aplikasi frontend.

---

## AI TRIGGER

### Tujuan Task
Memahami dan mengimplementasikan authentication flow frontend yang aman dan konsisten.

### Konteks yang Perlu Dipahami AI
- Authentication Flow: User Login → API Request → Token Received → Session Management → Protected Access
- Frontend Responsibility: Login interface, Session state, Protected route, Authentication status
- Security Rule: Frontend tidak menjadi sumber otorisasi utama, Backend tetap melakukan validasi permission
- Goal: Menyediakan pengalaman login yang aman dan konsisten

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/13-backend-foundation/004-authentication-authorization-foundation.md (backend auth)
- docs/22-security/002-authentication-hardening.md (security hardening)

### File/Folder yang Perlu Diperiksa
- docs/14-frontend-foundation/005-api-client-architecture.md (API client)
- docs/14-frontend-foundation/007-frontend-routing-standard.md (routing)

### Langkah Implementasi
1. Implementasikan login interface dengan form validation
2. Implementasikan session management dengan token storage
3. Implementasikan protected route dengan auth guard
4. Implementasikan token refresh mechanism

### Kriteria Keberhasilan (Definition of Done)
- Login interface berfungsi dengan proper validation
- Session state dikelola dengan secure token storage
- Protected route berfungsi dengan auth guard
- Token refresh mechanism berfungsi

### Prompt Implementasi
```
Anda akan mengimplementasikan authentication flow frontend ChannelHub.

Baca docs/14-frontend-foundation/006-frontend-authentication-flow.md untuk memahami alur auth.

Authentication Flow:
User Login → API Request → Token Received → Session Management → Protected Access

Frontend Responsibility:
- Login interface (form login, validation)
- Session state (token storage, user info)
- Protected route (auth guard, redirect)
- Authentication status (loading, authenticated, unauthenticated)

Security Rule (WAJIB):
- Frontend TIDAK menjadi sumber otorisasi utama
- Backend TETAP melakukan validasi permission
- Frontend hanya untuk UX, security enforcement di backend

Implementasikan:
1. Login interface di features/auth/:
   - Login form dengan email dan password
   - Form validation dengan proper error handling
   - Loading state saat login

2. Session management di src/lib/auth/:
   - Token storage (httpOnly cookie atau secure localStorage)
   - Token refresh mechanism (refresh token rotation)
   - User session state dengan React Context atau Zustand
   - Logout function dengan proper cleanup

3. Protected route guard:
   - Middleware atau HOC untuk route protection
   - Redirect ke login jika unauthenticated
   - Redirect ke dashboard jika authenticated di login page
   - Loading state saat checking auth

4. Auth utilities:
   - useAuth hook untuk auth state dan methods
   - Token retrieval and refresh logic
   - Permission check helper (can())

Pastikan pengalaman login aman dan konsisten.
```

---

---

# Authentication Flow

```
User Login
     |
API Request
     |
Token Received
     |
Session Management
     |
Protected Access
```

---

# Responsibility

Frontend menangani:

- Login interface.
- Session state.
- Protected route.
- Authentication status.

---

# Security Rule

Frontend tidak menjadi sumber otorisasi utama.

Backend tetap melakukan validasi permission.

---

# Goal

Menyediakan pengalaman login yang aman dan konsisten.
