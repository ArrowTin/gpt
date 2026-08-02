# ChannelHub Identity Database Schema Blueprint

> **Status: konseptual.** Dokumen ini menjelaskan pemikiran domain pada Phase 09.
> Sumber kebenaran implementasi adalah contract artifact:
> [contracts/openapi/channelhub.v1.yaml](../../contracts/openapi/channelhub.v1.yaml),
> [docs/15-database-implementation/009-canonical-erd.md](../15-database-implementation/009-canonical-erd.md),
> [docs/15-database-implementation/010-postgresql-ddl-reference.md](../15-database-implementation/010-postgresql-ddl-reference.md).
> Bila terjadi perbedaan, contract artifact yang berlaku.

## Purpose

Mendefinisikan schema database untuk Identity Service.

---

# Tables

## users

Menyimpan identitas pengguna.

Fields utama:

- id.
- email.
- password_hash.
- status.
- created_at.

---

## roles

Menyimpan hak akses kelompok pengguna.

---

## permissions

Menyimpan izin sistem.

---

## user_roles

Relasi user dan role.

---

## audit_logs

Mencatat aktivitas penting.

---

# Security Rule

Data sensitif harus:

- Dienkripsi bila diperlukan.
- Memiliki audit.
- Tidak menyimpan credential asli.

---

# Validation

Schema siap jika:

- Mendukung login.
- Mendukung authorization.
- Mendukung audit.
