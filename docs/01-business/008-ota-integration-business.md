# ChannelHub OTA Integration Business Model

## Purpose

Dokumen ini menjelaskan posisi OTA dalam ekosistem ChannelHub, bagaimana subscriber menggunakan koneksi OTA, dan bagaimana ChannelHub mengelola integrasi multi channel.

---

# 1. Positioning ChannelHub terhadap OTA

ChannelHub bukan menggantikan seluruh OTA eksternal.

ChannelHub berperan sebagai:

```
Property Management Layer
          +
OTA Distribution Layer
          +
Synchronization Engine
```

OTA tetap menjadi marketplace/channel distribusi.

Contoh OTA:

- Booking.com.
- Agoda.
- Traveloka.
- Airbnb.
- Expedia.
- Tiket.com.

---

# 2. ChannelHub OTA Channel

ChannelHub memiliki modul OTA Channel sebagai fitur bawaan.

Modul ini bertugas:

- Menyediakan connector OTA.
- Mengelola autentikasi koneksi.
- Mapping data property.
- Sinkronisasi inventory.
- Sinkronisasi availability.
- Sinkronisasi price.
- Sinkronisasi reservation.

---

# 3. Subscriber Experience

Subscriber tidak perlu memahami detail API OTA.

Flow:

```
Subscriber
    |
    v
Create Property
    |
    v
Create Room
    |
    v
Select OTA Channel
    |
    v
Connect Account
    |
    v
Synchronization
```

Subscriber cukup mengetahui:

- Properti.
- Kamar.
- Jumlah inventory.
- Harga.
- Channel yang digunakan.

---

# 4. OTA Connection Flow

Ketika subscriber memilih OTA:

```
Subscriber
      |
      v
ChannelHub OTA Service
      |
      v
OTA Connector
      |
      v
External OTA API
```

Sistem melakukan:

- Credential validation.
- Connection test.
- Property mapping.
- Room mapping.
- Initial synchronization.

---

# 5. Data Ownership

Master data berada di ChannelHub.

Contoh:

```
ChannelHub

Property
Room
Inventory
Price Rule

        |
        v

OTA Channels
```

ChannelHub menjadi sumber koordinasi data.

---

# 6. Synchronization Event

Perubahan dapat berasal dari:

## Dari ChannelHub

Contoh:

- Update harga.
- Update availability.
- Tambah kamar.

Flow:

```
ChannelHub Event
        |
        v
Queue
        |
        v
OTA Connector
        |
        v
OTA API
```

---

## Dari OTA

Contoh:

- Reservation baru.
- Cancellation.
- Modification.

Flow:

```
OTA Webhook
        |
        v
ChannelHub Event Processor
        |
        v
Update Internal Data
```

---

# 7. OTA Connector Architecture

Connector harus modular.

Contoh:

```
ota-service

connectors
 |
 +-- booking-com
 |
 +-- agoda
 |
 +-- traveloka
 |
 +-- airbnb
 |
 +-- expedia
```

Setiap connector memiliki:

- Authentication handler.
- API client.
- Data mapper.
- Error handler.
- Retry mechanism.

---

# 8. Error Handling

Integrasi OTA harus memiliki:

- Retry.
- Queue.
- Dead letter queue.
- Error code.
- Synchronization log.

Contoh:

```
OTA_SYNC_001
Authentication Failed

OTA_SYNC_002
Room Mapping Failed
```

---

# 9. Technical Principle

OTA integration tidak boleh menjadi satu kode besar.

Harus:

- Modular.
- Independently scalable.
- Observable.
- Maintainable.

---

# 10. Business Principle

Subscriber membeli kemudahan orkestrasi.

Nilai ChannelHub bukan hanya koneksi API, tetapi:

- Mengurangi pekerjaan manual.
- Mengurangi overbooking.
- Menyatukan banyak channel.
- Memberikan satu pusat kontrol operasional.
