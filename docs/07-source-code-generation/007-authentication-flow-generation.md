# ChannelHub Authentication Flow Generation Blueprint

## Purpose

Menjadi panduan AI untuk implementasi alur autentikasi end-to-end.

---

# 1. Authentication Flow

```
User
 |
Login Request
 |
API Gateway
 |
Identity Service
 |
Token Response
 |
Protected Resource
```

---

# 2. Required Components

Meliputi:

- Login endpoint.
- Token generation.
- Token validation.
- Guard/middleware.
- Permission check.

---

# 3. AI Implementation Rule

AI harus memisahkan:

- Authentication.
- Authorization.
- User profile.

---

# 4. Security Requirement

Wajib:

- Expiration handling.
- Refresh strategy.
- Failed attempt tracking.
- Audit logging.

---

# 5. Validation

Flow selesai jika:

- User dapat login.
- Protected API terlindungi.
- Permission bekerja.
