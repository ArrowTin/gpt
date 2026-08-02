# ChannelHub Domain Entity Design

## Purpose

Mendefinisikan **entity, field wajib, invariant, dan aturan bisnis** setiap domain ChannelHub. Dokumen ini menjembatani domain architecture dengan DDL: apa arti setiap data dan aturan apa yang tidak boleh dilanggar aplikasi.

## Scope

Seluruh entity operasional platform. Nama tabel mengikuti [009-canonical-erd.md](./009-canonical-erd.md); tipe kolom mengikuti [010-postgresql-ddl-reference.md](./010-postgresql-ddl-reference.md).

## Context

Domain map: [docs/02-product-architecture/002-domain-architecture.md](../02-product-architecture/002-domain-architecture.md). Role & permission: [docs/03-product-specification/002-role-permission-system.md](../03-product-specification/002-role-permission-system.md). Subscription: [docs/01-business/007-subscription-model.md](../01-business/007-subscription-model.md).

## Rules

- Entity merepresentasikan bisnis, bukan sekadar tabel; aturan bisnis di bawah wajib ditegakkan di domain layer, bukan hanya di database.
- Setiap entity tenant-owned tidak boleh diakses tanpa tenant context (ADR-006).
- Perubahan status wajib menghasilkan event atau audit record.
- Constraint database adalah jaring pengaman terakhir, bukan pengganti validasi domain.

## Technical Details

### Identity

**users** — identitas orang, bukan anggota organisasi.

| Field | Arti |
| --- | --- |
| `email` | unik global, case-insensitive, dipakai sebagai login |
| `password_hash` | Argon2id; algoritma mengikuti [docs/22-security/002-authentication-hardening.md](../22-security/002-authentication-hardening.md) |
| `status` | `PENDING` sebelum verifikasi email, `ACTIVE` setelah verifikasi |
| `is_super_admin` | akses lintas tenant (break-glass), wajib audit setiap penggunaan |
| `failed_login_count`, `locked_until` | proteksi brute force |

Aturan bisnis:

- User dapat menjadi anggota lebih dari satu organisasi; identitas tidak diduplikasi per tenant.
- Login gagal berturut-turut melebihi ambang mengunci akun sampai `locked_until`.
- User `DEACTIVATED` tidak dapat login dan seluruh `sessions` miliknya dicabut.

**roles / permissions / user_roles** — akses final = Role + Permission + Feature Entitlement + Organization Policy.

- `permissions` bersifat global dan berformat `RESOURCE_ACTION` (contoh `PROPERTY_UPDATE`, `RESERVATION_APPROVE`).
- `roles` dengan `organization_id IS NULL` adalah system role (`SUPER_ADMIN`, `OWNER`, `ADMINISTRATOR`, `MANAGER`, `STAFF`) dan tidak boleh diubah tenant.
- `roles.level` mengikuti hierarki pada [docs/03-product-specification/001-user-role-hierarchy.md](../03-product-specification/001-user-role-hierarchy.md); role tidak boleh memberikan permission yang tidak dimiliki pemberi assignment.
- Setiap perubahan `role_permissions` menulis `audit_logs` berisi nilai lama dan baru.

**sessions** — refresh token disimpan sebagai hash, bukan token asli; rotasi mengisi `rotated_from`. Pemakaian ulang refresh token yang sudah dirotasi wajib mencabut seluruh session user tersebut.

### Organization

**organizations** — tenant sekaligus subscriber; `slug` dipakai white-label routing, `branding` menyimpan konfigurasi tampilan.

Aturan bisnis:

- Tepat satu `organization_members.is_owner = true` per organisasi (dijaga partial unique index).
- Organisasi `SUSPENDED` menolak seluruh operasi tulis kecuali pembayaran dan renewal.
- Menghapus organisasi bersifat soft delete; data tenant tidak dihapus fisik sebelum masa retensi terpenuhi.

### Property

**properties → room_types → rooms** adalah hierarki master data properti.

| Entity | Invariant |
| --- | --- |
| `properties` | `code` unik per organisasi; `ACTIVE` hanya jika minimal satu `room_types` aktif |
| `room_types` | `total_rooms` harus sama dengan jumlah `rooms` aktif |
| `rate_plans` | mata uang wajib sama dengan `properties.currency` |
| `rate_calendar` | satu harga per `rate_plan_id` + `stay_date`; `closed = true` menutup penjualan tanggal tersebut |
| `inventory` | `booked_units + blocked_units <= total_units` (anti oversell) |

Aturan bisnis:

- `inventory` disimpan per hari per room type; ketersediaan yang dijual = `total_units - booked_units - blocked_units`.
- Perubahan `inventory` dan `rate_calendar` selalu memicu `sync_jobs` bertipe `INVENTORY` atau `RATE` untuk setiap `channel_connections` aktif.
- Update `inventory` memakai optimistic locking lewat kolom `version`; konflik dijawab `409 CONFLICT`.

### Reservation

**reservations** adalah agregat transaksi; `reservation_rooms` adalah baris detail, `reservation_events` adalah jejak lifecycle.

Lifecycle status:

```text
PENDING → CONFIRMED → CHECKED_IN → CHECKED_OUT
   ↓          ↓
CANCELLED  CANCELLED / NO_SHOW
```

Aturan bisnis:

- `check_out > check_in`; lama menginap maksimum mengikuti kebijakan organisasi.
- Membuat reservasi `CONFIRMED` wajib satu transaksi database yang menaikkan `inventory.booked_units` untuk setiap malam pada rentang menginap; gagal salah satu malam membatalkan seluruh transaksi.
- Pembatalan mengembalikan `booked_units` dan mengisi `cancelled_at`.
- Reservasi asal OTA wajib mengisi `channel_connection_id` + `external_reference`; pasangan ini unik agar webhook ganda tidak membuat duplikat.
- Setiap perubahan status menulis satu baris `reservation_events` dan mempublikasikan event domain (ADR-007).
- `total_amount` harus sama dengan jumlah `reservation_rooms.subtotal`.

### OTA Integration

- `ota_channels` adalah katalog channel level platform; tenant tidak dapat membuat channel baru.
- `channel_connections` menyimpan kredensial terenkripsi per property per channel; satu property hanya punya satu koneksi per channel.
- `channel_mappings` wajib lengkap sebelum koneksi boleh berstatus `CONNECTED`; room type tanpa mapping tidak disinkronkan.
- `sync_jobs` idempoten melalui `idempotency_key`; retry memakai backoff dan berhenti pada `max_attempt`, lalu status `FAILED` dan memicu notifikasi.
- `webhook_events` menyimpan payload mentah; event diproses hanya jika `signature_valid = true` dan `external_event_id` belum pernah diterima.
- OTA tidak pernah menjadi master data properti; konflik data selalu dimenangkan data internal.

### Subscription & Billing

- `subscription_plans` bersifat konfigurasi; kode aplikasi dilarang mengandung percabangan berdasarkan nama plan (ADR-009).
- Satu organisasi hanya boleh memiliki satu subscription berstatus `TRIAL`, `ACTIVE`, atau `EXPIRING` (dijaga partial unique index).
- `feature_entitlements` diturunkan dari `subscription_plans.features` dan `limits` saat subscription dibuat atau diubah; entitlement engine membaca tabel ini, bukan plan.
- Upgrade/downgrade menghitung prorata dan menerbitkan `invoices` baru; perubahan quota berlaku pada periode berjalan.
- `invoices.total_amount = subtotal + tax_amount`; invoice `PAID` tidak dapat diubah, koreksi memakai invoice baru berstatus `VOID` pada invoice lama.
- `payments` idempoten melalui `provider` + `provider_reference`; platform tidak menyimpan data kartu.
- `credit_wallets.balance` tidak boleh negatif; setiap perubahan wajib menulis `credit_transactions` dengan `balance_after` dan `idempotency_key`.

### Notification & Platform Management

- `notifications` selalu punya template atau isi eksplisit; pengiriman dilakukan worker BullMQ (ADR-005).
- `feature_flags` mengontrol rilis bertahap dan terpisah dari `feature_entitlements` (komersial).
- `menus` menentukan navigasi; item ditampilkan hanya jika `required_permission` dimiliki dan `required_feature` aktif.
- `system_configurations` menyimpan konfigurasi platform; perubahan wajib audit.

### Field wajib universal

| Field | Berlaku pada |
| --- | --- |
| `id uuid` | seluruh tabel kecuali tabel relasi murni dan tabel ber-PK natural |
| `organization_id` | seluruh tabel tenant-owned |
| `created_at`, `updated_at` | seluruh tabel operasional |
| `deleted_at` | entity master yang mendukung soft delete |

## Impact

- [docs/15-database-implementation/010-postgresql-ddl-reference.md](./010-postgresql-ddl-reference.md) — constraint mengikuti invariant di sini.
- [docs/17-core-services/](../17-core-services/) — service wajib menegakkan aturan bisnis ini.
- [checklists/code-review.md](../../checklists/code-review.md) — review domain layer memeriksa invariant.

## References

- [009-canonical-erd.md](./009-canonical-erd.md)
- [docs/09-database-api-contract/001-database-domain-model.md](../09-database-api-contract/001-database-domain-model.md)
- [docs/22-security/003-authorization-tenant-isolation.md](../22-security/003-authorization-tenant-isolation.md)
- [adr/ADR-009-configuration-driven-platform.md](../../adr/ADR-009-configuration-driven-platform.md)
