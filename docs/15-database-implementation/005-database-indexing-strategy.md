# ChannelHub Database Indexing Strategy Blueprint

## Purpose

Menetapkan strategi indexing PostgreSQL untuk menjaga performa query.

---

## AI TRIGGER

### Tujuan Task
Memahami strategy indexing untuk performa query database yang optimal.

### Konteks yang Perlu Dipahami AI
- Index Principle: Index digunakan pada kolom pencarian utama, Foreign key, Kolom filtering, Kolom sorting yang sering digunakan
- Rule: Index harus berdasarkan pola query nyata, dievaluasi secara berkala, tidak dibuat berlebihan
- Performance Goal: Database harus mampu menangani pertumbuhan data reservation dan synchronization

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/15-database-implementation/010-postgresql-ddl-reference.md (DDL reference)

### File/Folder yang Perlu Diperiksa
- docs/15-database-implementation/009-canonical-erd.md (canonical ERD)
- docs/15-database-implementation/002-domain-entity-design.md (domain entity)

### Langkah Implementasi
1. Analisis pola query yang sering digunakan
2. Buat index untuk kolom yang sering di-filter atau di-join
3. Buat index untuk foreign key
4. Evaluasi index secara berkala dan hapus yang tidak terpakai

### Kriteria Keberhasilan (Definition of Done)
- Index dibuat berdasarkan pola query nyata
- Foreign key memiliki index
- Query yang sering digunakan memiliki index yang sesuai
- Tidak ada index yang berlebihan

### Prompt Implementasi
```
Anda akan mengoptimalkan performa database ChannelHub dengan indexing.

Baca docs/15-database-implementation/005-database-indexing-strategy.md untuk memahami strategy indexing.

Index Principle (WAJIB):
- Index digunakan pada kolom pencarian utama
- Index digunakan pada foreign key
- Index digunakan pada kolom filtering
- Index digunakan pada kolom sorting yang sering digunakan

Rules:
- Index harus berdasarkan pola query nyata (bukan tebakan)
- Index harus dievaluasi secara berkala
- Index TIDAK BOLEH dibuat berlebihan (setiap index menambah write overhead)

Implementasikan:
1. Foreign key index (WAJIB):
   - Setiap foreign key WAJIB memiliki index
   - Naming: idx_{table}_{fk_column}

2. Query pattern index:
   - organization_id untuk tenant filtering (WAJIB untuk semua tenant-owned)
   - email untuk user lookup
   - status untuk filtering berdasarkan status
   - created_at, updated_at untuk sorting dan range query
   - Composite index untuk query yang sering menggabungkan filter

3. Partial index untuk conditional query:
   - WHERE deleted_at IS NULL untuk active records
   - WHERE status = 'ACTIVE' untuk active status

4. Unique index untuk uniqueness:
   - email uniqueness
   - slug uniqueness
   - Composite uniqueness

5. Index evaluation:
   - Monitor query performance dengan EXPLAIN ANALYZE
   - Hapus index yang tidak terpakai
   - Rebuild index jika fragmentasi tinggi

Performance Goal:
- Database harus mampu menangani pertumbuhan data reservation dan synchronization
- Query p95 < 500ms untuk endpoint utama

Pastikan indexing meningkatkan performa tanpa menambah write overhead yang berlebihan.
```

---

---

# Index Principle

Index digunakan pada:

- Kolom pencarian utama.
- Foreign key.
- Kolom filtering.
- Kolom sorting yang sering digunakan.

---

# Rule

Index harus:

- Berdasarkan pola query nyata.
- Dievaluasi secara berkala.
- Tidak dibuat berlebihan.

---

# Performance Goal

Database harus mampu menangani pertumbuhan data reservation dan synchronization.
