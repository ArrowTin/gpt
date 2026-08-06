# ChannelHub Test Automation Setup

## Purpose

Menetapkan setup test automation untuk seluruh stack ChannelHub (backend, frontend, database, integration).

---

## AI TRIGGER

### Tujuan Task
Menetapkan konfigurasi test automation framework untuk seluruh stack ChannelHub.

### Konteks yang Perlu Dipahami AI
- Testing pyramid: Unit (70%), Integration (20%), E2E (10%)
- Backend: NestJS dengan Jest
- Frontend: Next.js dengan Jest dan Playwright
- Database: Test database terpisah dengan proper seeding
- Coverage target: Unit >80%, Integration >70%, E2E >60%

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/02-product-architecture/010-testing-strategy.md (testing strategy)
- docs/13-backend-foundation/009-backend-project-structure.md (backend structure)
- docs/14-frontend-foundation/009-frontend-project-structure.md (frontend structure)
- .channelhub/OUTPUT-CONFIG.md (output directory structure)

### File/Folder yang Perlu Diperiksa
- docs/18-backend-implementation/004-service-testing-strategy.md (backend testing)
- docs/15-database-implementation/004-data-seed-strategy.md (data seed)

### Langkah Implementasi
1. Setup Jest untuk backend NestJS
2. Setup Jest untuk frontend Next.js
3. Setup Playwright untuk E2E testing
4. Setup test database configuration
5. Setup test coverage reporting
6. Setup test scripts di package.json

### Kriteria Keberhasilan (Definition of Done)
- Test framework terkonfigurasi untuk backend dan frontend
- Test database terpisah dengan proper seeding
- Coverage target terkonfigurasi
- Test scripts tersedia di package.json
- Test dapat dijalankan dengan single command

### Prompt Implementasi
```
Anda akan mengkonfigurasi test automation untuk ChannelHub.

Baca docs/02-product-architecture/011-test-automation-setup.md untuk memahami setup test automation.

Backend Testing (NestJS + Jest):
1. Install dependencies:
   - @nestjs/testing
   - jest
   - @types/jest
   - supertest (untuk HTTP testing)
   - ts-jest
   - jest-config

2. Konfigurasi jest.config.js:
   - Test environment: node
   - Coverage directory: coverage
   - Coverage threshold: global 80%, branches 80%, functions 80%, lines 80%
   - Setup files untuk test database

3. Test database setup:
   - Gunakan environment variable TEST_DATABASE_URL
   - Separate test database dari development
   - Run migration sebelum test
   - Seed test data sebelum test
   - Cleanup database setelah test

4. Test structure:
   - Unit test: *.spec.ts di setiap module
   - Integration test: *.e2e-spec.ts di test/e2e/
   - Mock external service (OTA, payment gateway)

Frontend Testing (Next.js + Jest + Playwright):
1. Install dependencies:
   - @testing-library/react
   - @testing-library/jest-dom
   - jest-environment-jsdom
   - @playwright/test

2. Konfigurasi jest.config.js:
   - Test environment: jsdom
   - Setup files untuk testing library
   - Module mocking untuk external dependencies

3. E2E testing dengan Playwright:
   - Install @playwright/test
   - Konfigurasi playwright.config.ts
   - Test environment: chromium, firefox, webkit
   - Base URL dari environment variable

4. Test structure:
   - Unit test: *.test.tsx di setiap component
   - Component test: testing-library
   - E2E test: test/e2e/ dengan Playwright

Test Scripts di package.json:
{
  "test": "jest",
  "test:watch": "jest --watch",
  "test:cov": "jest --coverage",
  "test:e2e": "playwright test",
  "test:e2e:ui": "playwright test --ui",
  "test:backend": "jest --testPathPattern apps/backend",
  "test:frontend": "jest --testPathPattern apps/frontend"
}

Coverage Reporting:
- Generate coverage report dengan istanbul
- Output format: html, lcov, json
- Upload ke Codecov atau similar untuk CI
- Coverage threshold enforcement di CI

Test Data Management:
- Gunakan factory pattern untuk test data
- Seed data yang konsisten untuk integration test
- Cleanup database setelah test suite
- Use transaction rollback untuk isolation

Pastikan test automation terkonfigurasi dengan benar dan dapat dijalankan dengan single command.
```

---

---

## AI TRIGGER

### Tujuan Task
Menetapkan konfigurasi test automation framework untuk seluruh stack ChannelHub.

### Konteks yang Perlu Dipahami AI
- Testing pyramid: Unit (70%), Integration (20%), E2E (10%)
- Backend: NestJS dengan Jest
- Frontend: Next.js dengan Jest dan Playwright
- Database: Test database terpisah dengan proper seeding
- Coverage target: Unit >80%, Integration >70%, E2E >60%

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/02-product-architecture/010-testing-strategy.md (testing strategy)
- docs/13-backend-foundation/009-backend-project-structure.md (backend structure)
- docs/14-frontend-foundation/009-frontend-project-structure.md (frontend structure)
- .channelhub/OUTPUT-CONFIG.md (output directory structure)

### File/Folder yang Perlu Diperiksa
- docs/18-backend-implementation/004-service-testing-strategy.md (backend testing)
- docs/15-database-implementation/004-data-seed-strategy.md (data seed)

### Langkah Implementasi
1. Setup Jest untuk backend NestJS
2. Setup Jest untuk frontend Next.js
3. Setup Playwright untuk E2E testing
4. Setup test database configuration
5. Setup test coverage reporting
6. Setup test scripts di package.json

### Kriteria Keberhasilan (Definition of Done)
- Test framework terkonfigurasi untuk backend dan frontend
- Test database terpisah dengan proper seeding
- Coverage target terkonfigurasi
- Test scripts tersedia di package.json
- Test dapat dijalankan dengan single command

### Prompt Implementasi
```
Anda akan mengkonfigurasi test automation untuk ChannelHub.

Baca docs/02-product-architecture/011-test-automation-setup.md untuk memahami setup test automation.

Backend Testing (NestJS + Jest):
1. Install dependencies:
   - @nestjs/testing
   - jest
   - @types/jest
   - supertest (untuk HTTP testing)
   - ts-jest
   - jest-config

2. Konfigurasi jest.config.js:
   - Test environment: node
   - Coverage directory: coverage
   - Coverage threshold: global 80%, branches 80%, functions 80%, lines 80%
   - Setup files untuk test database

3. Test database setup:
   - Gunakan environment variable TEST_DATABASE_URL
   - Separate test database dari development
   - Run migration sebelum test
   - Seed test data sebelum test
   - Cleanup database setelah test

4. Test structure:
   - Unit test: *.spec.ts di setiap module
   - Integration test: *.e2e-spec.ts di test/e2e/
   - Mock external service (OTA, payment gateway)

Frontend Testing (Next.js + Jest + Playwright):
1. Install dependencies:
   - @testing-library/react
   - @testing-library/jest-dom
   - jest-environment-jsdom
   - @playwright/test

2. Konfigurasi jest.config.js:
   - Test environment: jsdom
   - Setup files untuk testing library
   - Module mocking untuk external dependencies

3. E2E testing dengan Playwright:
   - Install @playwright/test
   - Konfigurasi playwright.config.ts
   - Test environment: chromium, firefox, webkit
   - Base URL dari environment variable

4. Test structure:
   - Unit test: *.test.tsx di setiap component
   - Component test: testing-library
   - E2E test: test/e2e/ dengan Playwright

Test Scripts di package.json:
{
  "test": "jest",
  "test:watch": "jest --watch",
  "test:cov": "jest --coverage",
  "test:e2e": "playwright test",
  "test:e2e:ui": "playwright test --ui",
  "test:backend": "jest --testPathPattern apps/backend",
  "test:frontend": "jest --testPathPattern apps/frontend"
}

Coverage Reporting:
- Generate coverage report dengan istanbul
- Output format: html, lcov, json
- Upload ke Codecov atau similar untuk CI
- Coverage threshold enforcement di CI

Test Data Management:
- Gunakan factory pattern untuk test data
- Seed data yang konsisten untuk integration test
- Cleanup database setelah test suite
- Use transaction rollback untuk isolation

Pastikan test automation terkonfigurasi dengan benar dan dapat dijalankan dengan single command.
```

---

## AI TRIGGER

### Tujuan Task
Menetapkan konfigurasi test automation framework untuk seluruh stack ChannelHub.

### Konteks yang Perlu Dipahami AI
- Testing pyramid: Unit (70%), Integration (20%), E2E (10%)
- Backend: NestJS dengan Jest
- Frontend: Next.js dengan Jest dan Playwright
- Database: Test database terpisah dengan proper seeding
- Coverage target: Unit >80%, Integration >70%, E2E >60%

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/02-product-architecture/010-testing-strategy.md (testing strategy)
- docs/13-backend-foundation/009-backend-project-structure.md (backend structure)
- docs/14-frontend-foundation/009-frontend-project-structure.md (frontend structure)
- .channelhub/OUTPUT-CONFIG.md (output directory structure)

### File/Folder yang Perlu Diperiksa
- docs/18-backend-implementation/004-service-testing-strategy.md (backend testing)
- docs/15-database-implementation/004-data-seed-strategy.md (data seed)

### Langkah Implementasi
1. Setup Jest untuk backend NestJS
2. Setup Jest untuk frontend Next.js
3. Setup Playwright untuk E2E testing
4. Setup test database configuration
5. Setup test coverage reporting
6. Setup test scripts di package.json

### Kriteria Keberhasilan (Definition of Done)
- Test framework terkonfigurasi untuk backend dan frontend
- Test database terpisah dengan proper seeding
- Coverage target terkonfigurasi
- Test scripts tersedia di package.json
- Test dapat dijalankan dengan single command

### Prompt Implementasi
```
Anda akan mengkonfigurasi test automation untuk ChannelHub.

Baca docs/02-product-architecture/011-test-automation-setup.md untuk memahami setup test automation.

Backend Testing (NestJS + Jest):
1. Install dependencies:
   - @nestjs/testing
   - jest
   - @types/jest
   - supertest (untuk HTTP testing)
   - ts-jest
   - jest-config

2. Konfigurasi jest.config.js:
   - Test environment: node
   - Coverage directory: coverage
   - Coverage threshold: global 80%, branches 80%, functions 80%, lines 80%
   - Setup files untuk test database

3. Test database setup:
   - Gunakan environment variable TEST_DATABASE_URL
   - Separate test database dari development
   - Run migration sebelum test
   - Seed test data sebelum test
   - Cleanup database setelah test

4. Test structure:
   - Unit test: *.spec.ts di setiap module
   - Integration test: *.e2e-spec.ts di test/e2e/
   - Mock external service (OTA, payment gateway)

Frontend Testing (Next.js + Jest + Playwright):
1. Install dependencies:
   - @testing-library/react
   - @testing-library/jest-dom
   - jest-environment-jsdom
   - @playwright/test

2. Konfigurasi jest.config.js:
   - Test environment: jsdom
   - Setup files untuk testing library
   - Module mocking untuk external dependencies

3. E2E testing dengan Playwright:
   - Install @playwright/test
   - Konfigurasi playwright.config.ts
   - Test environment: chromium, firefox, webkit
   - Base URL dari environment variable

4. Test structure:
   - Unit test: *.test.tsx di setiap component
   - Component test: testing-library
   - E2E test: test/e2e/ dengan Playwright

Test Scripts di package.json:
{
  "test": "jest",
  "test:watch": "jest --watch",
  "test:cov": "jest --coverage",
  "test:e2e": "playwright test",
  "test:e2e:ui": "playwright test --ui",
  "test:backend": "jest --testPathPattern apps/backend",
  "test:frontend": "jest --testPathPattern apps/frontend"
}

Coverage Reporting:
- Generate coverage report dengan istanbul
- Output format: html, lcov, json
- Upload ke Codecov atau similar untuk CI
- Coverage threshold enforcement di CI

Test Data Management:
- Gunakan factory pattern untuk test data
- Seed data yang konsisten untuk integration test
- Cleanup database setelah test suite
- Use transaction rollback untuk isolation

Pastikan test automation terkonfigurasi dengan benar dan dapat dijalankan dengan single command.
```

---

## Backend Test Setup (NestJS + Jest)

### Dependencies

```json
{
  "devDependencies": {
    "@nestjs/testing": "^10.0.0",
    "@types/jest": "^29.5.0",
    "@types/supertest": "^2.0.12",
    "jest": "^29.5.0",
    "supertest": "^6.3.0",
    "ts-jest": "^29.1.0",
    "ts-node": "^10.9.0"
  }
}
```

### Jest Configuration

```javascript
// jest.config.js
module.exports = {
  moduleFileExtensions: ['js', 'json', 'ts'],
  rootDir: '.',
  testRegex: '.*\\.spec\\.ts$',
  transform: {
    '^.+\\.(t|j)s$': 'ts-jest',
  },
  collectCoverageFrom: [
    '**/*.(t|j)s',
    '!**/node_modules/**',
    '!**/test/**',
    '!**/dist/**',
    '!**/src/main.ts',
    '!**/src/**/*.module.ts',
    '!**/src/**/*.dto.ts',
    '!**/src/**/*.interface.ts',
  ],
  coverageDirectory: './coverage',
  testEnvironment: 'node',
  roots: ['<rootDir>/src', '<rootDir>/test'],
  moduleNameMapper: {
    '^src/(.*)$': '<rootDir>/src/$1',
    '^test/(.*)$': '<rootDir>/test/$1',
  },
  setupFilesAfterEnv: ['<rootDir>/test/setup.ts'],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
};
```

### Test Database Setup

```typescript
// test/setup.ts
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { DataSource } from 'typeorm';

let app: INestApplication;
let dataSource: DataSource;

beforeAll(async () => {
  // Setup test database
  process.env.DATABASE_URL = process.env.TEST_DATABASE_URL;
  
  // Run migrations
  // await runMigrations();
  
  // Seed test data
  // await seedTestData();
});

afterAll(async () => {
  // Cleanup test database
  if (app) await app.close();
  if (dataSource) await dataSource.destroy();
});

beforeEach(async () => {
  // Setup transaction for each test
  // await dataSource.query('BEGIN');
});

afterEach(async () => {
  // Rollback transaction for isolation
  // await dataSource.query('ROLLBACK');
});
```

### Unit Test Example

```typescript
// users/users.service.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { UsersService } from './users.service';
import { getRepositoryToken } from '@nestjs/typeorm';
import { User } from './entities/user.entity';

describe('UsersService', () => {
  let service: UsersService;
  let mockUserRepository;

  beforeEach(async () => {
    mockUserRepository = {
      findOne: jest.fn(),
      create: jest.fn(),
      save: jest.fn(),
      find: jest.fn(),
    };

    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UsersService,
        {
          provide: getRepositoryToken(User),
          useValue: mockUserRepository,
        },
      ],
    }).compile();

    service = module.get<UsersService>(UsersService);
  });

  it('should be defined', () => {
    expect(service).toBeDefined();
  });

  it('should find user by email', async () => {
    const mockUser = { id: '1', email: 'test@example.com' };
    mockUserRepository.findOne.mockResolvedValue(mockUser);

    const result = await service.findByEmail('test@example.com');
    expect(result).toEqual(mockUser);
    expect(mockUserRepository.findOne).toHaveBeenCalledWith({
      where: { email: 'test@example.com' },
    });
  });
});
```

### Integration Test Example

```typescript
// users/users.controller.e2e-spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import * as request from 'supertest';
import { AppModule } from '../src/app.module';

describe('UsersController (e2e)', () => {
  let app: INestApplication;

  beforeEach(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });

  it('/users (GET)', () => {
    return request(app.getHttpServer())
      .get('/users')
      .expect(200)
      .expect('Content-Type', /json/);
  });

  afterAll(async () => {
    await app.close();
  });
});
```

---

## Frontend Test Setup (Next.js + Jest + Playwright)

### Dependencies

```json
{
  "devDependencies": {
    "@testing-library/react": "^14.0.0",
    "@testing-library/jest-dom": "^6.1.0",
    "@testing-library/user-event": "^14.4.0",
    "@playwright/test": "^1.40.0",
    "jest": "^29.5.0",
    "jest-environment-jsdom": "^29.5.0",
    "@types/jest": "^29.5.0"
  }
}
```

### Jest Configuration

```javascript
// jest.config.js
const nextJest = require('next/jest')

const createJestConfig = nextJest({
  dir: './',
})

const customJestConfig = {
  setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
  testEnvironment: 'jest-environment-jsdom',
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/src/$1',
  },
  collectCoverageFrom: [
    'src/**/*.{js,jsx,ts,tsx}',
    '!src/**/*.d.ts',
    '!src/**/*.stories.{js,jsx,ts,tsx}',
    '!src/**/__tests__/**',
  ],
  coverageThreshold: {
    global: {
      branches: 70,
      functions: 70,
      lines: 70,
      statements: 70,
    },
  },
}

module.exports = createJestConfig(customJestConfig)
```

### Jest Setup

```javascript
// jest.setup.js
import '@testing-library/jest-dom'

// Mock Next.js router
jest.mock('next/navigation', () => ({
  useRouter() {
    return {
      push: jest.fn(),
      replace: jest.fn(),
      prefetch: jest.fn(),
    }
  },
  useSearchParams() {
    return new URLSearchParams()
  },
  usePathname() {
    return '/'
  },
}))

// Mock environment variables
process.env.NEXT_PUBLIC_API_URL = 'http://localhost:3000'
```

### Component Test Example

```typescript
// components/Button/Button.test.tsx
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { Button } from './Button'

describe('Button', () => {
  it('renders button with text', () => {
    render(<Button>Click me</Button>)
    expect(screen.getByRole('button', { name: /click me/i })).toBeInTheDocument()
  })

  it('calls onClick when clicked', async () => {
    const handleClick = jest.fn()
    const user = userEvent.setup()
    
    render(<Button onClick={handleClick}>Click me</Button>)
    
    await user.click(screen.getByRole('button'))
    expect(handleClick).toHaveBeenCalledTimes(1)
  })

  it('is disabled when disabled prop is true', () => {
    render(<Button disabled>Click me</Button>)
    expect(screen.getByRole('button')).toBeDisabled()
  })
})
```

### Playwright Configuration

```typescript
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test'

export default defineConfig({
  testDir: './test/e2e',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: 'html',
  use: {
    baseURL: process.env.E2E_BASE_URL || 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },

  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
  ],

  webServer: {
    command: 'npm run dev',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
  },
})
```

### E2E Test Example

```typescript
// test/e2e/login.spec.ts
import { test, expect } from '@playwright/test'

test('user can login with valid credentials', async ({ page }) => {
  await page.goto('/login')
  
  await page.fill('input[name="email"]', 'test@example.com')
  await page.fill('input[name="password"]', 'password123')
  await page.click('button[type="submit"]')
  
  await expect(page).toHaveURL('/dashboard')
  await expect(page.locator('h1')).toContainText('Dashboard')
})

test('shows error with invalid credentials', async ({ page }) => {
  await page.goto('/login')
  
  await page.fill('input[name="email"]', 'invalid@example.com')
  await page.fill('input[name="password"]', 'wrongpassword')
  await page.click('button[type="submit"]')
  
  await expect(page.locator('.error-message')).toBeVisible()
  await expect(page.locator('.error-message')).toContainText('Invalid credentials')
})
```

---

## Test Data Management

### Factory Pattern

```typescript
// test/factories/user.factory.ts
import { User } from '../../src/users/entities/user.entity'

export class UserFactory {
  static create(overrides: Partial<User> = {}): User {
    return {
      id: 'test-user-id',
      email: overrides.email || 'test@example.com',
      passwordHash: overrides.passwordHash || 'hashed-password',
      fullName: overrides.fullName || 'Test User',
      status: overrides.status || 'ACTIVE',
      isSuperAdmin: overrides.isSuperAdmin || false,
      createdAt: new Date(),
      updatedAt: new Date(),
      ...overrides,
    }
  }

  static createMany(count: number, overrides: Partial<User> = {}): User[] {
    return Array.from({ length: count }, (_, i) =>
      this.create({ ...overrides, email: `test${i}@example.com` })
    )
  }
}
```

### Test Seeding

```typescript
// test/seed.ts
import { DataSource } from 'typeorm'
import { UserFactory } from './factories/user.factory'
import { RoleFactory } from './factories/role.factory'

export async function seedTestData(dataSource: DataSource) {
  const userRepository = dataSource.getRepository(User)
  const roleRepository = dataSource.getRepository(Role)

  // Seed roles
  const adminRole = RoleFactory.create({ name: 'ADMIN' })
  await roleRepository.save(adminRole)

  // Seed users
  const testUser = UserFactory.create({ email: 'test@example.com' })
  await userRepository.save(testUser)

  // Seed more test data as needed
}
```

---

## Coverage Configuration

### Coverage Directories

```
coverage/
├── lcov.info           # For Codecov
├── lcov-report/        # HTML report
└── coverage-final.json # JSON report
```

### Coverage Targets

| Layer | Target | Reason |
|-------|--------|--------|
| Unit Test | 80% | Critical business logic |
| Integration Test | 70% | API endpoints, database operations |
| E2E Test | 60% | Critical user journeys |

### Critical Path Coverage

Critical paths must have 90%+ coverage:
- Authentication flow
- Reservation creation
- Payment processing
- OTA synchronization
- Multi-tenant isolation

---

## CI Integration

### GitHub Actions Example

```yaml
# .github/workflows/test.yml
name: Test

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: test
          POSTGRES_DB: channelhub_test
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432

      redis:
        image: redis:7
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 6379:6379

    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run backend tests
        run: npm run test:backend
        env:
          DATABASE_URL: postgresql://postgres:test@localhost:5432/channelhub_test
          TEST_DATABASE_URL: postgresql://postgres:test@localhost:5432/channelhub_test
          REDIS_URL: redis://localhost:6379
      
      - name: Run frontend tests
        run: npm run test:frontend
      
      - name: Run E2E tests
        run: npm run test:e2e
        env:
          E2E_BASE_URL: http://localhost:3000
      
      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage/lcov.info
          flags: unittests
          name: codecov-umbrella
```

---

## Best Practices

1. **Test Isolation**: Setiap test harus independen dan tidak bergantung pada urutan
2. **Test Naming**: Gunakan descriptive nama untuk test (it('should do X when Y'))
3. **AAA Pattern**: Arrange, Act, Assert untuk struktur test
4. **Mock External Dependencies**: Mock OTA, payment gateway, external API
5. **Test Data**: Gunakan factory pattern untuk test data yang konsisten
6. **Coverage**: Fokus pada coverage critical path, bukan angka semata
7. **Performance**: Test harus cepat (<5s per unit test, <30s per integration test)
8. **Flaky Tests**: Identifikasi dan perbaiki test yang flaky
9. **Parallel Execution**: Konfigurasi test untuk parallel execution
10. **Test Documentation**: Document complex test scenario

END OF TEST AUTOMATION SETUP