# ChannelHub Relationship Mapping Standard Blueprint

## Purpose

Menetapkan standar hubungan antar entity database.

---

## AI TRIGGER

### Tujuan Task
Memahami standar relationship mapping untuk konsistensi hubungan antar entity.

### Konteks yang Perlu Dipahami AI
- Relationship Type: One To One, One To Many, Many To Many
- Mapping Principle: Relationship harus merepresentasikan domain bisnis, memiliki foreign key jelas, memiliki aturan lifecycle
- Example Domain: Organization → Property → Room → Reservation
- Goal: Data relationship tetap konsisten dan mudah dikembangkan

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/15-database-implementation/009-canonical-erd.md (canonical ERD)

### File/Folder yang Perlu Diperiksa
- docs/15-database-implementation/002-domain-entity-design.md (domain entity)
- docs/15-database-implementation/010-postgresql-ddl-reference.md (DDL reference)

### Langkah Implementasi
1. Pahami relationship type yang diperlukan
2. Implementasikan foreign key dengan proper constraint
3. Tentukan aturan lifecycle (CASCADE, RESTRICT, SET NULL)
4. Pastikan relationship merepresentasikan domain bisnis

### Kriteria Keberhasilan (Definition of Done)
- Relationship type sesuai dengan domain bisnis
- Foreign key memiliki proper constraint dan cascade rule
- Relationship mapping konsisten dengan ERD
- Aturan lifecycle diterapkan dengan benar

### Prompt Implementasi
```
Anda akan mengimplementasikan relationship mapping database ChannelHub.

Baca docs/15-database-implementation/006-relationship-mapping-standard.md untuk memahami standar relationship.

Relationship Type:
- One To One: 1:1 relationship
- One To Many: 1:N relationship (paling umum)
- Many To Many: N:N relationship (melalui junction table)

Mapping Principle (WAJIB):
- Relationship harus merepresentasikan domain bisnis
- Relationship harus memiliki foreign key jelas
- Relationship harus memiliki aturan lifecycle (CASCADE, RESTRICT, SET NULL)

Example Domain:
Organization → Property → Room → Reservation

Implementasikan:
1. One To Many (paling umum):
   - organization_id di properties (FK ke organizations)
   - property_id di rooms (FK ke properties)
   - user_id di organization_members (FK ke users)
   - ON DELETE CASCADE untuk child yang tergantung
   - ON DELETE RESTRICT untuk reference yang penting

2. Many To Many (melalui junction table):
   - user_roles (junction antara users dan roles)
   - role_permissions (junction antara roles dan permissions)
   - Composite unique constraint pada junction table

3. Aturan Lifecycle:
   - CASCADE: hapus child ketika parent dihapus (organization_members, organization_settings)
   - RESTRICT: cegah penghapusan jika ada reference (reservations ke properties)
   - SET NULL: set null ketika parent dihapus (jarang digunakan)
   - NO ACTION: default PostgreSQL (similar RESTRICT)

4. Cross-domain relationship:
   - Hanya FK yang didefinisikan di canonical ERD yang boleh lintas domain
   - Lihat docs/15-database-implementation/009-canonical-erd.md bagian Cross-Domain Relationship

Pastikan data relationship tetap konsisten dan mudah dikembangkan.
```

---

---

# Relationship Type

```
One To One
One To Many
Many To Many
```

---

# Mapping Principle

Relationship harus:

- Merepresentasikan domain bisnis.
- Memiliki foreign key jelas.
- Memiliki aturan lifecycle.

---

# Example Domain

```
Organization
      |
   Property
      |
     Room
      |
 Reservation
```

---

# Goal

Data relationship tetap konsisten dan mudah dikembangkan.
