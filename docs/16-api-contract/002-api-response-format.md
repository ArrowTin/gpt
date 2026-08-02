# ChannelHub API Response Format

## Purpose

Menetapkan **envelope response** yang dipakai seluruh endpoint ChannelHub agar client dapat menangani sukses, error, dan pagination secara seragam.

## Scope

Seluruh response API v1, termasuk error dari guard, validation pipe, dan exception filter.

## Context

Skema formal ada pada [contracts/openapi/channelhub.v1.yaml](../../contracts/openapi/channelhub.v1.yaml) (`SuccessEnvelope`, `PaginatedEnvelope`, `ErrorEnvelope`). Daftar kode error ada pada [010-error-code-catalog.md](./010-error-code-catalog.md).

## Rules

- Semua response memakai envelope; tidak ada endpoint yang mengembalikan array atau primitif telanjang.
- `success` menentukan cabang penanganan client: `true` → baca `data`, `false` → baca `error.code`.
- `metadata.requestId` selalu diisi dan dicatat pada log agar dapat ditelusuri.
- `204 No Content` tidak memakai body.
- Penamaan field JSON memakai `camelCase`.

## Technical Details

### Struktur

```json
{
  "success": true,
  "data": {},
  "message": null,
  "error": null,
  "metadata": {
    "requestId": "uuid",
    "correlationId": "uuid",
    "timestamp": "2026-08-02T10:15:30.000Z"
  }
}
```

### Success dengan pagination

```json
{
  "success": true,
  "data": [
    { "id": "3f1c...", "reservationCode": "RSV-2026-000123", "status": "CONFIRMED" }
  ],
  "message": null,
  "error": null,
  "metadata": {
    "requestId": "uuid",
    "timestamp": "2026-08-02T10:15:30.000Z",
    "page": 1,
    "pageSize": 20,
    "total": 137,
    "totalPages": 7
  }
}
```

### Error

```json
{
  "success": false,
  "data": null,
  "message": "Payload tidak valid",
  "error": {
    "code": "VALIDATION_FAILED",
    "details": [
      { "field": "checkOut", "rule": "afterCheckIn", "message": "checkOut harus setelah checkIn" }
    ]
  },
  "metadata": {
    "requestId": "uuid",
    "timestamp": "2026-08-02T10:15:30.000Z"
  }
}
```

### HTTP status yang dipakai

| Status | Dipakai untuk |
| --- | --- |
| 200 | Operasi baca dan update sinkron berhasil |
| 201 | Resource baru dibuat |
| 202 | Diterima untuk diproses asinkron (sync job, webhook) |
| 204 | Berhasil tanpa body (logout, mark as read, soft delete) |
| 400 | Request malformed atau header wajib hilang |
| 401 / 403 | Autentikasi gagal / permission & entitlement kurang |
| 404 | Tidak ditemukan pada tenant aktif |
| 409 | Konflik state, versi, atau constraint |
| 422 | Payload valid secara sintaks tetapi gagal validasi bisnis |
| 429 | Rate limit atau kuota habis |
| 5xx | Kegagalan internal atau dependency |

### Implementasi

Backend memakai satu response interceptor untuk membungkus `data` dan satu global exception filter untuk membentuk envelope error, sehingga controller mengembalikan objek domain apa adanya ([docs/13-backend-foundation/002-backend-module-design.md](../13-backend-foundation/002-backend-module-design.md)).

## Impact

- [docs/14-frontend-foundation/005-api-client-architecture.md](../14-frontend-foundation/005-api-client-architecture.md) — API client membuka envelope satu kali di layer HTTP.
- [checklists/code-review.md](../../checklists/code-review.md) — endpoint tanpa envelope ditolak review.

## References

- [009-api-endpoint-specification.md](./009-api-endpoint-specification.md)
- [010-error-code-catalog.md](./010-error-code-catalog.md)
- [adr/ADR-008-api-first-rest-contract.md](../../adr/ADR-008-api-first-rest-contract.md)
