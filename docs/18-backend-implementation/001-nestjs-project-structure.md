# ChannelHub NestJS Project Structure Blueprint

## Purpose

Menetapkan struktur kode backend menggunakan NestJS agar scalable dan mudah dipelihara.

---

# Project Structure

```
backend/
 |
 +-- src/
 |    +-- modules/
 |    +-- common/
 |    +-- config/
 |    +-- database/
 |    +-- main.ts
 |
 +-- test/
```

---

# Module Principle

Setiap domain memiliki module sendiri:

- User.
- Property.
- Reservation.
- Channel Sync.

---

# Rule

Kode harus:

- Terpisah berdasarkan domain.
- Tidak mencampur business logic.
- Mudah dikembangkan AI agent.

---

# Goal

Membuat fondasi backend production-ready.
