# ChannelHub Output Configuration

## Purpose

Menetapkan struktur dan konfigurasi output directory untuk implementasi ChannelHub.

---

## Output Directory

Seluruh implementasi source code WAJIB ditempatkan di:

```
Repo: ArrowTin/channelhub
```

**Note:** Ini adalah repo terpisah dari dokumentasi (ArrowTin/gpt).

## Directory Structure

```
ArrowTin/channelhub/
├── apps/                          # Application layer
│   ├── backend/                   # NestJS backend application
│   │   ├── src/
│   │   │   ├── modules/           # Feature modules
│   │   │   ├── common/            # Shared utilities
│   │   │   ├── config/            # Configuration files
│   │   │   └── main.ts            # Entry point
│   │   ├── test/                  # Backend tests
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   ├── nest-cli.json
│   │   └── Dockerfile
│   │
│   └── frontend/                  # NextJS frontend application
│       ├── src/
│       │   ├── app/               # App router (Next.js 13+)
│       │   ├── components/        # React components
│       │   ├── lib/               # Utilities
│       │   ├── styles/            # Global styles
│       │   └── public/            # Static assets
│       ├── test/                  # Frontend tests
│       ├── package.json
│       ├── tsconfig.json
│       ├── next.config.js
│       ├── tailwind.config.js
│       └── Dockerfile
│
├── services/                      # Microservices (optional, future)
│   ├── auth-service/
│   ├── property-service/
│   ├── reservation-service/
│   └── channel-sync-service/
│
├── packages/                      # Shared packages
│   ├── types/                     # Shared TypeScript types
│   ├── utils/                     # Shared utilities
│   ├── config/                    # Shared configuration
│   └── ui/                        # Shared UI components
│
├── infrastructure/                # Infrastructure as Code
│   ├── terraform/                 # Terraform configurations
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   ├── vpc/
│   │   ├── eks/
│   │   ├── rds/
│   │   ├── elasticache/
│   │   ├── s3/
│   │   └── cloudfront/
│   ├── kubernetes/                # Kubernetes manifests
│   │   ├── namespaces/
│   │   ├── deployments/
│   │   ├── services/
│   │   ├── configmaps/
│   │   └── secrets/
│   ├── docker/                    # Docker configurations
│   │   ├── docker-compose.yml
│   │   ├── docker-compose.prod.yml
│   │   └── docker-compose.dev.yml
│   └── monitoring/                # Monitoring configurations
│       ├── datadog/
│       ├── prometheus/
│       └── grafana/
│
├── tests/                         # E2E and integration tests
│   ├── e2e/                       # Playwright E2E tests
│   ├── integration/               # Integration tests
│   ├── performance/               # k6 performance tests
│   └── fixtures/                  # Test fixtures
│
├── scripts/                       # Utility scripts
│   ├── setup.sh
│   ├── build.sh
│   ├── deploy.sh
│   └── test.sh
│
├── docs/                          # Generated documentation
│   ├── api/                       # API documentation
│   ├── architecture/              # Architecture diagrams
│   └── guides/                    # User guides
│
├── .env.example                   # Environment variables example
├── .gitignore
├── docker-compose.yml
├── Makefile
└── README.md                      # App-specific README
```

## Implementation Rules

### 1. Output Directory Only

- **WAJIB**: Seluruh source code ditempatkan di repo `ArrowTin/channelhub`
- **DILARANG**: Membuat source code di repo ini (ArrowTin/gpt)
- **DILARANG**: Membuat source code di folder lain selain ArrowTin/channelhub

### 2. Structure Compliance

- Backend structure WAJIB mengikuti `docs/13-backend-foundation/009-backend-project-structure.md`
- Frontend structure WAJIB mengikuti `docs/14-frontend-foundation/009-frontend-project-structure.md`
- Database schema WAJIB mengikuti `docs/15-database-implementation/009-canonical-erd.md`

### 3. Configuration Files

- Environment variables di `.env` atau `.env.{environment}`
- Docker configuration di `infrastructure/docker/`
- Terraform configuration di `infrastructure/terraform/`
- Kubernetes manifests di `infrastructure/kubernetes/`

### 4. Testing

- Unit tests di setiap module/folder
- Integration tests di `tests/integration/`
- E2E tests di `tests/e2e/`
- Performance tests di `tests/performance/`

### 5. Documentation

- API documentation di `docs/api/`
- Architecture diagrams di `docs/architecture/`
- User guides di `docs/guides/`

## Environment Configuration

### Development

```bash
# ArrowTin/channelhub/.env.development
DATABASE_URL=postgresql://localhost:5432/channelhub_dev
REDIS_URL=redis://localhost:6379
API_PORT=3000
FRONTEND_PORT=3001
NODE_ENV=development
```

### Staging

```bash
# ArrowTin/channelhub/.env.staging
DATABASE_URL=postgresql://staging-db:5432/channelhub_staging
REDIS_URL=redis://staging-redis:6379
API_PORT=3000
FRONTEND_PORT=3001
NODE_ENV=staging
```

### Production

```bash
# ArrowTin/channelhub/.env.production
DATABASE_URL=postgresql://prod-db:5432/channelhub_prod
REDIS_URL=redis://prod-redis:6379
API_PORT=3000
FRONTEND_PORT=3001
NODE_ENV=production
```

## Build and Deployment

### Local Development

```bash
cd ArrowTin/channelhub
docker-compose up -d
```

### Build

```bash
cd ArrowTin/channelhub
npm run build:backend
npm run build:frontend
```

### Test

```bash
cd ArrowTin/channelhub
npm run test
npm run test:e2e
npm run test:performance
```

### Deploy

```bash
cd ArrowTin/channelhub/infrastructure/terraform
terraform init
terraform plan
terraform apply
```

## AI Implementation Instructions

### Untuk AI:

1. **Selalu** tempatkan source code di repo `ArrowTin/channelhub`
2. **Ikuti** struktur yang terdefinisi di atas
3. **Pastikan** nama file dan folder konsisten dengan CONTRACT ARTIFACT
4. **Tambahkan** test untuk setiap implementasi
5. **Update** dokumentasi jika ada perubahan struktur

### Contoh Prompt:

```
Implementasikan backend NestJS untuk auth module sesuai docs/13-backend-foundation/004-authentication-authorization-foundation.md.

Output WAJIB di: ArrowTin/channelhub/apps/backend/src/modules/auth/

Ikuti struktur:
- auth.module.ts
- auth.controller.ts
- auth.service.ts
- auth.dto.ts
- entities/
- guards/
- strategies/

Tambahkan unit test di: ArrowTin/channelhub/apps/backend/test/modules/auth/
```

END OF OUTPUT CONFIGURATION