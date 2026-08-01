# ChannelHub Identity Database Schema Blueprint

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
