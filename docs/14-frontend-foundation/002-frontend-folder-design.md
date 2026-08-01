# ChannelHub Frontend Folder Design Blueprint

## Purpose

Mendefinisikan struktur folder frontend agar konsisten.

---

# Structure

```
frontend/
 |
 +-- app/
 +-- components/
 +-- features/
 +-- services/
 +-- hooks/
 +-- utils/
 +-- tests/
```

---

# Feature Principle

Setiap feature dapat memiliki:

- Components.
- Hooks.
- API integration.
- Validation.
- Tests.

---

# Rule

Folder harus:

- Mudah ditemukan AI agent.
- Menghindari duplicate component.
- Memisahkan reusable dan feature code.
