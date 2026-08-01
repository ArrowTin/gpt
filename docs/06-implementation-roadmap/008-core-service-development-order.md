# ChannelHub Core Service Development Order

## Purpose

Menentukan urutan pembangunan service agar dependency tetap sehat.

---

# 1. Development Sequence

```
Infrastructure
      |
Identity Service
      |
Organization Service
      |
Property Service
      |
Reservation Service
      |
OTA Service
      |
Notification Service
      |
Reporting Service
```

---

# 2. Reasoning

Setiap service dibangun berdasarkan dependency sebelumnya.

---

# 3. Service Rule

Setiap service harus memiliki:

- Database ownership.
- API contract.
- Test.
- Documentation.
- Monitoring.

---

# 4. AI Execution Rule

AI mengerjakan satu service pada satu waktu.

Tidak membuat seluruh domain sekaligus.

---

# 5. Completion Criteria

Service selesai jika:

- Implementasi tersedia.
- Terintegrasi.
- Teruji.
- Terdokumentasi.
