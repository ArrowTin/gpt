# Global Implementation Rules

## Purpose

Dokumen ini berisi aturan global yang WAJIB dibaca oleh AI sebelum menjalankan task implementasi apa pun. Aturan ini memastikan konsistensi seluruh implementasi meskipun dikerjakan secara bertahap atau pada sesi AI yang berbeda.

---

## AI TRIGGER

### Tujuan Task
Menetapkan aturan dasar yang mengikat seluruh implementasi ChannelHub Enterprise agar tetap konsisten, terukur, dan dapat diandalkan.

### Konteks yang Perlu Dipahami AI
- ChannelHub adalah Hospitality Operating Platform dengan arsitektur DDD, Event Driven, API First
- Menggunakan NestJS (backend), Next.js (frontend), PostgreSQL (database), Redis (cache), BullMQ (queue)
- Configuration Driven Architecture - hal yang dapat berubah harus menjadi konfigurasi
- Multi-tenant, Security by Design, Observability First

### Dependensi
- README.md (prinsip dan filosofi arsitektur)
- .channelhub/START.md (kontrak eksekusi AI)
- docs/00-foundation/005-architecture-principles.md (prinsip arsitektur)
- docs/02-product-architecture/ (arsitektur produk dan teknis)

### File/Folder yang Perlu Diperiksa
- docs/13-backend-foundation/009-backend-project-structure.md
- docs/14-frontend-foundation/009-frontend-project-structure.md
- docs/15-database-implementation/009-canonical-erd.md
- docs/15-database-implementation/010-postgresql-ddl-reference.md
- contracts/openapi/channelhub.v1.yaml

### Langkah Implementasi
1. Baca dan pahami seluruh aturan global di bawah ini
2. Terapkan aturan ini pada setiap implementasi task
3. Jika terdapat konflik antar aturan, prioritaskan Contract Artifact
4. Tandai asumsi jika informasi kurang, jangan membuat keputusan sendiri

### Kriteria Keberhasilan (Definition of Done)
- AI memahami dan menerapkan seluruh aturan global
- Implementasi konsisten dengan Contract Artifact
- Tidak ada asumsi yang tidak terdokumentasi
- Seluruh cross-reference valid

### Prompt Implementasi
```
Anda adalah AI Engineer yang bertugas mengimplementasikan ChannelHub Enterprise.

SEBELUM memulai implementasi APAPUN, WAJIB membaca dan memahami seluruh aturan global di docs/00-foundation/009-global-implementation-rules.md.

Aturan ini mengikat seluruh implementasi dan TIDAK BOLEH dilanggar tanpa ADR baru.

Jika informasi kurang, TANDAI sebagai kebutuhan klarifikasi, JANGAN membuat keputusan sendiri.

Mulai implementasi dengan mengacu pada Contract Artifact terlebih dahulu.
```

---

## 1. CODING STANDARD

### 1.1 Bahasa Pemrograman
- **Backend**: TypeScript dengan NestJS framework
- **Frontend**: TypeScript dengan Next.js framework
- **Database**: PostgreSQL dengan DDL di `docs/15-database-implementation/010-postgresql-ddl-reference.md`
- **Migration**: Menggunakan tool yang sesuai dengan setup di Phase 15

### 1.2 Code Style
- Gunakan ESLint dan Prettier sesuai konfigurasi di Phase 12-14
- Indentasi: 2 spaces
- Maximum line length: 120 characters
- Gunakan semantic naming untuk variable, function, class
- Comment hanya untuk business logic yang kompleks, bukan untuk obvious code

### 1.3 Type Safety
- SELALU gunakan TypeScript strict mode
- Hindari `any` dan `unknown` kecuali ada alasan kuat
- Gunakan interface untuk public API, type untuk internal
- Export types yang digunakan lintas module

### 1.4 Error Handling
- Gunakan structured error handling dengan custom exception
- Log error dengan context yang cukup untuk debugging
- Jangan expose stack trace ke client
- Gunakan standard error code dari `docs/16-api-contract/010-error-code-catalog.md`

---

## 2. STRUKTUR FOLDER

### 2.1 Backend Structure
Ikuti struktur yang didefinisikan di `docs/13-backend-foundation/009-backend-project-structure.md`:

```
backend/
├── src/
│   ├── modules/           # Domain modules (DDD)
│   │   ├── {module}/
│   │   │   ├── domain/    # Entities, value objects, domain services
│   │   │   ├── application/ # Use cases, DTOs
│   │   │   ├── infrastructure/ # Repository implementation, external services
│   │   │   └── presentation/ # Controllers, GraphQL resolvers
│   ├── common/           # Shared utilities, guards, decorators
│   ├── config/           # Configuration files
│   └── main.ts
├── test/
└── package.json
```

### 2.2 Frontend Structure
Ikuti struktur yang didefinisikan di `docs/14-frontend-foundation/009-frontend-project-structure.md`:

```
frontend/
├── src/
│   ├── app/              # Next.js app directory
│   ├── components/       # Reusable components
│   ├── lib/              # Utilities, API clients
│   ├── hooks/            # Custom React hooks
│   ├── types/            # TypeScript types
│   └── styles/           # Global styles
├── public/
└── package.json
```

### 2.3 Database Structure
- Schema mengikuti `docs/15-database-implementation/009-canonical-erd.md`
- Migration file mengikuti urutan di `docs/15-database-implementation/010-postgresql-ddl-reference.md`
- Setiap module memiliki schema terpisah jika multi-tenant schema isolation

---

## 3. NAMING CONVENTION

### 3.1 Database
- Table names: snake_case, plural (e.g., `users`, `reservations`)
- Column names: snake_case (e.g., `created_at`, `is_active`)
- Index names: `idx_{table}_{column}` (e.g., `idx_users_email`)
- Foreign key: `fk_{table}_{referenced_table}` (e.g., `fk_reservations_users`)
- Enum names: PascalCase (e.g., `ReservationStatus`)

### 3.2 Backend
- Class names: PascalCase (e.g., `UserService`, `ReservationController`)
- Method names: camelCase (e.g., `createReservation`, `getUserById`)
- Variable names: camelCase (e.g., `userId`, `reservationData`)
- Constants: UPPER_SNAKE_CASE (e.g., `MAX_RETRY_COUNT`)
- File names: kebab-case (e.g., `user.service.ts`, `reservation.controller.ts`)

### 3.3 Frontend
- Component names: PascalCase (e.g., `ReservationList`, `UserForm`)
- File names: PascalCase untuk components (e.g., `ReservationList.tsx`)
- Hook names: camelCase dengan prefix `use` (e.g., `useReservations`, `useAuth`)
- API functions: camelCase (e.g., `fetchReservations`, `createUser`)

### 3.4 API Endpoints
- URL paths: kebab-case, plural (e.g., `/api/v1/reservations`, `/api/v1/users`)
- Query parameters: snake_case (e.g., `user_id`, `created_at`)
- Response fields: camelCase (e.g., `userId`, `createdAt`)

---

## 4. ARSITEKTUR SISTEM

### 4.1 Domain Driven Design (DDD)
- Setiap module domain terpisah (User, Property, Reservation, Channel, dll)
- Domain layer tidak bergantung pada infrastructure layer
- Use case di application layer mengorkestrasi domain logic
- Repository interface di domain, implementation di infrastructure

### 4.2 Event Driven Architecture
- Gunakan event untuk cross-module communication
- Event names: `{Domain}{Action}` (e.g., `ReservationCreated`, `UserUpdated`)
- Gunakan BullMQ untuk async event processing
- Event handler harus idempotent

### 4.3 API First
- SELALU definisikan API contract di `contracts/openapi/channelhub.v1.yaml` sebelum implementasi
- Backend mengikuti contract, frontend mengikuti contract
- Jangan bypass contract dengan direct database access

### 4.4 Configuration Driven
- Business logic yang dapat berubah harus menjadi konfigurasi di database
- Contoh: menu, role, permission, workflow, pricing, notification template
- Configuration di-load saat startup atau cached dengan Redis

### 4.5 Multi-Tenant
- Tenant isolation di semua layer: database, API, cache, queue
- Tenant context disimpan di request header atau JWT
- SELALU filter data by tenant di semua query
- Cross-tenant data access hanya dengan explicit permission

---

## 5. DEPENDENCY RULES

### 5.1 Backend Dependencies
- Gunakan package yang sudah stabilized (minimal 7 hari sejak publish)
- Hindari dependency yang memiliki security vulnerability
- Prefer official NestJS packages untuk integrasi umum
- Lock version di package.json untuk production

### 5.2 Frontend Dependencies
- Gunakan Next.js stable version
- Prefer component library yang konsisten dengan design system
- Hindari dependency duplication
- Review bundle size impact untuk setiap dependency baru

### 5.3 Dependency Direction
- Domain layer → TIDAK bergantung → Infrastructure layer
- Application layer → BISA bergantung → Domain layer
- Infrastructure layer → BISA bergantung → Domain layer
- Presentation layer → BISA bergantung → Application layer

---

## 6. UI/UX RULES

### 6.1 Design System
- Gunakan design system yang didefinisikan di `docs/14-frontend-foundation/003-design-system-foundation.md`
- Konsisten warna, typography, spacing, border-radius
- Gunakan component library yang tersedia

### 6.2 Responsive Design
- Mobile-first approach
- Breakpoint mengikuti standar (sm, md, lg, xl)
- Test pada berbagai screen size

### 6.3 Accessibility
- Gunakan semantic HTML
- ARIA labels untuk interactive elements
- Keyboard navigation support
- Color contrast ratio minimal 4.5:1

### 6.4 Performance
- Lazy loading untuk route dan component
- Image optimization dengan Next.js Image
- Code splitting untuk large bundle
- Client-side caching dengan SWR atau React Query

---

## 7. SECURITY RULES

### 7.1 Authentication
- Gunakan JWT dengan refresh token
- Token expiration: access token 15 menit, refresh token 7 hari
- Hash password dengan bcrypt (cost factor 12)
- Rate limiting untuk login endpoint

### 7.2 Authorization
- Role-Based Access Control (RBAC)
- Resource-based permission untuk fine-grained control
- SELALU cek permission di server-side
- Tenant isolation enforced di semua layer

### 7.3 Data Security
- Sensitive data (password, token) di-hash/encrypt
- Jangan log sensitive data
- Gunakan HTTPS untuk semua komunikasi
- Input validation dan sanitization

### 7.4 API Security
- Rate limiting per user dan per endpoint
- CORS configuration yang ketat
- Input validation dengan class-validator
- SQL injection prevention dengan parameterized query

---

## 8. PERFORMANCE RULES

### 8.1 Database Performance
- Gunakan index untuk column yang sering di-query
- Avoid N+1 query dengan proper join atau eager loading
- Pagination untuk list endpoint (max 100 items per page)
- Connection pooling dengan konfigurasi optimal

### 8.2 Caching Strategy
- Cache data yang sering di-access tapi jarang berubah dengan Redis
- Cache invalidation yang proper
- Gunakan cache tag untuk group invalidation
- Response caching untuk GET endpoint yang idempotent

### 8.3 API Performance
- Response time p95 < 500ms untuk endpoint utama
- Compression untuk response > 1KB
- Batching untuk multiple related request
- Async processing untuk long-running task

### 8.4 Frontend Performance
- First Contentful Paint (FCP) < 1.5s
- Time to Interactive (TTI) < 3.5s
- Cumulative Layout Shift (CLS) < 0.1
- Total bundle size < 500KB (gzipped)

---

## 9. TESTING RULES

### 9.1 Testing Pyramid
- Unit test: 70% (domain logic, utility functions)
- Integration test: 20% (API endpoint, database operation)
- E2E test: 10% (critical user journey)

### 9.2 Backend Testing
- Unit test untuk domain service dan use case
- Integration test untuk controller dan repository
- Test coverage minimal 80%
- Mock external service (OTA, payment gateway)

### 9.3 Frontend Testing
- Unit test untuk utility function dan custom hook
- Component test untuk UI component
- E2E test dengan Playwright untuk critical flow
- Visual regression test untuk design system

### 9.4 Database Testing
- Migration test untuk setiap perubahan schema
- Seed data test untuk initial data
- Performance test untuk query yang berat

---

## 10. GIT WORKFLOW

### 10.1 Branch Strategy
- `main`: production ready code
- `develop`: integration branch
- `feature/{ticket}-{description}`: feature branch
- `bugfix/{ticket}-{description}`: bugfix branch
- `hotfix/{ticket}-{description}`: hotfix untuk production

### 10.2 Commit Convention
- Format: `{type}({scope}): {description}`
- Type: feat, fix, docs, style, refactor, test, chore
- Scope: module yang diubah (user, reservation, dll)
- Example: `feat(reservation): add cancellation workflow`

### 10.3 Pull Request
- PR harus melalui Code Review
- Minimal 1 approval untuk merge
- CI harus passing
- PR description harus mencakup: what, why, testing

### 10.4 Code Review
- Review fokus pada: correctness, design, maintainability
- Comment yang constructive dan specific
- Reviewer harus familiar dengan domain
- Author harus response semua comment

---

## 11. DOKUMENTASI YANG WAJIB DIPERBARUI

### 11.1 Setelah Task Selesai
AI WAJIB memperbarui dokumentasi berikut:

1. **.channelhub/STATE.yml**
   - Update phase jika milestone selesai
   - Update current work item

2. **.channelhub/CHANGELOG.md**
   - Tambahkan entry baru dengan format:
     ```markdown
     ## [YYYY-MM-DD] - Phase NN - Milestone Name
     ### Added
     - [feature description]
     ### Changed
     - [change description]
     ### Fixed
     - [fix description]
     ```

3. **Contract Artifact (jika berubah)**
   - `contracts/openapi/channelhub.v1.yaml`
   - `docs/15-database-implementation/009-canonical-erd.md`
   - `docs/15-database-implementation/010-postgresql-ddl-reference.md`

4. **Cross-Reference**
   - Update link di docs yang relevan
   - Pastikan tidak ada broken link

### 11.2 ADR (Architecture Decision Record)
- Buat ADR baru untuk perubahan arsitektur signifikan
- Gunakan template di `templates/adr-template.md`
- Review dan approve ADR sebelum implementasi

### 11.3 README
- Update README.md jika ada perubahan besar yang mempengaruhi overview
- Tambahkan note di CHANGELOG untuk perubahan yang user-facing

---

## 12. CONTRACT ARTIFACT COMPLIANCE

### 12.1 Mandatory References
SEBELUM implementasi apapun, AI WAJIB membaca:

1. `contracts/openapi/channelhub.v1.yaml` - API contract
2. `docs/15-database-implementation/009-canonical-erd.md` - Database schema
3. `docs/15-database-implementation/010-postgresql-ddl-reference.md` - DDL reference
4. `docs/13-backend-foundation/009-backend-project-structure.md` - Backend structure
5. `docs/14-frontend-foundation/009-frontend-project-structure.md` - Frontend structure

### 12.2 No Guessing Rule
- TIDAK BOLEH menebak schema database
- TIDAK BOLEH menebak API endpoint
- TIDAK BOLEH menebak field name atau type
- Jika informasi kurang, TANDAI sebagai kebutuhan klarifikasi

### 12.3 Contract First
- Perubahan kontrak dilakukan pada file kontrak lebih dulu
- Kode mengikuti kontrak, bukan sebaliknya
- Kontrak yang berubah harus update di Contract Artifact

---

## 13. AI SESSION MANAGEMENT

### 13.1 Session Bootstrap
Setiap sesi AI WAJIB dimulai dengan:

1. Baca `README.md`
2. Baca `.channelhub/START.md`
3. Baca `.channelhub/STATE.yml`
4. Baca `docs/00-foundation/009-global-implementation-rules.md` (file ini)
5. Baca phase-specific documentation
6. Baca contract artifact yang relevan

### 13.2 Task Execution
- Kerjakan SATU micro-prompt per sesi
- Jangan berhenti hanya karena tidak bisa scan repository
- Gunakan START.md sebagai failsafe context
- Tandai asumsi yang dipakai

### 13.3 Context Continuity
Setiap sesi harus membawa:
- Current phase dan milestone
- Completed files dan task
- Pending task dan next step
- Known decisions dan ADR yang relevan

---

## 14. QUALITY GATES

### 14.1 Definition of Done
Task dianggap selesai jika:

- [ ] Code tersedia dan sesuai spec
- [ ] Test tersedia dan passing
- [ ] Documentation diperbarui
- [ ] Cross-reference valid
- [ ] Contract artifact di-sync (jika berubah)
- [ ] Code review selesai (untuk production code)
- [ ] CI/CD passing

### 14.2 Quality Checklist
Gunakan checklist di `checklists/`:
- `checklists/documentation-quality.md`
- `checklists/code-review.md`
- `checklists/security-review.md`
- `checklists/testing-release.md`

### 14.3 Self-Validation
Sebelum menandai task selesai:
- Review sendiri code dan documentation
- Cek consistency dengan Contract Artifact
- Verify cross-reference tidak broken
- Test manual jika diperlukan

---

## 15. EMERGENCY PROCEDURES

### 15.1 Failsafe Mode
Jika AI tidak bisa:
- Scan repository
- List directory
- Baca file selain README dan START.md

MAKA:
- Gunakan START.md sebagai working context
- JANGAN berhenti bekerja
- Kerjakan HANYA satu deliverable
- Tandai asumsi yang dipakai

### 15.2 Rollback Procedure
Jika implementasi menyebabkan issue:
- Revert perubahan dengan git
- Investigate root cause
- Update documentation jika perlu
- Re-apply dengan fix

### 15.3 Escalation
Jika tidak bisa resolve issue:
- Dokumentasikan issue yang dihadapi
- Tandai sebagai blocked task
- Lanjut ke task lain jika possible
- Report untuk human intervention

---

## SUMMARY

Aturan global ini adalah single source of truth untuk implementasi ChannelHub Enterprise. SELURUH implementasi WAJIB mengikuti aturan ini untuk memastikan konsistensi, kualitas, dan maintainability.

**KEY POINTS:**
1. Contract Artifact adalah sumber kebenaran teknis
2. Jangan menebak - tanyakan atau tandai asumsi
3. Konsistensi lebih penting dari speed
4. Quality gates wajib dilewati
5. Documentation harus selalu diperbarui

**REMEMBER:**
- Configuration Driven - hal yang berubah jadi konfigurasi
- Domain Driven Design - module domain terpisah
- API First - contract dulu, kode kemudian
- Multi-Tenant - isolation di semua layer
- Security by Design - security dari awal

**START HERE:**
1. Baca `.channelhub/START.md`
2. Baca file ini (Global Rules)
3. Baca phase-specific documentation
4. Baca Contract Artifact
5. Jalankan micro-prompt

END OF GLOBAL IMPLEMENTATION RULES