# ChannelHub Frontend Authentication Flow Blueprint

## Purpose

Mendefinisikan alur autentikasi pada aplikasi frontend.

---

# Authentication Flow

```
User Login
     |
API Request
     |
Token Received
     |
Session Management
     |
Protected Access
```

---

# Responsibility

Frontend menangani:

- Login interface.
- Session state.
- Protected route.
- Authentication status.

---

# Security Rule

Frontend tidak menjadi sumber otorisasi utama.

Backend tetap melakukan validasi permission.

---

# Goal

Menyediakan pengalaman login yang aman dan konsisten.
