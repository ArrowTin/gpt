# ChannelHub Data Seed Strategy Blueprint

## Purpose

Mendefinisikan pengelolaan data awal dan data testing.

---

## AI TRIGGER

### Tujuan Task
Memahami strategy data seed untuk persiapan environment yang cepat dan reproducible.

### Konteks yang Perlu Dipahami AI
- Seed Flow: Seed Definition → Data Generation → Database Insert → Validation
- Seed Category: Reference Data, Development Data, Testing Data
- Rule: Seed harus konsisten, dapat dijalankan ulang, tidak menggunakan data sensitif
- Goal: Environment dapat dipersiapkan secara cepat dan reproducible

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/15-database-implementation/010-postgresql-ddl-reference.md (DDL reference)

### File/Folder yang Perlu Diperiksa
- docs/15-database-implementation/003-database-migration-strategy.md (migration strategy)

### Langkah Implementasi
1. Definisikan seed data untuk reference data (permissions, roles, ota_channels)
2. Buat seed data untuk development (sample organizations, properties)
3. Buat seed data untuk testing (test users, test reservations)
4. Pastikan seed dapat dijalankan ulang (idempotent)

### Kriteria Keberhasilan (Definition of Done)
- Reference data ter-seed dengan benar
- Development data tersedia untuk testing
- Seed dapat dijalankan ulang tanpa error
- Tidak ada data sensitif di seed

### Prompt Implementasi
```
Anda akan membuat atau mengelola data seed ChannelHub.

Baca docs/15-database-implementation/004-data-seed-strategy.md untuk memahami strategy seed.

Seed Flow:
Seed Definition → Data Generation → Database Insert → Validation

Seed Category:
- Reference Data: permissions, roles, ota_channels, subscription_plans, booking_sources, feature_flags, menus, system_configurations, notification_templates
- Development Data: sample organizations, properties, users, reservations untuk development
- Testing Data: test data untuk automated testing

Rules (WAJIB):
- Seed harus konsisten (sama hasilnya setiap kali dijalankan)
- Seed harus dapat dijalankan ulang (idempotent)
- Seed TIDAK BOLEH menggunakan data sensitif (real user data, real credentials)
- Seed harus memiliki proper ordering (respect foreign key)

Implementasikan:
1. Reference seed:
   - System roles (SUPER_ADMIN, OWNER, ADMINISTRATOR, MANAGER, STAFF)
   - Default permissions (RESOURCE_ACTION format)
   - OTA channels (Booking.com, Agoda, Traveloka, Airbnb, Expedia)
   - Subscription plans (Free, Starter, Professional, Enterprise)
   - Feature flags (default features)

2. Development seed:
   - Sample organization dengan proper settings
   - Sample properties dengan room types dan rooms
   - Sample users dengan proper roles
   - Sample reservations untuk testing

3. Testing seed:
   - Minimal test data untuk automated test
   - Test users untuk authentication test
   - Test data untuk integration test

4. Seed mechanism:
   - Gunakan seed framework TypeORM/Prisma atau custom
   - Pastikan seed idempotent (check if exists before insert)
   - Seed dijalankan setelah migration
   - Environment-specific seed (development vs production)

Pastikan environment dapat dipersiapkan secara cepat dan reproducible.
```

---

---

# Seed Flow

```
Seed Definition
      |
Data Generation
      |
Database Insert
      |
Validation
```

---

# Seed Category

```
Reference Data
Development Data
Testing Data
```

---

# Rule

Seed harus:

- Konsisten.
- Dapat dijalankan ulang.
- Tidak menggunakan data sensitif.

---

# Goal

Environment dapat dipersiapkan secara cepat dan reproducible.
