# ChannelHub Prompt Engineering Standard

## Purpose

Membuat standar prompt agar AI menghasilkan implementasi yang konsisten.

---

# 1. Prompt Structure

Setiap prompt harus memiliki:

```
Context
Goal
Constraint
Implementation Detail
Validation
Output Requirement
```

---

# 2. Context Section

Berisi:

- Project identity.
- Architecture reference.
- Existing decision.
- Technology stack.

---

# 3. Implementation Section

Harus menjelaskan:

- File yang dibuat.
- Lokasi file.
- Responsibility.
- Dependency.
- Integration point.

---

# 4. Validation Section

AI harus memberikan:

- Test result.
- Potential issue.
- Next dependency.

---

# 5. Consistency Rule

Prompt baru tidak boleh bertentangan dengan blueprint sebelumnya.

Jika konflik ditemukan:

```
Stop
Analyze
Request Decision
Continue
```
