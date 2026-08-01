# ChannelHub Identity Service Generation Blueprint

## Purpose

Menjadi panduan AI untuk membangun service pertama yang menjadi fondasi keamanan sistem.

---

# 1. Responsibility

Identity Service menangani:

- User identity.
- Authentication.
- Role.
- Permission.
- Session.

---

# 2. Architecture Position

```
Frontend
   |
API Gateway
   |
Identity Service
   |
Identity Database
```

---

# 3. AI Generation Requirement

AI wajib membuat:

- Module structure.
- Entity.
- Repository layer.
- Service layer.
- Controller/API.
- Validation.
- Test.

---

# 4. Security Rule

Tidak menyimpan password plain text.

Wajib:

- Password hashing.
- Token expiration.
- Audit event.

---

# 5. Validation

Identity Service selesai jika:

- Register berjalan.
- Login berjalan.
- Token validation berjalan.
- Permission check tersedia.
