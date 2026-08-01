# ChannelHub DTO Validation Standard Blueprint

## Purpose

Menetapkan standar validasi data masuk ke backend.

---

# Request Flow

```
Client Request
      |
DTO Validation
      |
Controller
      |
Service
```

---

# Validation Responsibility

DTO menangani:

- Data type.
- Required field.
- Format validation.
- Input constraint.

---

# Rule

Business rule tidak ditempatkan di DTO.

DTO hanya menjaga kontrak input.

---

# Goal

Data yang masuk sistem selalu tervalidasi.
