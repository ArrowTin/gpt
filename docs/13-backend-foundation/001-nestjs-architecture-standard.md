# ChannelHub NestJS Architecture Standard Blueprint

## Purpose

Menetapkan standar arsitektur backend menggunakan NestJS.

---

# Backend Architecture

```
API Gateway
     |
Application Modules
     |
Domain Services
     |
Data Access Layer
     |
Database
```

---

# Layer Responsibility

## Controller

Menangani:

- HTTP request.
- Validation input.
- Response formatting.

## Service

Menangani:

- Business logic.
- Transaction flow.
- Domain operation.

## Repository

Menangani:

- Database access.
- Query abstraction.

---

# Rule

Backend wajib:

- Modular.
- Testable.
- Scalable.
- Mengikuti domain boundary.
