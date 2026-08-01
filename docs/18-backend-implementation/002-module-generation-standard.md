# ChannelHub Module Generation Standard Blueprint

## Purpose

Menetapkan standar pembuatan module backend.

---

# Module Structure

```
feature/
 |
 +-- controller
 +-- service
 +-- dto
 +-- entity
 +-- repository
 +-- test
```

---

# Responsibility

Controller:
- Handle request.

Service:
- Business logic.

Repository:
- Data access.

DTO:
- Contract validation.

---

# Goal

Setiap module memiliki pola yang konsisten.
