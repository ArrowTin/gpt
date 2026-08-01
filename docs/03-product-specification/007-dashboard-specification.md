# ChannelHub Dashboard Specification

## Purpose

Mendefinisikan dashboard berdasarkan kebutuhan setiap role pengguna.

---

# 1. Dashboard Principle

Dashboard bersifat dynamic.

Konten berdasarkan:

- Role.
- Permission.
- Feature entitlement.
- Organization setting.

---

# 2. Super Admin Dashboard

Menampilkan:

- Total tenant.
- Active subscription.
- Service health.
- Server resource.
- Error monitoring.
- System activity.

---

# 3. Owner Dashboard

Menampilkan:

- Revenue.
- Reservation.
- Occupancy.
- Property performance.

---

# 4. Manager Dashboard

Menampilkan:

- Operational status.
- Inventory.
- Booking activity.
- Staff activity.

---

# 5. Staff Dashboard

Menampilkan:

- Assigned task.
- Today's reservation.
- Customer activity.

---

# 6. Widget System

Dashboard menggunakan widget configurable.

```
Dashboard
   |
Widget Configuration
   |
Permission Check
   |
Render
```
