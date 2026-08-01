# ChannelHub OTA Database Schema Blueprint

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
