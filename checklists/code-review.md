# Code Review Checklist

## Purpose

Review hasil manusia atau AI sebelum merge.

## Code quality

- [ ] Naming & struktur folder sesuai standards/backend atau frontend
- [ ] Tidak ada hardcode config yang seharusnya metadata
- [ ] Dependency injection / module boundaries NestJS benar

## Security

- [ ] AuthN/AuthZ pada endpoint protected
- [ ] Input validation (DTO)
- [ ] SQL/ORM queries scoped tenant

## Observability

- [ ] Logging structured + correlation id
- [ ] Health/readiness jika service baru

## References

- docs/05-ai-development-blueprint/006-ai-review-checklist.md
- checklists/security-review.md
