# ChannelHub Development Environment Setup

## Purpose

Mendefinisikan standar environment developer sebelum implementasi source code dimulai.

---

# 1. Required Tools

Developer environment:

- Git.
- Node.js LTS.
- pnpm.
- Docker.
- PostgreSQL client.
- Redis client.

---

# 2. Development Principle

Semua developer menggunakan konfigurasi yang konsisten.

Tujuan:

- Mengurangi perbedaan environment.
- Memudahkan onboarding.
- Memastikan hasil AI generation dapat dijalankan.

---

# 3. Environment Layers

```
Local Development
        |
Docker Runtime
        |
Shared Service
        |
Production Equivalent
```

---

# 4. Configuration Rule

Tidak ada credential di source code.

Menggunakan:

- Environment variable.
- Example configuration.
- Secret management.

---

# 5. Developer Validation

Sebelum coding:

- Repository berhasil clone.
- Container berjalan.
- Database terkoneksi.
- Application dapat start.
