# ChannelHub Role Permission System

## Purpose

Mendefinisikan sistem permission dinamis yang dapat dikonfigurasi melalui Super Admin.

---

# 1. Permission Principle

Role adalah kumpulan permission.

Permission menentukan aksi spesifik.

Contoh:

```
Property Manager

VIEW_PROPERTY
EDIT_PROPERTY
CREATE_ROOM
```

---

# 2. Permission Model

```
User
 |
Role
 |
Permission
 |
Feature
```

---

# 3. Permission Type

CRUD:

- Create.
- Read.
- Update.
- Delete.

Business action:

- Approve.
- Export.
- Publish.
- Sync.

---

# 4. Dynamic Menu

Menu tidak hardcoded.

Flow:

```
Login
 |
Permission Check
 |
Generate Menu
 |
Display UI
```

---

# 5. Super Admin Control

Super Admin dapat:

- Membuat role.
- Mengubah permission.
- Mengatur menu.
- Mengatur feature access.
- Melihat audit perubahan.

---

# 6. Approval Support

Permission tertentu membutuhkan approval.

Contoh:

```
Staff
  |
Request Price Change
  |
Manager Approval
```

---

# 7. Audit

Semua perubahan permission dicatat:

- Actor.
- Change.
- Timestamp.
- Previous value.
- New value.
