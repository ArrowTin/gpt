# ChannelHub Relationship Mapping Standard Blueprint

## Purpose

Menetapkan standar hubungan antar entity database.

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
