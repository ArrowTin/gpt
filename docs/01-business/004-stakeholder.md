# ChannelHub Stakeholder Definition

## Purpose

Dokumen ini mendefinisikan seluruh aktor yang terlibat dalam ekosistem bisnis ChannelHub, tanggung jawabnya, dan hubungan antar pihak.

---

# 1. ChannelHub Internal Organization

## Super Admin

Super Admin adalah role tertinggi pada platform ChannelHub.

Tanggung jawab:

- Mengelola seluruh tenant.
- Mengelola role dan permission.
- Mengatur menu dinamis berdasarkan permission.
- Mengatur konfigurasi platform.
- Melihat kondisi infrastruktur.
- Melihat audit log.
- Mengelola feature flag.
- Mengelola subscription plan.

Approval yang diperlukan:

- Perubahan permission global.
- Perubahan konfigurasi sistem.
- Aktivasi fitur besar.

---

## Technical Admin / DevOps

Bertanggung jawab terhadap operasional teknis.

Tanggung jawab:

- Monitoring server.
- Monitoring service.
- Deployment.
- Scaling.
- Log management.
- Incident handling.
- Performance analysis.

Data yang dipantau:

- CPU usage.
- Memory usage.
- Database performance.
- Queue health.
- API latency.
- Error rate.

---

## Developer

Bertanggung jawab terhadap pengembangan sistem.

Tanggung jawab:

- Implementasi fitur.
- Maintenance code.
- Testing.
- Bug fixing.
- Dokumentasi teknis.

Tidak memiliki akses langsung terhadap konfigurasi production tanpa approval.

---

## Finance

Bertanggung jawab terhadap transaksi bisnis.

Tanggung jawab:

- Billing.
- Invoice.
- Revenue report.
- Subscription payment.
- Refund process.
- Financial reconciliation.

---

## Marketing Staff

Marketing bukan hanya melakukan promosi eksternal, tetapi mengelola pertumbuhan ecosystem.

Tanggung jawab:

- Mengelola campaign.
- Mengatur promotional feature.
- Membantu subscriber mendapatkan exposure.
- Mengelola iklan internal platform.
- Mengelola partnership marketing.

Aktivitas marketing berbayar harus melalui fitur campaign atau advertising module.

---

## Customer Support

Customer Support menangani kebutuhan pengguna.

Tanggung jawab:

- Membantu onboarding.
- Menangani pertanyaan pengguna.
- Membantu troubleshooting awal.
- Eskalasi masalah teknis.

CS tidak perlu memahami seluruh kode teknis, karena sistem harus menyediakan:

- Knowledge base.
- Error code.
- Ticket system.
- Audit history.

---

# 2. Subscriber Organization

## Organization Owner

Pemilik akun bisnis pada ChannelHub.

Tanggung jawab:

- Mengelola organisasi.
- Mengatur pengguna internal.
- Memilih subscription.
- Menyetujui koneksi OTA.
- Melihat laporan bisnis.

---

## Property Owner

Pemilik properti yang dijual melalui channel distribusi.

Tanggung jawab:

- Data properti.
- Informasi kamar.
- Harga.
- Kebijakan reservasi.

---

## Property Manager

Mengelola operasional harian properti.

Tanggung jawab:

- Update inventory.
- Mengelola reservasi.
- Monitoring channel.
- Mengelola availability.

---

## Property Staff

Role operasional dengan akses terbatas.

Contoh:

- Front desk.
- Operator reservasi.
- Housekeeping coordinator.

---

# 3. External Partner

## OTA Partner

Contoh:

- Booking.com
- Agoda
- Traveloka
- Airbnb
- Expedia
- Tiket.com

Peran:

- Menyediakan channel distribusi.
- Menerima inventory.
- Mengirim reservasi.

---

## Payment Provider

Menyediakan layanan pembayaran:

- Subscription payment.
- Wallet.
- Settlement.

---

# 4. Guest

Guest adalah pelanggan akhir yang melakukan reservasi.

Guest dapat berasal dari:

- OTA.
- Website property.
- ChannelHub marketplace.

---

# Approval Principle

Semakin tinggi dampak perubahan, semakin tinggi level approval yang diperlukan.

Contoh:

```
Property Staff
      ↓
Property Manager
      ↓
Organization Owner
      ↓
ChannelHub Support/Admin
      ↓
Super Admin
```

## Design Rule

Role dan permission harus bersifat dinamis dan tidak hardcoded.

Sistem harus mendukung:

- Custom role.
- Custom permission.
- Menu visibility.
- Feature access control.
