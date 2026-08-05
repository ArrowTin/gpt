# ChannelHub Redis Cache Queue Foundation Blueprint

## Purpose

Mendefinisikan penggunaan Redis untuk cache dan asynchronous processing.

---

## AI TRIGGER

### Tujuan Task
Memahami penggunaan Redis untuk cache dan queue dalam backend ChannelHub.

### Konteks yang Perlu Dipahami AI
- Redis Usage: Application → Redis → Cache/Queue/Session
- Cache Responsibility: Data yang sering dibaca, Temporary state, Performance improvement
- Queue Responsibility: Background job, OTA synchronization, Retry mechanism, Event processing
- Rule: Redis bukan sumber data utama, Database tetap source of truth

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/13-backend-foundation/009-backend-project-structure.md (struktur project)

### File/Folder yang Perlu Diperiksa
- docs/13-backend-foundation/008-backend-observability-standard.md (observability)
- docs/17-core-services/008-event-driven-architecture.md (event driven)

### Langkah Implementasi
1. Setup Redis connection dan configuration
2. Implementasikan cache layer untuk data yang sering dibaca
3. Implementasikan queue dengan BullMQ untuk background job
4. Pastikan cache invalidation yang proper

### Kriteria Keberhasilan (Definition of Done)
- Redis terintegrasi untuk cache dan queue
- Cache berfungsi untuk performance improvement
- Queue berfungsi untuk background job
- Cache invalidation berfungsi dengan benar

### Prompt Implementasi
```
Anda akan mengimplementasikan Redis cache dan queue backend ChannelHub.

Baca docs/13-backend-foundation/006-redis-cache-queue-foundation.md untuk memahami penggunaan Redis.

Redis Usage:
Application → Redis → Cache/Queue/Session

Cache Responsibility:
- Data yang sering dibaca (user session, config, reference data)
- Temporary state (OTP, rate limiting counter)
- Performance improvement (reduce database load)

Queue Responsibility (BullMQ):
- Background job (email notification, report generation)
- OTA synchronization (inventory sync, rate sync)
- Retry mechanism (failed external API call)
- Event processing (event-driven architecture)

RULE (WAJIB):
- Redis BUKAN sumber data utama
- Database TETAP menjadi source of truth
- Cache harus memiliki invalidation strategy
- Queue harus memiliki retry mechanism dan error handling

Implementasikan:
1. Redis connection dan configuration
2. Cache service dengan TTL dan invalidation
3. BullMQ setup dengan queue processor
4. Queue untuk OTA synchronization
5. Queue untuk notification
6. Cache untuk session dan frequent data

Pastikan cache dan queue meningkatkan performance dan reliability.
```

---

---

# Redis Usage

```
Application
     |
Redis
     |
Cache / Queue / Session
```

---

# Cache Responsibility

Digunakan untuk:

- Data yang sering dibaca.
- Temporary state.
- Performance improvement.

---

# Queue Responsibility

Digunakan untuk:

- Background job.
- OTA synchronization.
- Retry mechanism.
- Event processing.

---

# Rule

Redis bukan sumber data utama.

Database tetap menjadi source of truth.
