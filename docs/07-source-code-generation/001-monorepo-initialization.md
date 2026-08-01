# ChannelHub Monorepo Initialization Blueprint

## Purpose

Mendefinisikan struktur awal repository source code sebelum implementasi aplikasi.

---

# 1. Repository Strategy

Menggunakan monorepo agar frontend, backend, shared package, dan infrastructure tetap sinkron.

---

# 2. Target Structure

```
channelhub/

apps/
  web/
  api-gateway/
  worker/

services/
  identity/
  organization/
  property/
  reservation/
  ota/

packages/
  shared-types/
  config/
  logger/

infrastructure/

```

---

# 3. Generation Rule

AI harus membuat struktur terlebih dahulu sebelum feature coding.

---

# 4. Validation

Monorepo siap jika:

- Dependency management aktif.
- Build system berjalan.
- Shared package dapat digunakan.
- Semua app dapat dijalankan.
