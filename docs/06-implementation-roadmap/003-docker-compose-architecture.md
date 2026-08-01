# ChannelHub Docker Compose Architecture

## Purpose

Mendefinisikan arsitektur container untuk development dan testing.

---

# 1. Container Overview

```
Frontend
   |
API Gateway
   |
Services
   |
PostgreSQL
   |
Redis
```

---

# 2. Compose Responsibility

Docker Compose mengatur:

- Service startup.
- Network.
- Volume.
- Environment.
- Health check.

---

# 3. Required Services

Awal:

```
web
api-gateway
postgres
redis
worker
```

---

# 4. Persistence

Data penting menggunakan:

- Named volume.
- Backup strategy.
- Migration process.

---

# 5. Development Rule

Satu command harus mampu menjalankan seluruh stack.

Contoh:

```
docker compose up -d
```

---

# 6. Production Direction

Compose menjadi referensi awal sebelum migrasi ke orchestrator.
