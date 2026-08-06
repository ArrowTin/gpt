# ChannelHub Test Coverage Implementation

## Purpose

Menetapkan implementasi test coverage target dan reporting untuk seluruh stack ChannelHub.

---

## AI TRIGGER

### Tujuan Task
Menetapkan implementasi test coverage dengan target dan reporting yang terukur.

### Konteks yang Perlu Dipahami AI
- Coverage target: Unit >80%, Integration >70%, E2E >60%
- Critical path coverage: 90%+
- Coverage reporting: HTML, LCov, JSON
- Coverage enforcement di CI
- Coverage trend tracking

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/02-product-architecture/010-testing-strategy.md (testing strategy)
- docs/02-product-architecture/011-test-automation-setup.md (test automation)
- .channelhub/OUTPUT-CONFIG.md (output directory structure)

### File/Folder yang Perlu Diperiksa
- docs/18-backend-implementation/004-service-testing-strategy.md (backend testing)

### Langkah Implementasi
1. Setup coverage reporting (Istanbul/nyc)
2. Define coverage targets per layer
3. Define critical path coverage
4. Setup coverage enforcement di CI
5. Setup coverage trend tracking
6. Define coverage reporting dashboard

### Kriteria Keberhasilan (Definition of Done)
- Coverage reporting terkonfigurasi
- Coverage target terdefinisi per layer
- Critical path coverage terdefinisi
- Coverage enforcement di CI
- Coverage trend tracking setup

### Prompt Implementasi
```
Anda akan mengimplementasikan test coverage untuk ChannelHub.

Baca docs/02-product-architecture/013-test-coverage-implementation.md untuk memahami coverage implementation.

Coverage Targets Per Layer:
- Unit Test: >80% (branches, functions, lines, statements)
- Integration Test: >70%
- E2E Test: >60%
- Critical Path: >90%

Critical Path Coverage (90%+):
- Authentication flow
- Reservation creation
- Payment processing
- OTA synchronization
- Multi-tenant isolation

Coverage Reporting Setup:
1. Install coverage tools:
   npm install --save-dev nyc istanbul

2. Configure coverage (nyc.config.js):
   {
     reporter: ['html', 'lcov', 'json', 'text-summary'],
     reporterOptions: {
       html: { outputDir: './coverage/html' },
       lcov: { outputDir: './coverage/lcov' },
       json: { outputDir: './coverage/json' },
     },
     exclude: [
       '**/node_modules/**',
       '**/test/**',
       '**/dist/**',
       '**/src/main.ts',
       '**/*.module.ts',
       '**/*.dto.ts',
       '**/*.interface.ts',
       '**/*.constant.ts',
       '**/*.config.ts',
     ],
     checkCoverage: true,
     coverageThreshold: {
       global: {
         branches: 80,
         functions: 80,
         lines: 80,
         statements: 80,
       },
     },
   }

3. Update package.json scripts:
   {
     "test": "nyc jest",
     "test:cov": "nyc jest --coverage",
     "test:cov:html": "nyc jest --coverage --coverage-reporter=html",
     "test:watch": "jest --watch",
   }

Coverage Enforcement di CI:
1. Add coverage check to CI pipeline:
   - Run test with coverage
   - Enforce coverage threshold
   - Fail if coverage below target
   - Generate coverage report

2. GitHub Actions example:
   - name: Run tests with coverage
     run: npm run test:cov
   
   - name: Enforce coverage threshold
     run: nyc check-coverage --lines 80 --functions 80 --branches 80 --statements 80
   
   - name: Upload coverage to Codecov
     uses: codecov/codecov-action@v3
     with:
       files: ./coverage/lcov/lcov.info
       flags: unittests
       name: codecov-umbrella

Critical Path Coverage Definition:
1. Identify critical paths:
   - Authentication: src/modules/auth/
   - Reservation: src/modules/reservations/
   - Payment: src/modules/billing/
   - OTA Sync: src/modules/channels/
   - Multi-tenant: src/common/guards/

2. Set higher threshold for critical paths:
   {
     checkCoverage: true,
     coverageThreshold: {
       global: {
         branches: 80,
         functions: 80,
         lines: 80,
         statements: 80,
       },
       './src/modules/auth/': {
         branches: 90,
         functions: 90,
         lines: 90,
         statements: 90,
       },
       './src/modules/reservations/': {
         branches: 90,
         functions: 90,
         lines: 90,
         statements: 90,
       },
       './src/modules/billing/': {
         branches: 90,
         functions: 90,
         lines: 90,
         statements: 90,
       },
       './src/modules/channels/': {
         branches: 90,
         functions: 90,
         lines: 90,
         statements: 90,
       },
       './src/common/guards/': {
         branches: 90,
         functions: 90,
         lines: 90,
         statements: 90,
       },
     },
   }

Coverage Trend Tracking:
1. Setup Codecov or Coveralls:
   - Upload coverage reports
   - Track coverage over time
   - Set coverage targets
   - Send PR comments with coverage changes

2. Coverage dashboard:
   - Configure Codecov dashboard
   - Show coverage trend
   - Show coverage by module
   - Show coverage by team

Coverage Best Practices:
1. Focus on meaningful coverage:
   - Test business logic, not getters/setters
   - Test edge cases, not happy path only
   - Test error handling, not success only

2. Maintain coverage:
   - Add test when adding new code
   - Update test when changing code
   - Remove unused code instead of excluding from coverage

3. Review coverage regularly:
   - Weekly coverage review
   - Identify declining coverage
   - Plan improvement

Coverage Anti-Patterns:
- Don't exclude code without good reason
- Don't sacrifice quality for coverage
- Don't write tests just for coverage
- Don't ignore coverage warnings

Pastikan coverage implementation terkonfigurasi dengan proper target dan reporting.
```

---

---

## AI TRIGGER

### Tujuan Task
Menetapkan implementasi test coverage dengan target dan reporting yang terukur.

### Konteks yang Perlu Dipahami AI
- Coverage target: Unit >80%, Integration >70%, E2E >60%
- Critical path coverage: 90%+
- Coverage reporting: HTML, LCov, JSON
- Coverage enforcement di CI
- Coverage trend tracking

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/02-product-architecture/010-testing-strategy.md (testing strategy)
- docs/02-product-architecture/011-test-automation-setup.md (test automation)
- .channelhub/OUTPUT-CONFIG.md (output directory structure)

### File/Folder yang Perlu Diperiksa
- docs/18-backend-implementation/004-service-testing-strategy.md (backend testing)

### Langkah Implementasi
1. Setup coverage reporting (Istanbul/nyc)
2. Define coverage targets per layer
3. Define critical path coverage
4. Setup coverage enforcement di CI
5. Setup coverage trend tracking
6. Define coverage reporting dashboard

### Kriteria Keberhasilan (Definition of Done)
- Coverage reporting terkonfigurasi
- Coverage target terdefinisi per layer
- Critical path coverage terdefinisi
- Coverage enforcement di CI
- Coverage trend tracking setup

### Prompt Implementasi
```
Anda akan mengimplementasikan test coverage untuk ChannelHub.

Baca docs/02-product-architecture/013-test-coverage-implementation.md untuk memahami coverage implementation.

Coverage Targets Per Layer:
- Unit Test: >80% (branches, functions, lines, statements)
- Integration Test: >70%
- E2E Test: >60%
- Critical Path: >90%

Critical Path Coverage (90%+):
- Authentication flow
- Reservation creation
- Payment processing
- OTA synchronization
- Multi-tenant isolation

Coverage Reporting Setup:
1. Install coverage tools:
   npm install --save-dev nyc istanbul

2. Configure coverage (nyc.config.js):
   {
     reporter: ['html', 'lcov', 'json', 'text-summary'],
     reporterOptions: {
       html: { outputDir: './coverage/html' },
       lcov: { outputDir: './coverage/lcov' },
       json: { outputDir: './coverage/json' },
     },
     exclude: [
       '**/node_modules/**',
       '**/test/**',
       '**/dist/**',
       '**/src/main.ts',
       '**/*.module.ts',
       '**/*.dto.ts',
       '**/*.interface.ts',
       '**/*.constant.ts',
       '**/*.config.ts',
     ],
     checkCoverage: true,
     coverageThreshold: {
       global: {
         branches: 80,
         functions: 80,
         lines: 80,
         statements: 80,
       },
     },
   }

3. Update package.json scripts:
   {
     "test": "nyc jest",
     "test:cov": "nyc jest --coverage",
     "test:cov:html": "nyc jest --coverage --coverage-reporter=html",
     "test:watch": "jest --watch",
   }

Coverage Enforcement di CI:
1. Add coverage check to CI pipeline:
   - Run test with coverage
   - Enforce coverage threshold
   - Fail if coverage below target
   - Generate coverage report

2. GitHub Actions example:
   - name: Run tests with coverage
     run: npm run test:cov
   
   - name: Enforce coverage threshold
     run: nyc check-coverage --lines 80 --functions 80 --branches 80 --statements 80
   
   - name: Upload coverage to Codecov
     uses: codecov/codecov-action@v3
     with:
       files: ./coverage/lcov/lcov.info
       flags: unittests
       name: codecov-umbrella

Critical Path Coverage Definition:
1. Identify critical paths:
   - Authentication: src/modules/auth/
   - Reservation: src/modules/reservations/
   - Payment: src/modules/billing/
   - OTA Sync: src/modules/channels/
   - Multi-tenant: src/common/guards/

2. Set higher threshold for critical paths:
   {
     checkCoverage: true,
     coverageThreshold: {
       global: {
         branches: 80,
         functions: 80,
         lines: 80,
         statements: 80,
       },
       './src/modules/auth/': {
         branches: 90,
         functions: 90,
         lines: 90,
         statements: 90,
       },
       './src/modules/reservations/': {
         branches: 90,
         functions: 90,
         lines: 90,
         statements: 90,
       },
       './src/modules/billing/': {
         branches: 90,
         functions: 90,
         lines: 90,
         statements: 90,
       },
       './src/modules/channels/': {
         branches: 90,
         functions: 90,
         lines: 90,
         statements: 90,
       },
       './src/common/guards/': {
         branches: 90,
         functions: 90,
         lines: 90,
         statements: 90,
       },
     },
   }

Coverage Trend Tracking:
1. Setup Codecov or Coveralls:
   - Upload coverage reports
   - Track coverage over time
   - Set coverage targets
   - Send PR comments with coverage changes

2. Coverage dashboard:
   - Configure Codecov dashboard
   - Show coverage trend
   - Show coverage by module
   - Show coverage by team

Coverage Best Practices:
1. Focus on meaningful coverage:
   - Test business logic, not getters/setters
   - Test edge cases, not happy path only
   - Test error handling, not success only

2. Maintain coverage:
   - Add test when adding new code
   - Update test when changing code
   - Remove unused code instead of excluding from coverage

3. Review coverage regularly:
   - Weekly coverage review
   - Identify declining coverage
   - Plan improvement

Coverage Anti-Patterns:
- Don't exclude code without good reason
- Don't sacrifice quality for coverage
- Don't write tests just for coverage
- Don't ignore coverage warnings

Pastikan coverage implementation terkonfigurasi dengan proper target dan reporting.
```

---

## Coverage Targets

### Overall Targets

| Layer | Target | Rationale |
|-------|--------|-----------|
| Unit Test | 80% | Critical business logic must be tested |
| Integration Test | 70% | API endpoints and database operations |
| E2E Test | 60% | Critical user journeys |
| Combined | 75% | Overall target |

### Critical Path Targets

| Component | Target | Rationale |
|-----------|--------|-----------|
| Authentication | 90% | Security critical |
| Reservation | 90% | Core business logic |
| Payment | 90% | Financial critical |
| OTA Sync | 90% | Integration critical |
| Multi-tenant Guards | 90% | Security critical |

### Per-Module Targets

| Module | Unit | Integration | E2E |
|--------|------|-------------|-----|
| Auth | 90% | 80% | 70% |
| Users | 85% | 75% | 60% |
| Organizations | 85% | 75% | 60% |
| Properties | 80% | 70% | 60% |
| Reservations | 90% | 80% | 70% |
| Channels | 85% | 75% | 60% |
| Billing | 90% | 80% | 70% |
| Notifications | 80% | 70% | 60% |
| Platform | 75% | 65% | 50% |

---

## Coverage Configuration

### Backend Coverage (nyc)

```javascript
// nyc.config.js
module.exports = {
  reporter: ['html', 'lcov', 'json', 'text-summary'],
  reporterOptions: {
    html: { outputDir: './coverage/html' },
    lcov: { outputDir: './coverage/lcov' },
    json: { outputDir: './coverage/json' },
  },
  exclude: [
    '**/node_modules/**',
    '**/test/**',
    '**/dist/**',
    '**/build/**',
    '**/.next/**',
    '**/src/main.ts',
    '**/*.module.ts',
    '**/*.dto.ts',
    '**/*.interface.ts',
    '**/*.constant.ts',
    '**/*.config.ts',
    '**/*.types.ts',
    '**/*.entity.ts', // TypeORM entities often auto-generated
  ],
  checkCoverage: true,
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
    './src/modules/auth/': {
      branches: 90,
      functions: 90,
      lines: 90,
      statements: 90,
    },
    './src/modules/reservations/': {
      branches: 90,
      functions: 90,
      lines: 90,
      statements: 90,
    },
    './src/modules/billing/': {
      branches: 90,
      functions: 90,
      lines: 90,
      statements: 90,
    },
    './src/modules/channels/': {
      branches: 90,
      functions: 90,
      lines: 90,
      statements: 90,
    },
    './src/common/guards/': {
      branches: 90,
      functions: 90,
      lines: 90,
      statements: 90,
    },
  },
};
```

### Frontend Coverage (Jest)

```javascript
// jest.config.js
module.exports = {
  collectCoverageFrom: [
    'src/**/*.{js,jsx,ts,tsx}',
    '!src/**/*.d.ts',
    '!src/**/*.stories.{js,jsx,ts,tsx}',
    '!src/**/__tests__/**',
    '!src/**/*.config.{js,ts}',
    '!src/types/**',
  ],
  coverageThreshold: {
    global: {
      branches: 70,
      functions: 70,
      lines: 70,
      statements: 70,
    },
    './src/features/auth/**': {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
    './src/features/reservations/**': {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
};
```

---

## Coverage Reporting

### HTML Report

```bash
# Generate HTML coverage report
npm run test:cov:html

# Open report
open coverage/html/index.html
```

### LCov Report

```bash
# Generate LCov report
npm run test:cov

# Upload to Codecov
codecov upload coverage/lcov/lcov.info
```

### JSON Report

```bash
# Generate JSON report for custom analysis
npm run test:cov

# Parse JSON report
node scripts/analyze-coverage.js
```

---

## Coverage Enforcement

### Pre-Commit Hook

```javascript
// .husky/pre-commit
#!/bin/sh
npm run test:cov
if [ $? -ne 0 ]; then
  echo "Coverage threshold not met"
  exit 1
fi
```

### CI Enforcement

```yaml
# .github/workflows/test.yml
- name: Run tests with coverage
  run: npm run test:cov

- name: Check coverage threshold
  run: |
    node scripts/check-coverage.js

- name: Upload coverage to Codecov
  uses: codecov/codecov-action@v3
  with:
    files: ./coverage/lcov/lcov.info
    flags: unittests
    fail_ci_if_error: true
```

### Coverage Check Script

```javascript
// scripts/check-coverage.js
const coverage = require('./coverage/coverage-final.json');

const thresholds = {
  global: {
    lines: 80,
    functions: 80,
    branches: 80,
    statements: 80,
  },
};

function checkCoverage(coverage, thresholds) {
  for (const [key, value] of Object.entries(thresholds)) {
    if (coverage[key].pct < value) {
      console.error(`Coverage ${key} (${coverage[key].pct}%) below threshold (${value}%)`);
      process.exit(1);
    }
  }
  console.log('Coverage threshold met');
}

checkCoverage(coverage.total, thresholds.global);
```

---

## Coverage Trend Tracking

### Codecov Configuration

```yaml
# codecov.yml
coverage:
  status:
    project:
      default:
        target: 80%
        threshold: 1%
        informational: true
    patch:
      default:
        target: 80%
        threshold: 1%
        informational: true

comment:
  layout: "reach,diff,flags,files,footer"
  behavior: default
  require_changes: false
  require_base: false
  branches: [main, develop]

ignore:
  - "**/*.test.ts"
  - "**/*.spec.ts"
  - "**/*.e2e-spec.ts"
  - "**/test/**"
```

### Coverage Dashboard

**Codecov Dashboard:**
- Project overview
- Coverage trend
- Coverage by file
- Coverage by commit
- Pull request coverage changes

**Custom Dashboard:**
- Grafana with coverage metrics
- Coverage trend over time
- Coverage by module
- Coverage by team

---

## Coverage Best Practices

### 1. Write Meaningful Tests

**Do:**
- Test business logic
- Test edge cases
- Test error handling
- Test integration points

**Don't:**
- Test getters/setters
- Test trivial functions
- Test only happy path
- Exclude code without reason

### 2. Maintain Coverage

**When Adding New Code:**
- Write test for new code
- Aim for high coverage on new code
- Update coverage documentation

**When Changing Code:**
- Update or add tests
- Ensure coverage doesn't drop
- Address coverage warnings

**When Removing Code:**
- Remove corresponding tests
- Update coverage thresholds if needed

### 3. Review Coverage Regularly

**Weekly Review:**
- Check coverage trend
- Identify declining coverage
- Plan improvement actions

**Monthly Review:**
- Review coverage by module
- Identify low-coverage modules
- Plan coverage improvement

**Quarterly Review:**
- Review coverage targets
- Adjust targets if needed
- Update coverage strategy

---

## Coverage Anti-Patterns

### Anti-Pattern 1: Excluding Code Without Reason

```javascript
// BAD
exclude: [
  '**/src/modules/**', // Exclude entire modules
]

// GOOD
exclude: [
  '**/src/types/**', // Type definitions
  '**/src/config/**', // Configuration
]
```

### Anti-Pattern 2: Writing Tests Just for Coverage

```typescript
// BAD - Trivial test
describe('utils', () => {
  it('should add numbers', () => {
    expect(add(1, 2)).toBe(3);
  });
});

// GOOD - Meaningful test
describe('utils', () => {
  it('should add numbers with proper validation', () => {
    expect(add(1, 2)).toBe(3);
    expect(add(null, 2)).toThrow();
    expect(add(1, null)).toThrow();
  });
});
```

### Anti-Pattern 3: Ignoring Coverage Warnings

```bash
# BAD - Ignore warnings
npm run test:cov --silent

# GOOD - Address warnings
npm run test:cov
# Investigate and fix coverage gaps
```

---

## Coverage Improvement Strategy

### Identify Low Coverage Areas

```bash
# Generate coverage report
npm run test:cov

# Analyze low-coverage files
node scripts/analyze-low-coverage.js
```

### Prioritize Improvement

**Priority 1 - Critical Path:**
- Authentication
- Reservation
- Payment
- OTA Sync

**Priority 2 - Core Business Logic:**
- User management
- Property management
- Channel management

**Priority 3 - Supporting Code:**
- Utilities
- Helpers
- Configuration

### Incremental Improvement

1. Start with critical path
2. Aim for 5-10% improvement per sprint
3. Track progress in coverage dashboard
4. Celebrate milestones

END OF TEST COVERAGE IMPLEMENTATION