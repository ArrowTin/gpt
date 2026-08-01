# ChannelHub API Gateway Architecture

## Purpose

API Gateway menjadi pintu masuk seluruh request aplikasi menuju backend services.

---

# Responsibility

- Request routing.
- Authentication validation.
- Authorization check.
- Rate limiting.
- Request logging.
- API versioning.

---

# Flow

```
Frontend
   |
   v
API Gateway
   |
   +--> Identity Service
   +--> Property Service
   +--> OTA Service
   +--> Billing Service
```

---

# Security Layer

Gateway menangani:

- JWT validation.
- Permission verification.
- Tenant isolation.
- Request filtering.

---

# Design Principle

Gateway tidak berisi business logic utama.

Business rule tetap berada pada masing-masing domain service.
