# ChannelHub Machine-Readable Contracts

## Purpose

Menyimpan kontrak yang dapat dibaca mesin dan dipakai langsung oleh generator kode, mock server, dan contract test.

## Isi

| File | Kontrak |
| --- | --- |
| [openapi/channelhub.v1.yaml](./openapi/channelhub.v1.yaml) | REST API v1 ChannelHub (OpenAPI 3.1) |

## Rules

- File di folder ini adalah sumber kebenaran teknis; dokumen naratif di `/docs` menjelaskannya, bukan menggantikannya.
- Perubahan kontrak wajib disertai pembaruan dokumen terkait pada [docs/16-api-contract/](../docs/16-api-contract/) dan, bila mengubah perilaku, satu ADR.
- Breaking change tidak boleh masuk ke `v1`; buat file versi baru sesuai [docs/16-api-contract/007-api-versioning-strategy.md](../docs/16-api-contract/007-api-versioning-strategy.md).
- Validasi spec sebelum commit, contoh: `npx @redocly/cli lint contracts/openapi/channelhub.v1.yaml`.

## Pemakaian

| Kebutuhan | Cara |
| --- | --- |
| Client TypeScript | generate dari spec ini ke `apps/frontend/src/lib/api/generated` |
| DTO backend | turunkan skema komponen menjadi DTO NestJS + validator |
| Mock server | jalankan mock dari spec untuk pengembangan frontend paralel |
| Contract test | bandingkan response CI terhadap spec |

## References

- [docs/16-api-contract/009-api-endpoint-specification.md](../docs/16-api-contract/009-api-endpoint-specification.md)
- [docs/16-api-contract/010-error-code-catalog.md](../docs/16-api-contract/010-error-code-catalog.md)
- [adr/ADR-008-api-first-rest-contract.md](../adr/ADR-008-api-first-rest-contract.md)
