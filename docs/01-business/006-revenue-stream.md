# ChannelHub Revenue Stream Definition

## Purpose

Dokumen ini menjelaskan sumber pendapatan ChannelHub dan bagaimana nilai bisnis dikonversi menjadi revenue.

---

# Revenue Strategy Overview

ChannelHub tidak bergantung pada satu sumber pendapatan.

Strategi revenue menggunakan beberapa layer:

```
Primary Revenue
      |
      ├── Subscription
      |
      └── Usage Based

Secondary Revenue
      |
      ├── Premium Feature
      ├── Marketplace
      └── Partner Revenue

Long Term Revenue
      |
      └── Ecosystem Transaction
```

---

# 1. Subscription Revenue

Subscription menjadi sumber pendapatan utama.

User membayar akses terhadap platform ChannelHub.

Contoh:

- Starter Plan.
- Professional Plan.
- Business Plan.
- Enterprise Plan.

Subscription memberikan akses terhadap:

- Dashboard.
- Property management.
- OTA integration.
- Reporting.
- Automation sesuai paket.

---

# 2. Usage Based Revenue

Biaya berdasarkan penggunaan layanan tertentu.

Contoh:

## Synchronization Usage

Jika subscriber menggunakan volume sinkronisasi tinggi, sistem dapat menggunakan model kredit.

Contoh:

```
Subscription
     +
Sync Credit
     =
ChannelHub Usage Billing
```

## Resource Based Usage

Contoh:

- Jumlah property.
- Jumlah kamar.
- Jumlah user.
- Jumlah transaksi.

---

# 3. Premium Feature Revenue

Fitur tambahan yang meningkatkan nilai bisnis.

Contoh:

## Analytics

- Revenue analytics.
- OTA performance.
- Forecasting.

## Automation

- Automated pricing.
- Automated report.
- Workflow automation.

## AI Feature

- AI assistant.
- Market analysis.
- Business recommendation.

## Marketing Feature

- Campaign management.
- Promotion tools.
- Property exposure.

---

# 4. Transaction Ecosystem Revenue

ChannelHub dapat memperoleh revenue dari aktivitas transaksi ecosystem.

Namun model ini harus memperhatikan sumber transaksi.

## Booking melalui OTA

Flow:

```
Guest
  |
OTA
  |
Property
  |
ChannelHub
```

Pendapatan dapat berasal dari:

- Partnership.
- Revenue sharing.
- Integration value.

## Booking langsung property

Flow:

```
Guest
  |
Property
```

ChannelHub tidak otomatis mengambil komisi transaksi.

Pendapatan berasal dari:

- Subscription.
- Premium service.
- Operational tools.

---

# 5. Marketplace Revenue

Jika ChannelHub memiliki marketplace sendiri:

Revenue dapat berasal dari:

- Commission.
- Featured listing.
- Promotion placement.

---

# 6. Partner Revenue

Ekosistem partner dapat menghasilkan pendapatan melalui:

- OTA partnership.
- Payment provider.
- Hospitality vendor.
- Technology integration.

---

# Revenue Principle

ChannelHub harus memberikan nilai sebelum mengambil revenue.

Prinsip:

```
More operational value
        ↓
More platform usage
        ↓
More revenue opportunity
```

---

# Technical Requirement

Revenue model harus configurable.

Database harus mampu menyimpan:

- Pricing rule.
- Subscription plan.
- Feature entitlement.
- Usage quota.
- Billing cycle.
- Revenue report.

Tidak boleh hardcoded di aplikasi.
