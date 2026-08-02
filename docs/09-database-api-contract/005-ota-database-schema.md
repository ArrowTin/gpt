# ChannelHub OTA Database Schema Blueprint

> **Status: konseptual.** Dokumen ini menjelaskan pemikiran domain pada Phase 09.
> Sumber kebenaran implementasi adalah contract artifact:
> [contracts/openapi/channelhub.v1.yaml](../../contracts/openapi/channelhub.v1.yaml),
> [docs/15-database-implementation/009-canonical-erd.md](../15-database-implementation/009-canonical-erd.md),
> [docs/15-database-implementation/010-postgresql-ddl-reference.md](../15-database-implementation/010-postgresql-ddl-reference.md).
> Bila terjadi perbedaan, contract artifact yang berlaku.

## Purpose

Mendefinisikan penyimpanan data integrasi OTA dan proses sinkronisasi.

---

# Tables

## ota_channels

Menyimpan daftar channel OTA.

Fields:

- id.
- name.
- type.
- status.

---

## channel_connections

Menyimpan konfigurasi koneksi channel.

---

## sync_jobs

Menyimpan pekerjaan sinkronisasi.

---

## webhook_events

Menyimpan event masuk dari OTA.

---

## sync_logs

Menyimpan histori proses sinkronisasi.

---

# Sync Principle

Wajib mendukung:

- Retry.
- Idempotency.
- Audit.
- Failure recovery.

---

# Validation

Schema siap jika:

- Multiple OTA channel dapat dikelola.
- Sync history tercatat.
- Error dapat ditelusuri.
