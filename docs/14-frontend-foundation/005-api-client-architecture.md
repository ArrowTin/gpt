# ChannelHub API Client Architecture Blueprint

## Purpose

Mendefinisikan standar komunikasi frontend dengan backend API.

---

## AI TRIGGER

### Tujuan Task
Memahami dan mengimplementasikan API client layer untuk komunikasi frontend-backend yang konsisten.

### Konteks yang Perlu Dipahami AI
- Architecture: Feature Module → API Client Layer → HTTP Client → API Gateway
- API Client Responsibility: Request configuration, Authentication token, Error normalization, Response mapping
- Rule: Frontend tidak boleh memanggil endpoint langsung dari component, tidak boleh membuat logic API berulang
- Goal: Komunikasi frontend-backend konsisten dan mudah dipelihara

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- contracts/openapi/channelhub.v1.yaml (API contract)
- docs/16-api-contract/009-api-endpoint-specification.md (endpoint spec)

### File/Folder yang Perlu Diperiksa
- docs/14-frontend-foundation/009-frontend-project-structure.md (struktur project)
- docs/16-api-contract/002-api-response-format.md (response format)

### Langkah Implementasi
1. Buat API client wrapper di src/lib/api/client.ts
2. Generate types dari OpenAPI contract ke src/lib/api/generated/
3. Implementasikan error normalization dan mapping
4. Pastikan seluruh pemanggilan API melalui API client layer

### Kriteria Keberhasilan (Definition of Done)
- API client wrapper berfungsi dengan proper configuration
- Types dari OpenAPI contract ter-generate
- Error normalization berfungsi
- Tidak ada fetch langsung dari component

### Prompt Implementasi
```
Anda akan mengimplementasikan API client layer frontend ChannelHub.

Baca docs/14-frontend-foundation/005-api-client-architecture.md untuk memahami arsitektur API client.

Architecture:
Feature Module → API Client Layer → HTTP Client → API Gateway

API Client Responsibility:
- Request configuration (base URL, headers, timeout)
- Authentication token (JWT dengan refresh mechanism)
- Error normalization (mapping error.code → pesan UI)
- Response mapping (envelope unwrap, data transformation)

Rules (WAJIB):
- Frontend TIDAK BOLEH memanggil endpoint langsung dari component
- Frontend TIDAK BOLEH membuat logic API berulang
- Seluruh pemanggilan API HANYA melalui src/lib/api/
- Types request/response digenerate dari OpenAPI contract

Implementasikan:
1. API client wrapper di src/lib/api/client.ts:
   - Fetch wrapper dengan proper configuration
   - Token injection (JWT di Authorization header)
   - Tenant ID injection (X-Tenant-Id header)
   - Request/response interceptor
   - Error handling dengan retry logic

2. Generate types dari contracts/openapi/channelhub.v1.yaml:
   - Gunakan openapi-typescript atau similar
   - Output ke src/lib/api/generated/
   - JANGAN edit file generated secara manual

3. Error normalization di src/lib/api/errors.ts:
   - Mapping error.code dari backend ke pesan UI
   - Centralized error handling

4. Feature-specific API di features/<domain>/api/<domain>.api.ts:
   - Wrapper functions untuk endpoint spesifik
   - Gunakan types dari generated/
   - Gunakan client.ts untuk HTTP request

Pastikan komunikasi frontend-backend konsisten dan mudah dipelihara.
```

---

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
