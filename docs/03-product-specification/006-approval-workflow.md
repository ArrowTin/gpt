# ChannelHub Approval Workflow

## Purpose

Mendefinisikan alur persetujuan untuk tindakan yang memiliki risiko bisnis atau operasional.

---

# 1. Approval Principle

Tidak semua aksi membutuhkan approval.

Approval digunakan untuk:

- Perubahan kritikal.
- Dampak finansial.
- Perubahan konfigurasi besar.

---

# 2. Example Flow

```
Staff Request
      |
      v
Manager Review
      |
      v
Approved / Rejected
```

---

# 3. Approval Object

Setiap request memiliki:

- Request ID.
- Requester.
- Action type.
- Data perubahan.
- Approver.
- Status.
- History.

---

# 4. Approval Status

```
Draft
Pending
Approved
Rejected
Cancelled
```

---

# 5. Technical Requirement

Approval engine harus configurable.

Rule dapat berdasarkan:

- Role.
- Amount.
- Property.
- Action type.
