# ChannelHub Property Service Design Blueprint

## Purpose

Mendefinisikan pengelolaan properti sebagai domain utama channel manager.

---

# Responsibility

Property Service menangani:

- Property.
- Room type.
- Inventory.
- Rate configuration.
- Availability.

---

# Domain Flow

```
Property
 |
Room
 |
Inventory
 |
OTA Distribution
```

---

# Core Entity

```
Property
RoomType
RoomInventory
RatePlan
```

---

# Integration

Berhubungan dengan:

- Reservation Service.
- OTA Service.
- Reporting Service.

---

# Completion Criteria

- Property management tersedia.
- Inventory dapat dikelola.
- Data siap sinkronisasi OTA.
