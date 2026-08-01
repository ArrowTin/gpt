# ChannelHub Service Folder Standard

## Purpose

Mendefinisikan standar folder agar semua microservice memiliki pola yang sama.

---

# 1. Standard Structure

```
service-name/

src/
  modules/
  common/
  config/
  database/
  events/
  main.ts

test/
docker/
README.md
```

---

# 2. Module Structure

```
module/
  controller/
  service/
  repository/
  dto/
  entity/
  events/
```

---

# 3. Configuration

Semua konfigurasi melalui environment/config service.

Tidak hardcode.

---

# 4. Required Files

Setiap service wajib memiliki:

- README.
- Dockerfile.
- Test configuration.
- Health check.
- API documentation.

---

# 5. Maintenance Principle

Standar folder harus sama agar developer mudah memahami service baru.
