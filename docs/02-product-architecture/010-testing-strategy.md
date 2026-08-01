# ChannelHub Testing Strategy

## Purpose

Mendefinisikan strategi testing agar sistem stabil dan mudah dikembangkan.

---

# Testing Pyramid

```
        E2E Test
      Integration Test
    Unit Test
```

---

# Unit Testing

Digunakan untuk:

- Business logic.
- Service function.
- Validation rule.

---

# Integration Testing

Menguji:

- Service communication.
- Database interaction.
- External API integration.

---

# End To End Testing

Menguji:

- User workflow.
- Subscription flow.
- OTA synchronization.
- Booking process.

---

# Quality Gate

CI/CD harus menjalankan:

- Test execution.
- Code quality check.
- Security scan.
- Build verification.
