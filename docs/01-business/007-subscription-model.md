# ChannelHub Subscription Model

## Purpose

Dokumen ini mendefinisikan model subscription ChannelHub sebagai mekanisme pengelolaan akses platform, fitur, kapasitas, dan penggunaan layanan.

Subscription bukan hanya pembayaran aplikasi, tetapi menjadi dasar pengaturan entitlement pengguna.

---

# 1. Subscription Concept

ChannelHub menggunakan kombinasi model:

- SaaS Subscription.
- Usage Based Billing.
- Service Credit.
- Premium Feature.

Model:

```
Subscriber
    |
    v
Subscription Plan
    |
    v
Feature Entitlement
    |
    v
Usage Control
```

---

# 2. Subscription Plan

Plan harus bersifat dinamis dan dapat dikonfigurasi.

Contoh:

## Starter

- Basic property management.
- Limited OTA connection.
- Basic reporting.

## Professional

- Multiple OTA.
- Advanced synchronization.
- Analytics.
- Automation.

## Business

- Multiple user.
- Advanced permission.
- API access.

## Enterprise

- Custom contract.
- Dedicated support.
- Custom integration.

Plan tidak boleh hardcoded dalam aplikasi.

Data disimpan sebagai konfigurasi:

```
subscription_plans

id
name
price
billing_cycle
features
limits
status
```

---

# 3. Feature Entitlement

Feature access dikontrol melalui entitlement.

Contoh:

```
Plan Professional

allowed_features:
- ota_sync
- analytics
- automation
```

Sistem menentukan fitur berdasarkan:

- Subscription.
- Permission.
- Role.
- Quota.

---

# 4. Credit Wallet

ChannelHub dapat menggunakan service credit wallet.

Wallet ini bukan dompet uang.

Fungsi:

- Sinkronisasi OTA.
- AI usage.
- Automation processing.
- Premium service usage.

Contoh:

```
Initial Credit: 100000

OTA Sync Usage:
-500

Remaining:
99500
```

---

# 5. Payment Architecture

ChannelHub tidak menjadi penyimpan uang utama.

Flow:

```
Payment Provider
        |
        v
Billing Service
        |
        +---- Subscription Service
        |
        +---- Credit Wallet Service
        |
        +---- Invoice Service
```

---

# 6. Subscription Lifecycle

Status subscription:

```
TRIAL
  |
  v
ACTIVE
  |
  v
EXPIRING
  |
  v
EXPIRED
  |
  v
SUSPENDED
  |
  v
CANCELLED
```

---

# 7. Notification

Sistem memberikan notifikasi:

- Subscription akan habis.
- Credit hampir habis.
- Pembayaran gagal.
- Renewal berhasil.

---

# 8. Upgrade and Downgrade

Perubahan plan harus mendukung:

- Upgrade.
- Downgrade.
- Prorated billing.
- Perubahan feature access.
- Perubahan quota.

---

# 9. Billing History

Subscriber dapat melihat:

- Invoice.
- Payment history.
- Subscription history.
- Credit usage.
- Refund history.

---

# 10. Technical Domain Requirement

Subscription dipisahkan menjadi domain service:

```
subscription-service
billing-service
payment-service
wallet-service
notification-service
```

---

# Design Principle

Semua aturan subscription harus configurable.

Tidak boleh:

```
if(plan == premium)
```

Tetapi:

```
Database Configuration
        |
        v
Entitlement Engine
        |
        v
Feature Access
```
