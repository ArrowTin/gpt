# ChannelHub Transaction Fee Model

## Purpose

Dokumen ini mendefinisikan model biaya transaksi ChannelHub, sumber pendapatan transaksi, dan hubungan finansial antara ChannelHub, OTA, property owner, dan payment provider.

---

# 1. Prinsip Transaction Fee

ChannelHub tidak bergantung hanya pada transaction fee.

Revenue utama tetap:

- Subscription.
- Usage based service.
- Premium feature.
- Ecosystem revenue.

Transaction fee menjadi sumber pendapatan tambahan ketika ChannelHub memiliki peran langsung dalam transaksi.

---

# 2. Jenis Transaksi

## A. Booking Melalui OTA Eksternal

Flow:

```
Guest
  |
  v
OTA External
  |
  v
Property
  |
  v
ChannelHub Synchronization
```

Pada kondisi ini:

- OTA memiliki aturan komisi sendiri.
- ChannelHub tidak otomatis mengambil komisi booking.
- Revenue ChannelHub berasal dari subscription dan layanan platform.

Potensi revenue:

- Partnership.
- Integration fee.
- Revenue sharing agreement.

---

## B. Booking Melalui ChannelHub OTA

Jika ChannelHub memiliki marketplace sendiri:

Flow:

```
Guest
  |
  v
ChannelHub OTA
  |
  v
Property
```

Model revenue:

- Commission per transaction.
- Service fee.
- Featured placement.
- Promotion fee.

---

## C. Direct Booking Property

Flow:

```
Guest
  |
  v
Property
```

ChannelHub tidak mengambil transaksi secara otomatis.

Revenue berasal dari:

- Subscription.
- Premium tools.
- Automation service.

---

# 3. Commission Engine

Jika transaction fee digunakan, sistem membutuhkan:

```
Commission Engine

Input:
- Transaction amount
- Channel source
- Property
- Contract rule

Output:
- Commission amount
- Settlement amount
```

---

# 4. Settlement Flow

```
Transaction
      |
      v
Transaction Ledger
      |
      v
Commission Calculation
      |
      v
Settlement Report
      |
      v
Payment Process
```

---

# 5. Transaction Ledger

Semua transaksi harus tercatat.

Data:

- Transaction ID.
- Source channel.
- Property.
- Amount.
- Fee.
- Commission.
- Settlement status.
- Timestamp.

---

# 6. Reconciliation

Sistem harus mendukung:

- Matching transaksi.
- Verifikasi pembayaran.
- Perbedaan nilai transaksi.
- Audit history.

---

# 7. Technical Requirement

Domain yang diperlukan:

```
transaction-service
commission-service
settlement-service
payment-service
reporting-service
```

---

# 8. Business Rule

Transaction fee harus configurable.

Tidak boleh hardcoded.

Konfigurasi:

- Fee percentage.
- Fixed fee.
- Channel rule.
- Partner agreement.
- Effective date.

---

# 9. Strategic Direction

Tahapan bisnis:

Phase 1:

Subscription first.

Phase 2:

Premium ecosystem.

Phase 3:

Transaction marketplace revenue.

ChannelHub harus membangun nilai operasional sebelum mengambil bagian transaksi.
