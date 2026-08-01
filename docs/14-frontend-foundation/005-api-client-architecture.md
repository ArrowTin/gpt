# ChannelHub API Client Architecture Blueprint

## Purpose

Mendefinisikan standar komunikasi frontend dengan backend API.

---

# Architecture

```
Feature Module
      |
API Client Layer
      |
HTTP Client
      |
API Gateway
```

---

# Responsibility

API Client menangani:

- Request configuration.
- Authentication token.
- Error normalization.
- Response mapping.

---

# Rule

Frontend tidak boleh:

- Memanggil endpoint secara langsung dari component.
- Membuat logic API berulang.

---

# Goal

Membuat komunikasi frontend-backend konsisten dan mudah dipelihara.
