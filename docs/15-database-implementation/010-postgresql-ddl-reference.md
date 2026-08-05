# ChannelHub PostgreSQL DDL Reference

## Purpose

Menyediakan **DDL eksekusi lengkap** untuk seluruh tabel kanonik ChannelHub. Migration pertama aplikasi wajib menghasilkan schema yang setara dengan dokumen ini.

---

## AI TRIGGER

### Tujuan Task
Menyediakan DDL eksekusi yang menjadi CONTRACT ARTIFACT untuk seluruh implementasi database migration.

### Konteks yang Perlu Dipahami AI
- Ini adalah CONTRACT ARTIFACT - DDL eksekusi lengkap yang WAJIB diikuti
- Migration pertama aplikasi wajib menghasilkan schema yang setara dengan dokumen ini
- Daftar tabel dan relasi mengikuti 009-canonical-erd.md
- Definisi field dan aturan bisnis ada di 002-domain-entity-design.md
- DDL adalah kontrak, perubahan kolom wajib melalui migration baru
- PostgreSQL 15+, extension `pgcrypto` untuk `gen_random_uuid()`

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/15-database-implementation/009-canonical-erd.md (canonical ERD)
- docs/15-database-implementation/002-domain-entity-design.md (domain entity design)

### File/Folder yang Perlu Diperiksa
- docs/15-database-implementation/001-postgresql-schema-standard.md (schema standard)
- docs/15-database-implementation/003-database-migration-strategy.md (migration strategy)
- docs/15-database-implementation/005-database-indexing-strategy.md (indexing strategy)

### Langkah Implementasi
1. Gunakan DDL di dokumen ini sebagai reference untuk migration
2. Pastikan migration menghasilkan schema yang setara
3. Ikuti urutan migration yang ditentukan
4. Jangan mengubah migration lama, buat migration baru untuk perubahan

### Kriteria Keberhasilan (Definition of Done)
- Migration menghasilkan schema yang setara dengan DDL
- Tabel, kolom, tipe data, constraint SESUAI dengan DDL
- Index SESUAI dengan yang didefinisikan
- Enum SESUAI dengan yang didefinisikan

### Prompt Implementasi
```
Anda akan membuat atau memodifikasi database migration ChannelHub.

PERINGATAN: Ini adalah CONTRACT ARTIFACT dari docs/15-database-implementation/010-postgresql-ddl-reference.md.

DDL adalah kontrak. Migration pertama aplikasi WAJIB menghasilkan schema yang setara dengan dokumen ini.

Rules (WAJIB diikuti):
- DDL adalah kontrak, JANGAN diubah sembarangan
- Perubahan kolom WAJIB melalui migration baru, JANGAN mengubah migration lama
- Semua tabel tenant-owned memakai organization_id uuid NOT NULL REFERENCES organizations(id)
- Uang disimpan sebagai numeric(14,2) dengan kolom currency char(3)
- Kredit disimpan sebagai bigint (satuan credit, bukan mata uang)
- Timestamp memakai timestamptz
- Tanggal operasional hotel (stay_date, check_in) memakai date
- Kolom konfigurasi dinamis memakai jsonb (configuration driven)
- Kredensial OTA TIDAK disimpan plaintext, gunakan credentials_encrypted bytea

Extension & Enum (WAJIB):
- CREATE EXTENSION IF NOT EXISTS pgcrypto
- Enum: user_status, organization_status, property_status, reservation_status, subscription_status, invoice_status, payment_status, sync_status, connection_status, notification_status

Standar Table (WAJIB diikuti):
- Primary key: uuid PRIMARY KEY DEFAULT gen_random_uuid()
- Timestamp: created_at timestamptz NOT NULL DEFAULT now(), updated_at timestamptz NOT NULL DEFAULT now()
- Soft delete: deleted_at timestamptz (nullable)
- Tenant filtering: organization_id untuk tenant-owned

Urutan Migration (WAJIB diikuti):
1. Extension & enum
2. Identity domain (users, roles, permissions, role_permissions, user_roles, sessions, audit_logs)
3. Organization domain (organizations, organization_settings, organization_members)
4. Property domain (properties, room_types, rooms, rate_plans, rate_calendar, inventory)
5. Reservation domain (guests, booking_sources, reservations, reservation_rooms, reservation_events)
6. OTA Integration domain (ota_channels, channel_connections, channel_mappings, sync_jobs, sync_logs, webhook_events)
7. Subscription domain (subscription_plans, subscriptions, feature_entitlements)
8. Billing domain (invoices, invoice_items, payments, credit_wallets, credit_transactions)
9. Notification domain (notification_templates, notifications)
10. Platform Management domain (feature_flags, menus, system_configurations)

Jika perlu perubahan schema:
1. UPDATE docs/15-database-implementation/009-canonical-erd.md dulu
2. UPDATE docs/15-database-implementation/010-postgresql-ddl-reference.md dulu
3. Buat migration baru (JANGAN ubah migration lama)
4. Pastikan migration menghasilkan schema yang setara dengan DDL

JANGAN menebak DDL. JANGAN membuat migration tanpa reference DDL.
```

---

## Scope

PostgreSQL 15+, satu database logis, extension `pgcrypto` untuk `gen_random_uuid()`. Daftar tabel dan relasi mengikuti [009-canonical-erd.md](./009-canonical-erd.md); definisi field dan aturan bisnis ada di [002-domain-entity-design.md](./002-domain-entity-design.md).

## Context

Standar penamaan: [001-postgresql-schema-standard.md](./001-postgresql-schema-standard.md). Strategi migration: [003-database-migration-strategy.md](./003-database-migration-strategy.md). Index: [005-database-indexing-strategy.md](./005-database-indexing-strategy.md).

## Rules

- DDL adalah kontrak. Perubahan kolom wajib melalui migration baru, bukan mengubah migration lama.
- Semua tabel tenant-owned memakai `organization_id uuid NOT NULL REFERENCES organizations(id)`.
- Uang disimpan sebagai `numeric(14,2)` dengan kolom `currency char(3)`; kredit sebagai `bigint` (satuan credit, bukan mata uang).
- Timestamp memakai `timestamptz`; tanggal operasional hotel (`stay_date`, `check_in`) memakai `date`.
- Kolom konfigurasi dinamis memakai `jsonb` sesuai prinsip configuration driven (ADR-009).
- Kredensial OTA tidak disimpan plaintext; kolom `credentials_encrypted bytea` diisi ciphertext (lihat [docs/22-security/004-secrets-and-credential-management.md](../22-security/004-secrets-and-credential-management.md)).

## Technical Details

### Extension & enum

```sql
CREATE EXTENSION IF NOT EXISTS pgcrypto;

CREATE TYPE user_status AS ENUM ('PENDING','ACTIVE','SUSPENDED','DEACTIVATED');
CREATE TYPE organization_status AS ENUM ('TRIAL','ACTIVE','SUSPENDED','CLOSED');
CREATE TYPE property_status AS ENUM ('DRAFT','ACTIVE','INACTIVE','ARCHIVED');
CREATE TYPE reservation_status AS ENUM ('PENDING','CONFIRMED','CHECKED_IN','CHECKED_OUT','CANCELLED','NO_SHOW');
CREATE TYPE subscription_status AS ENUM ('TRIAL','ACTIVE','EXPIRING','EXPIRED','SUSPENDED','CANCELLED');
CREATE TYPE invoice_status AS ENUM ('DRAFT','ISSUED','PAID','OVERDUE','VOID');
CREATE TYPE payment_status AS ENUM ('PENDING','SUCCEEDED','FAILED','REFUNDED');
CREATE TYPE sync_status AS ENUM ('QUEUED','RUNNING','SUCCEEDED','FAILED','RETRYING');
CREATE TYPE connection_status AS ENUM ('DISCONNECTED','CONNECTED','ERROR','REVOKED');
CREATE TYPE notification_status AS ENUM ('QUEUED','SENT','FAILED','READ');
```

### Organization domain

```sql
CREATE TABLE organizations (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    name            varchar(160) NOT NULL,
    slug            varchar(80) NOT NULL,
    status          organization_status NOT NULL DEFAULT 'TRIAL',
    country_code    char(2),
    timezone        varchar(64) NOT NULL DEFAULT 'Asia/Jakarta',
    default_currency char(3) NOT NULL DEFAULT 'IDR',
    branding        jsonb NOT NULL DEFAULT '{}'::jsonb,
    created_at      timestamptz NOT NULL DEFAULT now(),
    updated_at      timestamptz NOT NULL DEFAULT now(),
    deleted_at      timestamptz,
    CONSTRAINT organizations_slug_unique UNIQUE (slug)
);

CREATE TABLE organization_settings (
    organization_id uuid PRIMARY KEY REFERENCES organizations(id) ON DELETE CASCADE,
    locale          varchar(10) NOT NULL DEFAULT 'id-ID',
    approval_policy jsonb NOT NULL DEFAULT '{}'::jsonb,
    notification_policy jsonb NOT NULL DEFAULT '{}'::jsonb,
    updated_at      timestamptz NOT NULL DEFAULT now()
);

CREATE TABLE organization_members (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    user_id         uuid NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    is_owner        boolean NOT NULL DEFAULT false,
    invited_at      timestamptz,
    joined_at       timestamptz,
    created_at      timestamptz NOT NULL DEFAULT now(),
    updated_at      timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT organization_members_unique UNIQUE (organization_id, user_id)
);

CREATE UNIQUE INDEX organization_members_single_owner
    ON organization_members (organization_id) WHERE is_owner;
```

> `organization_members` dibuat setelah `users` pada urutan migration; blok Identity di bawah dieksekusi lebih dulu.

### Identity domain

```sql
CREATE TABLE users (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    email           citext NOT NULL,
    password_hash   text NOT NULL,
    full_name       varchar(160) NOT NULL,
    phone           varchar(32),
    status          user_status NOT NULL DEFAULT 'PENDING',
    is_super_admin  boolean NOT NULL DEFAULT false,
    mfa_secret_encrypted bytea,
    last_login_at   timestamptz,
    failed_login_count smallint NOT NULL DEFAULT 0,
    locked_until    timestamptz,
    created_at      timestamptz NOT NULL DEFAULT now(),
    updated_at      timestamptz NOT NULL DEFAULT now(),
    deleted_at      timestamptz,
    CONSTRAINT users_email_unique UNIQUE (email)
);

CREATE TABLE permissions (
    id          uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    code        varchar(80) NOT NULL,
    resource    varchar(40) NOT NULL,
    action      varchar(40) NOT NULL,
    description text,
    created_at  timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT permissions_code_unique UNIQUE (code)
);

CREATE TABLE roles (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id uuid REFERENCES organizations(id) ON DELETE CASCADE,
    code            varchar(60) NOT NULL,
    name            varchar(120) NOT NULL,
    level           smallint NOT NULL,
    is_system       boolean NOT NULL DEFAULT false,
    created_at      timestamptz NOT NULL DEFAULT now(),
    updated_at      timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT roles_scope_code_unique UNIQUE (organization_id, code),
    CONSTRAINT roles_system_is_global CHECK (NOT is_system OR organization_id IS NULL)
);

CREATE TABLE role_permissions (
    role_id         uuid NOT NULL REFERENCES roles(id) ON DELETE CASCADE,
    permission_id   uuid NOT NULL REFERENCES permissions(id) ON DELETE CASCADE,
    created_at      timestamptz NOT NULL DEFAULT now(),
    PRIMARY KEY (role_id, permission_id)
);

CREATE TABLE user_roles (
    user_id         uuid NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    role_id         uuid NOT NULL REFERENCES roles(id) ON DELETE CASCADE,
    organization_id uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    assigned_by     uuid REFERENCES users(id),
    created_at      timestamptz NOT NULL DEFAULT now(),
    PRIMARY KEY (user_id, role_id, organization_id)
);

CREATE TABLE sessions (
    id                  uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id             uuid NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    organization_id     uuid REFERENCES organizations(id) ON DELETE CASCADE,
    refresh_token_hash  text NOT NULL,
    user_agent          text,
    ip_address          inet,
    expires_at          timestamptz NOT NULL,
    revoked_at          timestamptz,
    rotated_from        uuid REFERENCES sessions(id),
    created_at          timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT sessions_refresh_hash_unique UNIQUE (refresh_token_hash)
);

CREATE TABLE audit_logs (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id uuid REFERENCES organizations(id) ON DELETE SET NULL,
    actor_user_id   uuid REFERENCES users(id) ON DELETE SET NULL,
    action          varchar(80) NOT NULL,
    resource_type   varchar(60) NOT NULL,
    resource_id     uuid,
    previous_value  jsonb,
    new_value       jsonb,
    correlation_id  uuid,
    ip_address      inet,
    created_at      timestamptz NOT NULL DEFAULT now()
);
```

`citext` memerlukan `CREATE EXTENSION IF NOT EXISTS citext;` pada migration pertama.

### Property domain

```sql
CREATE TABLE properties (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    code            varchar(40) NOT NULL,
    name            varchar(160) NOT NULL,
    status          property_status NOT NULL DEFAULT 'DRAFT',
    address_line    text,
    city            varchar(100),
    country_code    char(2),
    timezone        varchar(64) NOT NULL DEFAULT 'Asia/Jakarta',
    currency        char(3) NOT NULL DEFAULT 'IDR',
    check_in_time   time NOT NULL DEFAULT '14:00',
    check_out_time  time NOT NULL DEFAULT '12:00',
    facilities      jsonb NOT NULL DEFAULT '[]'::jsonb,
    created_at      timestamptz NOT NULL DEFAULT now(),
    updated_at      timestamptz NOT NULL DEFAULT now(),
    deleted_at      timestamptz,
    CONSTRAINT properties_code_unique UNIQUE (organization_id, code)
);

CREATE TABLE room_types (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    property_id     uuid NOT NULL REFERENCES properties(id) ON DELETE CASCADE,
    code            varchar(40) NOT NULL,
    name            varchar(120) NOT NULL,
    max_occupancy   smallint NOT NULL CHECK (max_occupancy > 0),
    base_rate       numeric(14,2) NOT NULL CHECK (base_rate >= 0),
    currency        char(3) NOT NULL DEFAULT 'IDR',
    total_rooms     integer NOT NULL DEFAULT 0 CHECK (total_rooms >= 0),
    created_at      timestamptz NOT NULL DEFAULT now(),
    updated_at      timestamptz NOT NULL DEFAULT now(),
    deleted_at      timestamptz,
    CONSTRAINT room_types_code_unique UNIQUE (property_id, code)
);

CREATE TABLE rooms (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    room_type_id    uuid NOT NULL REFERENCES room_types(id) ON DELETE CASCADE,
    room_number     varchar(20) NOT NULL,
    floor           varchar(10),
    is_active       boolean NOT NULL DEFAULT true,
    created_at      timestamptz NOT NULL DEFAULT now(),
    updated_at      timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT rooms_number_unique UNIQUE (room_type_id, room_number)
);

CREATE TABLE rate_plans (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    room_type_id    uuid NOT NULL REFERENCES room_types(id) ON DELETE CASCADE,
    code            varchar(40) NOT NULL,
    name            varchar(120) NOT NULL,
    currency        char(3) NOT NULL DEFAULT 'IDR',
    cancellation_policy jsonb NOT NULL DEFAULT '{}'::jsonb,
    min_stay        smallint NOT NULL DEFAULT 1 CHECK (min_stay >= 1),
    is_active       boolean NOT NULL DEFAULT true,
    created_at      timestamptz NOT NULL DEFAULT now(),
    updated_at      timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT rate_plans_code_unique UNIQUE (room_type_id, code)
);

CREATE TABLE rate_calendar (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    rate_plan_id    uuid NOT NULL REFERENCES rate_plans(id) ON DELETE CASCADE,
    stay_date       date NOT NULL,
    price           numeric(14,2) NOT NULL CHECK (price >= 0),
    closed          boolean NOT NULL DEFAULT false,
    updated_at      timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT rate_calendar_unique UNIQUE (rate_plan_id, stay_date)
);

CREATE TABLE inventory (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    room_type_id    uuid NOT NULL REFERENCES room_types(id) ON DELETE CASCADE,
    stay_date       date NOT NULL,
    total_units     integer NOT NULL CHECK (total_units >= 0),
    booked_units    integer NOT NULL DEFAULT 0 CHECK (booked_units >= 0),
    blocked_units   integer NOT NULL DEFAULT 0 CHECK (blocked_units >= 0),
    version         integer NOT NULL DEFAULT 0,
    updated_at      timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT inventory_unique UNIQUE (room_type_id, stay_date),
    CONSTRAINT inventory_not_oversold CHECK (booked_units + blocked_units <= total_units)
);
```

`inventory.version` dipakai untuk optimistic locking pada proses booking dan sinkronisasi OTA ([007-transaction-consistency-pattern.md](./007-transaction-consistency-pattern.md)).

### Reservation domain

```sql
CREATE TABLE guests (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    full_name       varchar(160) NOT NULL,
    email           citext,
    phone           varchar(32),
    country_code    char(2),
    document_ref    varchar(80),
    created_at      timestamptz NOT NULL DEFAULT now(),
    updated_at      timestamptz NOT NULL DEFAULT now(),
    deleted_at      timestamptz
);

CREATE TABLE booking_sources (
    id          uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    code        varchar(40) NOT NULL,
    name        varchar(120) NOT NULL,
    kind        varchar(20) NOT NULL CHECK (kind IN ('DIRECT','OTA','AGENT','WALK_IN')),
    created_at  timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT booking_sources_code_unique UNIQUE (code)
);

CREATE TABLE reservations (
    id                  uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id     uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    property_id         uuid NOT NULL REFERENCES properties(id) ON DELETE RESTRICT,
    guest_id            uuid NOT NULL REFERENCES guests(id) ON DELETE RESTRICT,
    booking_source_id   uuid REFERENCES booking_sources(id),
    channel_connection_id uuid REFERENCES channel_connections(id) ON DELETE SET NULL,
    reservation_code    varchar(30) NOT NULL,
    external_reference  varchar(80),
    status              reservation_status NOT NULL DEFAULT 'PENDING',
    check_in            date NOT NULL,
    check_out           date NOT NULL,
    adults              smallint NOT NULL DEFAULT 1 CHECK (adults >= 1),
    children            smallint NOT NULL DEFAULT 0 CHECK (children >= 0),
    total_amount        numeric(14,2) NOT NULL DEFAULT 0 CHECK (total_amount >= 0),
    currency            char(3) NOT NULL DEFAULT 'IDR',
    notes               text,
    created_at          timestamptz NOT NULL DEFAULT now(),
    updated_at          timestamptz NOT NULL DEFAULT now(),
    cancelled_at        timestamptz,
    CONSTRAINT reservations_code_unique UNIQUE (organization_id, reservation_code),
    CONSTRAINT reservations_external_unique UNIQUE (channel_connection_id, external_reference),
    CONSTRAINT reservations_date_range CHECK (check_out > check_in)
);

CREATE TABLE reservation_rooms (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    reservation_id  uuid NOT NULL REFERENCES reservations(id) ON DELETE CASCADE,
    room_type_id    uuid NOT NULL REFERENCES room_types(id) ON DELETE RESTRICT,
    rate_plan_id    uuid REFERENCES rate_plans(id) ON DELETE SET NULL,
    room_id         uuid REFERENCES rooms(id) ON DELETE SET NULL,
    quantity        smallint NOT NULL DEFAULT 1 CHECK (quantity >= 1),
    nightly_rate    numeric(14,2) NOT NULL CHECK (nightly_rate >= 0),
    subtotal        numeric(14,2) NOT NULL CHECK (subtotal >= 0),
    created_at      timestamptz NOT NULL DEFAULT now()
);

CREATE TABLE reservation_events (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    reservation_id  uuid NOT NULL REFERENCES reservations(id) ON DELETE CASCADE,
    event_type      varchar(60) NOT NULL,
    previous_status reservation_status,
    new_status      reservation_status,
    actor_user_id   uuid REFERENCES users(id) ON DELETE SET NULL,
    payload         jsonb NOT NULL DEFAULT '{}'::jsonb,
    correlation_id  uuid,
    created_at      timestamptz NOT NULL DEFAULT now()
);
```

`event_type` memakai nama event kanonik pada [docs/09-database-api-contract/006-event-contract-design.md](../09-database-api-contract/006-event-contract-design.md): `ReservationCreated`, `ReservationUpdated`, `ReservationCancelled`.

### OTA integration domain

```sql
CREATE TABLE ota_channels (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    code            varchar(40) NOT NULL,
    name            varchar(120) NOT NULL,
    capabilities    jsonb NOT NULL DEFAULT '{}'::jsonb,
    is_active       boolean NOT NULL DEFAULT true,
    created_at      timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT ota_channels_code_unique UNIQUE (code)
);

CREATE TABLE channel_connections (
    id                  uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id     uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    property_id         uuid NOT NULL REFERENCES properties(id) ON DELETE CASCADE,
    ota_channel_id      uuid NOT NULL REFERENCES ota_channels(id) ON DELETE RESTRICT,
    status              connection_status NOT NULL DEFAULT 'DISCONNECTED',
    credentials_encrypted bytea,
    settings            jsonb NOT NULL DEFAULT '{}'::jsonb,
    webhook_secret_hash text,
    last_sync_at        timestamptz,
    last_error          text,
    created_at          timestamptz NOT NULL DEFAULT now(),
    updated_at          timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT channel_connections_unique UNIQUE (property_id, ota_channel_id)
);

CREATE TABLE channel_mappings (
    id                      uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id         uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    channel_connection_id   uuid NOT NULL REFERENCES channel_connections(id) ON DELETE CASCADE,
    room_type_id            uuid NOT NULL REFERENCES room_types(id) ON DELETE CASCADE,
    rate_plan_id            uuid REFERENCES rate_plans(id) ON DELETE SET NULL,
    external_room_id        varchar(80) NOT NULL,
    external_rate_plan_id   varchar(80),
    created_at              timestamptz NOT NULL DEFAULT now(),
    updated_at              timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT channel_mappings_unique UNIQUE (channel_connection_id, external_room_id, external_rate_plan_id)
);

CREATE TABLE sync_jobs (
    id                      uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id         uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    channel_connection_id   uuid NOT NULL REFERENCES channel_connections(id) ON DELETE CASCADE,
    job_type                varchar(40) NOT NULL CHECK (job_type IN ('INVENTORY','RATE','RESERVATION','FULL')),
    status                  sync_status NOT NULL DEFAULT 'QUEUED',
    idempotency_key         varchar(120) NOT NULL,
    attempt                 smallint NOT NULL DEFAULT 0,
    max_attempt             smallint NOT NULL DEFAULT 5,
    scheduled_at            timestamptz NOT NULL DEFAULT now(),
    started_at              timestamptz,
    finished_at             timestamptz,
    error_code              varchar(60),
    created_at              timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT sync_jobs_idempotency_unique UNIQUE (channel_connection_id, idempotency_key)
);

CREATE TABLE sync_logs (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    sync_job_id     uuid NOT NULL REFERENCES sync_jobs(id) ON DELETE CASCADE,
    level           varchar(10) NOT NULL CHECK (level IN ('INFO','WARN','ERROR')),
    message         text NOT NULL,
    context         jsonb NOT NULL DEFAULT '{}'::jsonb,
    created_at      timestamptz NOT NULL DEFAULT now()
);

CREATE TABLE webhook_events (
    id                      uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id         uuid REFERENCES organizations(id) ON DELETE CASCADE,
    channel_connection_id   uuid REFERENCES channel_connections(id) ON DELETE CASCADE,
    external_event_id       varchar(120) NOT NULL,
    event_type              varchar(60) NOT NULL,
    signature_valid         boolean NOT NULL DEFAULT false,
    payload                 jsonb NOT NULL,
    processed_at            timestamptz,
    error_code              varchar(60),
    received_at             timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT webhook_events_unique UNIQUE (channel_connection_id, external_event_id)
);
```

`sync_jobs.idempotency_key` dan `webhook_events.external_event_id` menegakkan idempotency yang diwajibkan [docs/16-api-contract/003-webhook-architecture.md](../16-api-contract/003-webhook-architecture.md).

### Subscription & billing domain

```sql
CREATE TABLE subscription_plans (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    code            varchar(40) NOT NULL,
    name            varchar(120) NOT NULL,
    price           numeric(14,2) NOT NULL CHECK (price >= 0),
    currency        char(3) NOT NULL DEFAULT 'IDR',
    billing_cycle   varchar(20) NOT NULL CHECK (billing_cycle IN ('MONTHLY','YEARLY','CUSTOM')),
    features        jsonb NOT NULL DEFAULT '[]'::jsonb,
    limits          jsonb NOT NULL DEFAULT '{}'::jsonb,
    is_active       boolean NOT NULL DEFAULT true,
    created_at      timestamptz NOT NULL DEFAULT now(),
    updated_at      timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT subscription_plans_code_unique UNIQUE (code)
);

CREATE TABLE subscriptions (
    id                      uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id         uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    subscription_plan_id    uuid NOT NULL REFERENCES subscription_plans(id) ON DELETE RESTRICT,
    status                  subscription_status NOT NULL DEFAULT 'TRIAL',
    trial_ends_at           timestamptz,
    current_period_start    timestamptz NOT NULL DEFAULT now(),
    current_period_end      timestamptz NOT NULL,
    cancel_at_period_end    boolean NOT NULL DEFAULT false,
    created_at              timestamptz NOT NULL DEFAULT now(),
    updated_at              timestamptz NOT NULL DEFAULT now()
);

CREATE UNIQUE INDEX subscriptions_one_active_per_org
    ON subscriptions (organization_id)
    WHERE status IN ('TRIAL','ACTIVE','EXPIRING');

CREATE TABLE feature_entitlements (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    subscription_id uuid NOT NULL REFERENCES subscriptions(id) ON DELETE CASCADE,
    feature_code    varchar(60) NOT NULL,
    is_enabled      boolean NOT NULL DEFAULT true,
    quota_limit     integer,
    quota_used      integer NOT NULL DEFAULT 0 CHECK (quota_used >= 0),
    period_start    date,
    period_end      date,
    updated_at      timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT feature_entitlements_unique UNIQUE (subscription_id, feature_code)
);

CREATE TABLE invoices (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    subscription_id uuid REFERENCES subscriptions(id) ON DELETE SET NULL,
    invoice_number  varchar(40) NOT NULL,
    status          invoice_status NOT NULL DEFAULT 'DRAFT',
    subtotal        numeric(14,2) NOT NULL DEFAULT 0 CHECK (subtotal >= 0),
    tax_amount      numeric(14,2) NOT NULL DEFAULT 0 CHECK (tax_amount >= 0),
    total_amount    numeric(14,2) NOT NULL DEFAULT 0 CHECK (total_amount >= 0),
    currency        char(3) NOT NULL DEFAULT 'IDR',
    issued_at       timestamptz,
    due_at          timestamptz,
    paid_at         timestamptz,
    created_at      timestamptz NOT NULL DEFAULT now(),
    updated_at      timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT invoices_number_unique UNIQUE (invoice_number)
);

CREATE TABLE invoice_items (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    invoice_id      uuid NOT NULL REFERENCES invoices(id) ON DELETE CASCADE,
    description     varchar(200) NOT NULL,
    quantity        numeric(12,2) NOT NULL DEFAULT 1 CHECK (quantity > 0),
    unit_price      numeric(14,2) NOT NULL CHECK (unit_price >= 0),
    amount          numeric(14,2) NOT NULL CHECK (amount >= 0),
    metadata        jsonb NOT NULL DEFAULT '{}'::jsonb
);

CREATE TABLE payments (
    id                  uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id     uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    invoice_id          uuid REFERENCES invoices(id) ON DELETE SET NULL,
    provider            varchar(40) NOT NULL,
    provider_reference  varchar(120) NOT NULL,
    status              payment_status NOT NULL DEFAULT 'PENDING',
    amount              numeric(14,2) NOT NULL CHECK (amount >= 0),
    currency            char(3) NOT NULL DEFAULT 'IDR',
    paid_at             timestamptz,
    failure_reason      text,
    created_at          timestamptz NOT NULL DEFAULT now(),
    updated_at          timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT payments_provider_reference_unique UNIQUE (provider, provider_reference)
);

CREATE TABLE credit_wallets (
    organization_id uuid PRIMARY KEY REFERENCES organizations(id) ON DELETE CASCADE,
    balance         bigint NOT NULL DEFAULT 0 CHECK (balance >= 0),
    low_threshold   bigint NOT NULL DEFAULT 0,
    version         integer NOT NULL DEFAULT 0,
    updated_at      timestamptz NOT NULL DEFAULT now()
);

CREATE TABLE credit_transactions (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id uuid NOT NULL REFERENCES credit_wallets(organization_id) ON DELETE CASCADE,
    delta           bigint NOT NULL,
    reason          varchar(60) NOT NULL,
    reference_type  varchar(60),
    reference_id    uuid,
    balance_after   bigint NOT NULL CHECK (balance_after >= 0),
    idempotency_key varchar(120) NOT NULL,
    created_at      timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT credit_transactions_idempotency_unique UNIQUE (organization_id, idempotency_key)
);
```

`credit_wallets` tidak menyimpan uang; satuannya credit layanan sesuai [docs/01-business/007-subscription-model.md](../01-business/007-subscription-model.md).

### Notification & platform domain

```sql
CREATE TABLE notification_templates (
    id          uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    code        varchar(60) NOT NULL,
    channel     varchar(20) NOT NULL CHECK (channel IN ('EMAIL','PUSH','IN_APP','WEBHOOK')),
    subject     varchar(200),
    body        text NOT NULL,
    locale      varchar(10) NOT NULL DEFAULT 'id-ID',
    created_at  timestamptz NOT NULL DEFAULT now(),
    updated_at  timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT notification_templates_unique UNIQUE (code, locale)
);

CREATE TABLE notifications (
    id                      uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id         uuid NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    recipient_user_id       uuid REFERENCES users(id) ON DELETE CASCADE,
    notification_template_id uuid REFERENCES notification_templates(id) ON DELETE SET NULL,
    channel                 varchar(20) NOT NULL CHECK (channel IN ('EMAIL','PUSH','IN_APP','WEBHOOK')),
    status                  notification_status NOT NULL DEFAULT 'QUEUED',
    title                   varchar(200) NOT NULL,
    body                    text NOT NULL,
    payload                 jsonb NOT NULL DEFAULT '{}'::jsonb,
    sent_at                 timestamptz,
    read_at                 timestamptz,
    created_at              timestamptz NOT NULL DEFAULT now()
);

CREATE TABLE feature_flags (
    id              uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    code            varchar(60) NOT NULL,
    description     text,
    default_enabled boolean NOT NULL DEFAULT false,
    rollout         jsonb NOT NULL DEFAULT '{}'::jsonb,
    updated_at      timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT feature_flags_code_unique UNIQUE (code)
);

CREATE TABLE menus (
    id                  uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    parent_id           uuid REFERENCES menus(id) ON DELETE CASCADE,
    code                varchar(60) NOT NULL,
    label               varchar(120) NOT NULL,
    path                varchar(200),
    icon                varchar(60),
    sort_order          smallint NOT NULL DEFAULT 0,
    required_permission varchar(80) REFERENCES permissions(code),
    required_feature    varchar(60),
    is_active           boolean NOT NULL DEFAULT true,
    CONSTRAINT menus_code_unique UNIQUE (code)
);

CREATE TABLE system_configurations (
    key         varchar(120) PRIMARY KEY,
    value       jsonb NOT NULL,
    description text,
    updated_by  uuid REFERENCES users(id) ON DELETE SET NULL,
    updated_at  timestamptz NOT NULL DEFAULT now()
);
```

`menus` menyediakan dynamic menu yang diwajibkan [docs/03-product-specification/002-role-permission-system.md](../03-product-specification/002-role-permission-system.md); UI tidak boleh hardcode menu.

### Index wajib

```sql
CREATE INDEX idx_users_status ON users (status) WHERE deleted_at IS NULL;
CREATE INDEX idx_sessions_user_active ON sessions (user_id) WHERE revoked_at IS NULL;
CREATE INDEX idx_audit_logs_org_created ON audit_logs (organization_id, created_at DESC);
CREATE INDEX idx_properties_org_status ON properties (organization_id, status) WHERE deleted_at IS NULL;
CREATE INDEX idx_inventory_lookup ON inventory (organization_id, room_type_id, stay_date);
CREATE INDEX idx_rate_calendar_lookup ON rate_calendar (organization_id, rate_plan_id, stay_date);
CREATE INDEX idx_reservations_org_status ON reservations (organization_id, status);
CREATE INDEX idx_reservations_stay_window ON reservations (property_id, check_in, check_out);
CREATE INDEX idx_reservation_events_reservation ON reservation_events (reservation_id, created_at DESC);
CREATE INDEX idx_sync_jobs_pending ON sync_jobs (status, scheduled_at) WHERE status IN ('QUEUED','RETRYING');
CREATE INDEX idx_webhook_events_unprocessed ON webhook_events (channel_connection_id) WHERE processed_at IS NULL;
CREATE INDEX idx_invoices_org_status ON invoices (organization_id, status);
CREATE INDEX idx_notifications_recipient ON notifications (recipient_user_id, status, created_at DESC);
```

### Urutan migration

```text
001_extensions_and_enums
002_identity_users_permissions_roles
003_organizations_and_membership
004_identity_sessions_audit
005_property_domain
006_reservation_domain
007_ota_integration_domain
008_subscription_billing_domain
009_notification_platform_domain
010_indexes
011_seed_reference_data
```

`organization_members`, `roles.organization_id`, dan `user_roles` dibuat pada migration 003 setelah `organizations` ada. Seed data (permission catalog, system role, `booking_sources`, `ota_channels`, `subscription_plans`) mengikuti [004-data-seed-strategy.md](./004-data-seed-strategy.md).

## Impact

- [docs/15-database-implementation/003-database-migration-strategy.md](./003-database-migration-strategy.md) — urutan migration mengikuti dokumen ini.
- [docs/16-api-contract/009-api-endpoint-specification.md](../16-api-contract/009-api-endpoint-specification.md) — DTO memetakan kolom di sini.
- [checklists/code-review.md](../../checklists/code-review.md) — review schema membandingkan migration dengan DDL ini.

## References

- [009-canonical-erd.md](./009-canonical-erd.md)
- [002-domain-entity-design.md](./002-domain-entity-design.md)
- [005-database-indexing-strategy.md](./005-database-indexing-strategy.md)
- [adr/ADR-006-multi-tenant-isolation.md](../../adr/ADR-006-multi-tenant-isolation.md)
