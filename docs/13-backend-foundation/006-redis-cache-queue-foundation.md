# ChannelHub Redis Cache Queue Foundation Blueprint

## Purpose

Mendefinisikan penggunaan Redis untuk cache dan asynchronous processing.

---

# Redis Usage

```
Application
     |
Redis
     |
Cache / Queue / Session
```

---

# Cache Responsibility

Digunakan untuk:

- Data yang sering dibaca.
- Temporary state.
- Performance improvement.

---

# Queue Responsibility

Digunakan untuk:

- Background job.
- OTA synchronization.
- Retry mechanism.
- Event processing.

---

# Rule

Redis bukan sumber data utama.

Database tetap menjadi source of truth.
