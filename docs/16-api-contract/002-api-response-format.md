# ChannelHub API Response Format Blueprint

## Purpose

Menetapkan format response API yang konsisten.

---

# Response Structure

```
{
 success,
 data,
 message,
 error,
 metadata
}
```

---

# Success Response

Harus menyediakan:

- Data hasil proses.
- Informasi pagination bila diperlukan.
- Status operation.

---

# Error Response

Harus menyediakan:

- Error code.
- Human readable message.
- Detail validation.

---

# Goal

Client dapat menangani response secara predictable.
