# ChannelHub Authentication Authorization Foundation Blueprint

## Purpose

Mendefinisikan fondasi keamanan backend.

---

## AI TRIGGER

### Tujuan Task
Memahami dan mengimplementasikan fondasi authentication dan authorization backend.

### Konteks yang Perlu Dipahami AI
- Authentication Flow: User Login → Credential Validation → Token Generation → Authenticated Request
- Authorization: Role based access control, Permission validation, Resource ownership check
- Security Rule: Validate every request, Protect sensitive data, Record security events

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/22-security/ (security documentation)
- docs/16-api-contract/005-api-authentication-standard.md (API auth standard)

### File/Folder yang Perlu Diperiksa
- docs/13-backend-foundation/003-api-gateway-implementation.md (API gateway)
- docs/22-security/002-authentication-hardening.md (auth hardening)
- docs/22-security/003-authorization-tenant-isolation.md (authorization)

### Langkah Implementasi
1. Implementasikan authentication flow dengan JWT
2. Implementasikan RBAC dengan role dan permission
3. Implementasikan resource ownership check
4. Pastikan setiap request ter-autentikasi dan ter-authorized

### Kriteria Keberhasilan (Definition of Done)
- Authentication flow berfungsi dengan JWT
- RBAC berfungsi dengan role dan permission
- Resource ownership check berfungsi
- Setiap request ter-validasi dan ter-authorized
- Security events ter-record

### Prompt Implementasi
```
Anda akan mengimplementasikan authentication dan authorization backend ChannelHub.

Baca docs/13-backend-foundation/004-authentication-authorization-foundation.md untuk memahami fondasi keamanan.

Authentication Flow:
User Login → Credential Validation → Token Generation → Authenticated Request

Authorization menggunakan:
- Role Based Access Control (RBAC)
- Permission validation
- Resource ownership check

Security Rules (WAJIB):
- Validate EVERY request
- Protect sensitive data
- Record security events

Implementasikan:
1. JWT authentication dengan access token (15 menit) dan refresh token (7 hari)
2. Password hashing dengan bcrypt (cost factor 12)
3. RBAC dengan role dan permission
4. Resource ownership check untuk data ownership
5. Guards untuk authentication dan authorization
6. Security event logging untuk audit

Pastikan backend aman dan siap production.
```

---

---

# Authentication Flow

```
User Login
    |
Credential Validation
    |
Token Generation
    |
Authenticated Request
```

---

# Authorization

Menggunakan:

- Role based access control.
- Permission validation.
- Resource ownership check.

---

# Security Rule

Backend wajib:

- Validate every request.
- Protect sensitive data.
- Record security events.

---

# Goal

Membangun backend yang aman dan siap production.
