# ChannelHub User Role Hierarchy

## Purpose

Mendefinisikan struktur role pengguna ChannelHub agar organisasi dapat dikelola dengan kontrol akses yang jelas.

---

# 1. Role Hierarchy

```
Super Admin
      |
Organization Owner
      |
Administrator
      |
Manager
      |
Staff
```

---

# 2. Super Admin

Pemilik kontrol platform ChannelHub.

Tanggung jawab:

- Mengelola seluruh tenant.
- Mengatur role dan permission.
- Mengatur konfigurasi sistem.
- Monitoring infrastructure.
- Audit aktivitas.

Tidak mengelola operasional properti harian.

---

# 3. Organization Owner

Pemilik akun bisnis.

Tanggung jawab:

- Membuat organisasi.
- Mengatur anggota.
- Mengelola subscription.
- Melihat laporan bisnis.
- Menyetujui tindakan penting.

---

# 4. Administrator

Mengelola konfigurasi operasional organisasi.

Tanggung jawab:

- User management.
- Property configuration.
- Permission assignment sesuai batas owner.

---

# 5. Manager

Mengelola operasional harian.

Tanggung jawab:

- Reservation monitoring.
- Inventory control.
- Staff supervision.
- Operational report.

---

# 6. Staff

Melakukan aktivitas operasional.

Contoh:

- Front desk.
- Customer service.
- Marketing staff.

Akses berdasarkan permission.

---

# 7. Principle

Role bukan menentukan semua akses.

Akses final ditentukan oleh:

```
Role
+
Permission
+
Feature Entitlement
+
Organization Policy
```
