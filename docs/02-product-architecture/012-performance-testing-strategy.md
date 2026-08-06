# ChannelHub Performance Testing Strategy

## Purpose

Menetapkan strategi performance testing untuk memastikan sistem ChannelHub dapat menangani load yang diharapkan.

---

## AI TRIGGER

### Tujuan Task
Menetapkan strategi dan implementasi performance testing untuk backend dan frontend.

### Konteks yang Perlu Dipahami AI
- Performance target: API response p95 < 500ms, frontend FCP < 1.5s, TTI < 3.5s
- Load testing: Test capacity dan throughput
- Stress testing: Test breaking point
- Spike testing: Test sudden traffic increase
- Soak testing: Test sustainability over time

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/02-product-architecture/010-testing-strategy.md (testing strategy)
- docs/02-product-architecture/006-observability-architecture.md (observability)
- .channelhub/OUTPUT-CONFIG.md (output directory structure)

### File/Folder yang Perlu Diperiksa
- docs/13-backend-foundation/008-backend-observability-standard.md (backend observability)
- docs/14-frontend-foundation/008-frontend-performance-standard.md (frontend performance)

### Langkah Implementasi
1. Setup k6 untuk load testing
2. Setup Lighthouse untuk frontend performance
3. Define performance metrics dan target
4. Create performance test scenarios
5. Integrate performance test ke CI/CD
6. Setup performance monitoring

### Kriteria Keberhasilan (Definition of Done)
- Performance test framework terkonfigurasi
- Performance target terdefinisi
- Test scenarios tersedia untuk critical path
- Performance test terintegrasi ke CI/CD
- Performance monitoring setup

### Prompt Implementasi
```
Anda akan mengimplementasikan performance testing untuk ChannelHub.

Baca docs/02-product-architecture/012-performance-testing-strategy.md untuk memahami strategy performance testing.

Performance Targets:
- Backend API: p95 response time < 500ms untuk endpoint utama
- Frontend: FCP < 1.5s, TTI < 3.5s, CLS < 0.1
- Database: Query p95 < 100ms
- System: CPU < 70%, Memory < 80% pada normal load

Backend Performance Testing (k6):
1. Install k6:
   npm install -g k6

2. Load testing scenario:
   - Normal load: 100 RPS (requests per second)
   - Peak load: 500 RPS
   - Stress test: 1000+ RPS hingga breaking point
   - Spike test: Sudden increase dari 50 ke 500 RPS
   - Soak test: 100 RPS selama 1 jam

3. Critical path scenarios:
   - Authentication flow (login, refresh token)
   - Reservation creation (transaksional)
   - Inventory update (bulk operation)
   - OTA synchronization (external API)
   - Dashboard loading (aggregate query)

4. k6 script example:
   import http from 'k6/http';
   import { check, sleep } from 'k6';

   export let options = {
     stages: [
       { duration: '2m', target: 100 },  // Ramp up to 100 users
       { duration: '5m', target: 100 },  // Stay at 100 users
       { duration: '2m', target: 500 },  // Ramp up to 500 users
       { duration: '5m', target: 500 },  // Stay at 500 users
       { duration: '2m', target: 0 },    // Ramp down to 0
     ],
     thresholds: {
       http_req_duration: ['p(95)<500'],  // 95% of requests must complete below 500ms
       http_req_failed: ['rate<0.01'],    // Error rate must be less than 1%
     },
   };

   export default function () {
     let res = http.post('http://localhost:3000/api/v1/auth/login', {
       email: 'test@example.com',
       password: 'password123',
     });
     
     check(res, {
       'status is 200': (r) => r.status === 200,
       'response time < 500ms': (r) => r.timings.duration < 500,
     });
     
     sleep(1);
   }

Frontend Performance Testing (Lighthouse CI):
1. Install Lighthouse CI:
   npm install -g @lhci/cli

2. Lighthouse CI configuration:
   {
     "ci": {
       "collect": {
         "staticDistDir": "./out",
         "numberOfRuns": 3,
         "settings": {
           "preset": "desktop",
           "throttling": {
             "rttMs": 40,
             "throughputKbps": 10240,
             "cpuSlowdownMultiplier": 1
           }
         }
       },
       "assert": {
         "preset": "lighthouse:recommended",
         "assertions": {
           "categories:performance": ["error", { "minScore": 0.9 }],
           "categories:accessibility": ["warn", { "minScore": 0.9 }],
           "categories:best-practices": ["warn", { "minScore": 0.9 }],
           "categories:seo": ["warn", { "minScore": 0.9 }],
           "first-contentful-paint": ["error", { "maxNumericValue": 1500 }],
           "time-to-interactive": ["error", { "maxNumericValue": 3500 }],
           "cumulative-layout-shift": ["error", { "maxNumericValue": 0.1 }]
         }
       },
       "upload": {
         "target": "temporary-public-storage"
       }
     }
   }

3. Run Lighthouse CI:
   lhci autorun
   lhci collect --url=http://localhost:3000/dashboard
   lhci collect --url=http://localhost:3000/properties
   lhci collect --url=http://localhost:3000/reservations
   lhci assert

Database Performance Testing:
1. Query performance monitoring:
   - Log query execution time
   - Identify slow queries (>100ms)
   - Add index for slow queries
   - Optimize N+1 query problem

2. Load testing database:
   - Test dengan data volume yang besar (1M+ records)
   - Test query dengan complex join
   - Test transaction throughput
   - Monitor connection pool usage

Performance Monitoring:
1. APM integration (Datadog, New Relic, atau similar):
   - Track response time percentiles (p50, p95, p99)
   - Track error rate
   - Track throughput
   - Set up alerting for performance degradation

2. Database monitoring:
   - Query performance
   - Connection pool usage
   - Index efficiency
   - Lock contention

3. Frontend monitoring:
   - Real User Monitoring (RUM)
   - Core Web Vitals tracking
   - Error tracking

CI/CD Integration:
1. Add performance test to CI pipeline:
   - Run load test pada setiap PR
   - Run Lighthouse CI pada setiap PR
   - Block PR jika performance degradation > 10%
   - Generate performance report

2. Schedule periodic performance test:
   - Daily smoke test (normal load)
   - Weekly load test (peak load)
   - Monthly stress test (breaking point)

Performance Optimization Checklist:
- [ ] Database index optimization
- [ ] Query optimization (avoid N+1)
- [ ] Caching strategy (Redis)
- [ ] CDN for static assets
- [ ] Image optimization
- [ ] Code splitting
- [ ] Lazy loading
- [ ] Compression (gzip, brotli)
- [ ] HTTP/2 or HTTP/3
- [ ] Connection pooling
- [ ] Load balancing

Pastikan performance testing terkonfigurasi dan terintegrasi ke CI/CD.
```

---

---

## AI TRIGGER

### Tujuan Task
Menetapkan strategi dan implementasi performance testing untuk backend dan frontend.

### Konteks yang Perlu Dipahami AI
- Performance target: API response p95 < 500ms, frontend FCP < 1.5s, TTI < 3.5s
- Load testing: Test capacity dan throughput
- Stress testing: Test breaking point
- Spike testing: Test sudden traffic increase
- Soak testing: Test sustainability over time

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/02-product-architecture/010-testing-strategy.md (testing strategy)
- docs/02-product-architecture/006-observability-architecture.md (observability)
- .channelhub/OUTPUT-CONFIG.md (output directory structure)

### File/Folder yang Perlu Diperiksa
- docs/13-backend-foundation/008-backend-observability-standard.md (backend observability)
- docs/14-frontend-foundation/008-frontend-performance-standard.md (frontend performance)

### Langkah Implementasi
1. Setup k6 untuk load testing
2. Setup Lighthouse untuk frontend performance
3. Define performance metrics dan target
4. Create performance test scenarios
5. Integrate performance test ke CI/CD
6. Setup performance monitoring

### Kriteria Keberhasilan (Definition of Done)
- Performance test framework terkonfigurasi
- Performance target terdefinisi
- Test scenarios tersedia untuk critical path
- Performance test terintegrasi ke CI/CD
- Performance monitoring setup

### Prompt Implementasi
```
Anda akan mengimplementasikan performance testing untuk ChannelHub.

Baca docs/02-product-architecture/012-performance-testing-strategy.md untuk memahami strategy performance testing.

Performance Targets:
- Backend API: p95 response time < 500ms untuk endpoint utama
- Frontend: FCP < 1.5s, TTI < 3.5s, CLS < 0.1
- Database: Query p95 < 100ms
- System: CPU < 70%, Memory < 80% pada normal load

Backend Performance Testing (k6):
1. Install k6:
   npm install -g k6

2. Load testing scenario:
   - Normal load: 100 RPS (requests per second)
   - Peak load: 500 RPS
   - Stress test: 1000+ RPS hingga breaking point
   - Spike test: Sudden increase dari 50 ke 500 RPS
   - Soak test: 100 RPS selama 1 jam

3. Critical path scenarios:
   - Authentication flow (login, refresh token)
   - Reservation creation (transaksional)
   - Inventory update (bulk operation)
   - OTA synchronization (external API)
   - Dashboard loading (aggregate query)

4. k6 script example:
   import http from 'k6/http';
   import { check, sleep } from 'k6';

   export let options = {
     stages: [
       { duration: '2m', target: 100 },  // Ramp up to 100 users
       { duration: '5m', target: 100 },  // Stay at 100 users
       { duration: '2m', target: 500 },  // Ramp up to 500 users
       { duration: '5m', target: 500 },  // Stay at 500 users
       { duration: '2m', target: 0 },    // Ramp down to 0
     ],
     thresholds: {
       http_req_duration: ['p(95)<500'],  // 95% of requests must complete below 500ms
       http_req_failed: ['rate<0.01'],    // Error rate must be less than 1%
     },
   };

   export default function () {
     let res = http.post('http://localhost:3000/api/v1/auth/login', {
       email: 'test@example.com',
       password: 'password123',
     });
     
     check(res, {
       'status is 200': (r) => r.status === 200,
       'response time < 500ms': (r) => r.timings.duration < 500,
     });
     
     sleep(1);
   }

Frontend Performance Testing (Lighthouse CI):
1. Install Lighthouse CI:
   npm install -g @lhci/cli

2. Lighthouse CI configuration:
   {
     "ci": {
       "collect": {
         "staticDistDir": "./out",
         "numberOfRuns": 3,
         "settings": {
           "preset": "desktop",
           "throttling": {
             "rttMs": 40,
             "throughputKbps": 10240,
             "cpuSlowdownMultiplier": 1
           }
         }
       },
       "assert": {
         "preset": "lighthouse:recommended",
         "assertions": {
           "categories:performance": ["error", { "minScore": 0.9 }],
           "categories:accessibility": ["warn", { "minScore": 0.9 }],
           "categories:best-practices": ["warn", { "minScore": 0.9 }],
           "categories:seo": ["warn", { "minScore": 0.9 }],
           "first-contentful-paint": ["error", { "maxNumericValue": 1500 }],
           "time-to-interactive": ["error", { "maxNumericValue": 3500 }],
           "cumulative-layout-shift": ["error", { "maxNumericValue": 0.1 }]
         }
       },
       "upload": {
         "target": "temporary-public-storage"
       }
     }
   }

3. Run Lighthouse CI:
   lhci autorun
   lhci collect --url=http://localhost:3000/dashboard
   lhci assert

Database Performance Testing:
1. Query performance monitoring:
   - Log query execution time
   - Identify slow queries (>100ms)
   - Add index for slow queries
   - Optimize N+1 query problem

2. Load testing database:
   - Test dengan data volume yang besar (1M+ records)
   - Test query dengan complex join
   - Test transaction throughput
   - Monitor connection pool usage

Performance Monitoring:
1. APM integration (Datadog, New Relic, atau similar):
   - Track response time percentiles (p50, p95, p99)
   - Track error rate
   - Track throughput
   - Set up alerting for performance degradation

2. Database monitoring:
   - Query performance
   - Connection pool usage
   - Index efficiency
   - Lock contention

3. Frontend monitoring:
   - Real User Monitoring (RUM)
   - Core Web Vitals tracking
   - Error tracking

CI/CD Integration:
1. Add performance test to CI pipeline:
   - Run load test pada setiap PR
   - Run Lighthouse CI pada setiap PR
   - Block PR jika performance degradation > 10%
   - Generate performance report

2. Schedule periodic performance test:
   - Daily smoke test (normal load)
   - Weekly load test (peak load)
   - Monthly stress test (breaking point)

Performance Optimization Checklist:
- [ ] Database index optimization
- [ ] Query optimization (avoid N+1)
- [ ] Caching strategy (Redis)
- [ ] CDN for static assets
- [ ] Image optimization
- [ ] Code splitting
- [ ] Lazy loading
- [ ] Compression (gzip, brotli)
- [ ] HTTP/2 or HTTP/3
- [ ] Connection pooling
- [ ] Load balancing

Pastikan performance testing terkonfigurasi dan terintegrasi ke CI/CD.
```

---

## Performance Testing Types

### 1. Load Testing

**Purpose**: Menguji sistem pada load normal dan peak yang diharapkan.

**Scenario**:
- Normal load: 100 concurrent users, 100 RPS
- Peak load: 500 concurrent users, 500 RPS
- Duration: 10-30 minutes

**Tools**: k6, JMeter, Gatling

### 2. Stress Testing

**Purpose**: Menemukan breaking point sistem.

**Scenario**:
- Ramp up dari 100 ke 2000 RPS
- Identify where system fails
- Identify bottlenecks (database, API, queue)

**Tools**: k6, JMeter

### 3. Spike Testing

**Purpose**: Menguji sistem terhadap sudden traffic increase.

**Scenario**:
- Baseline: 50 RPS
- Spike: 500 RPS dalam 10 detik
- Sustain: 5 menit
- Recovery: Kembali ke baseline

**Tools**: k6

### 4. Soak Testing

**Purpose**: Menguji sustainability sistem over time.

**Scenario**:
- Load: 100 RPS
- Duration: 1-24 jam
- Monitor: Memory leak, connection leak, performance degradation

**Tools**: k6, JMeter

### 5. Frontend Performance Testing

**Purpose**: Menguji performance UI/UX.

**Metrics**:
- First Contentful Paint (FCP) < 1.5s
- Time to Interactive (TTI) < 3.5s
- Cumulative Layout Shift (CLS) < 0.1
- First Input Delay (FID) < 100ms
- Largest Contentful Paint (LCP) < 2.5s

**Tools**: Lighthouse, WebPageTest, Chrome DevTools

---

## Backend Performance Testing (k6)

### Installation

```bash
npm install -g k6
```

### Load Test Script Example

```javascript
// load-tests/reservation-creation.js
import http from 'k6/http';
import { check, sleep } from 'k6';

export let options = {
  stages: [
    { duration: '2m', target: 50 },   // Ramp up to 50 users
    { duration: '5m', target: 50 },   // Stay at 50 users
    { duration: '2m', target: 200 },  // Ramp up to 200 users
    { duration: '5m', target: 200 },  // Stay at 200 users
    { duration: '2m', target: 0 },    // Ramp down to 0
  ],
  thresholds: {
    http_req_duration: ['p(95)<500', 'p(99)<1000'],
    http_req_failed: ['rate<0.01'],
  },
};

const BASE_URL = __ENV.API_URL || 'http://localhost:3000';

export function setup() {
  // Login and get token
  const loginRes = http.post(`${BASE_URL}/api/v1/auth/login`, JSON.stringify({
    email: 'test@example.com',
    password: 'password123',
  }), {
    headers: { 'Content-Type': 'application/json' },
  });

  const token = loginRes.json('accessToken');
  return { token };
}

export default function (data) {
  const headers = {
    'Authorization': `Bearer ${data.token}`,
    'Content-Type': 'application/json',
    'X-Tenant-Id': 'test-tenant-id',
  };

  // Create reservation
  const payload = {
    propertyId: 'test-property-id',
    roomTypeId: 'test-room-type-id',
    checkIn: '2026-08-10',
    checkOut: '2026-08-12',
    guest: {
      name: 'Test Guest',
      email: 'guest@example.com',
      phone: '+628123456789',
    },
  };

  const res = http.post(`${BASE_URL}/api/v1/reservations`, JSON.stringify(payload), {
    headers,
  });

  check(res, {
    'status is 201': (r) => r.status === 201,
    'response time < 500ms': (r) => r.timings.duration < 500,
    'has reservation ID': (r) => r.json('id') !== undefined,
  });

  sleep(1);
}
```

### Stress Test Script Example

```javascript
// load-tests/stress-test.js
import http from 'k6/http';
import { check, sleep } from 'k6';

export let options = {
  stages: [
    { duration: '2m', target: 100 },
    { duration: '2m', target: 500 },
    { duration: '2m', target: 1000 },
    { duration: '2m', target: 2000 },
    { duration: '2m', target: 0 },
  ],
  thresholds: {
    http_req_duration: ['p(95)<1000'], // More lenient for stress test
    http_req_failed: ['rate<0.05'],   // Allow 5% error rate
  },
};

export default function () {
  const res = http.get('http://localhost:3000/api/v1/properties');
  check(res, { 'status is 200': (r) => r.status === 200 });
  sleep(1);
}
```

### Running Tests

```bash
# Load test
k6 run load-tests/reservation-creation.js

# Stress test
k6 run load-tests/stress-test.js

# With environment variables
API_URL=https://staging-api.channelhub.com k6 run load-tests/reservation-creation.js

# Generate HTML report
k6 run --out json=report.json load-tests/reservation-creation.js
```

---

## Frontend Performance Testing (Lighthouse)

### Lighthouse CI Setup

```bash
npm install -g @lhci/cli
```

### Lighthouse CI Configuration

```json
// lighthouserc.json
{
  "ci": {
    "collect": {
      "staticDistDir": "./out",
      "numberOfRuns": 3,
      "settings": {
        "preset": "desktop",
        "throttling": {
          "rttMs": 40,
          "throughputKbps": 10240,
          "cpuSlowdownMultiplier": 1
        }
      }
    },
    "assert": {
      "preset": "lighthouse:recommended",
      "assertions": {
        "categories:performance": ["error", { "minScore": 0.9 }],
        "categories:accessibility": ["warn", { "minScore": 0.9 }],
        "categories:best-practices": ["warn", { "minScore": 0.9 }],
        "categories:seo": ["warn", { "minScore": 0.9 }],
        "first-contentful-paint": ["error", { "maxNumericValue": 1500 }],
        "time-to-interactive": ["error", { "maxNumericValue": 3500 }],
        "cumulative-layout-shift": ["error", { "maxNumericValue": 0.1 }],
        "largest-contentful-paint": ["error", { "maxNumericValue": 2500 }]
      }
    },
    "upload": {
      "target": "temporary-public-storage"
    }
  }
}
```

### Running Lighthouse CI

```bash
# Collect and assert
lhci autorun

# Collect specific URL
lhci collect --url=http://localhost:3000/dashboard
lhci collect --url=http://localhost:3000/properties
lhci collect --url=http://localhost:3000/reservations

# Assert against thresholds
lhci assert

# Upload report
lhci upload
```

### Lighthouse CI in GitHub Actions

```yaml
- name: Run Lighthouse CI
  run: |
    npm run build
    lhci autorun
  env:
    LHCI_GITHUB_APP_TOKEN: ${{ secrets.LHCI_GITHUB_APP_TOKEN }}
```

---

## Database Performance Testing

### Query Performance Monitoring

```typescript
// src/common/interceptors/query-logging.interceptor.ts
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable()
export class QueryLoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const now = Date.now();
    return next.handle().pipe(
      tap(() => {
        const duration = Date.now() - now;
        if (duration > 100) {
          console.warn(`Slow query detected: ${duration}ms`);
        }
      }),
    );
  }
}
```

### Load Testing Database

```javascript
// load-tests/database-load-test.js
import { Pool } from 'pg';

const pool = new Pool({
  connectionString: process.env.TEST_DATABASE_URL,
});

export default function () {
  // Simulate concurrent database operations
  const promises = Array.from({ length: 10 }, () =>
    pool.query('SELECT * FROM reservations WHERE status = $1', ['CONFIRMED'])
  );

  Promise.all(promises)
    .then(() => console.log('Batch query completed'))
    .catch(err => console.error('Query failed:', err));
}
```

---

## Performance Monitoring

### APM Integration (Datadog Example)

```typescript
// datadog.config.ts
import { datadogLogs } from '@datadog/browser-logs';

datadogLogs.init({
  clientToken: process.env.DATADOG_CLIENT_TOKEN,
  site: 'datadoghq.com',
  forwardErrorsToLogs: true,
  sessionSampleRate: 100,
  service: 'channelhub-frontend',
  env: process.env.NODE_ENV,
});
```

### Custom Metrics

```typescript
// src/common/metrics/custom-metrics.service.ts
import { Injectable } from '@nestjs/common';
import { Counter, Histogram, Registry } from 'prom-client';

@Injectable()
export class CustomMetricsService {
  private httpRequestDuration: Histogram;
  private httpRequestTotal: Counter;

  constructor(private registry: Registry) {
    this.httpRequestDuration = new Histogram({
      name: 'http_request_duration_seconds',
      help: 'Duration of HTTP requests in seconds',
      labelNames: ['method', 'route', 'status_code'],
      registers: [this.registry],
    });

    this.httpRequestTotal = new Counter({
      name: 'http_requests_total',
      help: 'Total number of HTTP requests',
      labelNames: ['method', 'route', 'status_code'],
      registers: [this.registry],
    });
  }

  recordHttpRequest(method: string, route: string, statusCode: number, duration: number) {
    this.httpRequestDuration.labels(method, route, statusCode).observe(duration);
    this.httpRequestTotal.labels(method, route, statusCode).inc();
  }
}
```

---

## CI/CD Integration

### GitHub Actions Performance Test

```yaml
# .github/workflows/performance.yml
name: Performance Test

on:
  pull_request:
  schedule:
    - cron: '0 2 * * *' # Daily at 2 AM

jobs:
  load-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup k6
        run: |
          curl https://github.com/grafana/k6/releases/download/v0.47.0/k6-v0.47.0-linux-amd64.tar.gz -L | tar xvz
          sudo mv k6-v0.47.0-linux-amd64/k6 /usr/local/bin/
      
      - name: Run load test
        run: k6 run load-tests/reservation-creation.js
        env:
          API_URL: ${{ secrets.STAGING_API_URL }}
      
      - name: Upload report
        uses: actions/upload-artifact@v3
        with:
          name: load-test-report
          path: report.json

  lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build application
        run: npm run build
      
      - name: Run Lighthouse CI
        run: npx lhci autorun
        env:
          LHCI_GITHUB_APP_TOKEN: ${{ secrets.LHCI_GITHUB_APP_TOKEN }}
```

---

## Performance Optimization Checklist

### Backend Optimization

- [ ] Database index optimization
- [ ] Query optimization (avoid N+1)
- [ ] Connection pooling configuration
- [ ] Caching strategy (Redis)
- [ ] Response compression (gzip, brotli)
- [ ] Pagination for large datasets
- [ ] Batch operations for bulk updates
- [ ] Async processing for long-running tasks
- [ ] CDN for static assets
- [ ] Load balancing

### Frontend Optimization

- [ ] Code splitting (dynamic import)
- [ ] Lazy loading (components, images)
- [ ] Image optimization (Next.js Image)
- [ ] Bundle size optimization
- [ ] Tree shaking
- [ ] Minification
- [ ] Compression (gzip, brotli)
- [ ] HTTP/2 or HTTP/3
- [ ] Service worker for caching
- [ ] Critical CSS inline

### Database Optimization

- [ ] Proper indexing strategy
- [ ] Query optimization
- [ ] Connection pooling
- [ ] Read replicas for read-heavy workloads
- [ ] Partitioning for large tables
- [ ] Archival strategy for old data
- [ ] Regular vacuum and analyze

---

## Performance Regression Detection

### Baseline Establishment

```typescript
// Establish performance baseline
const baseline = {
  api_response_p95: 450, // ms
  api_response_p99: 800, // ms
  fcp: 1200, // ms
  tti: 3000, // ms
  cls: 0.08,
};

// Alert if regression > 10%
function checkRegression(current, baseline, threshold = 0.1) {
  const regression = (current - baseline) / baseline;
  if (regression > threshold) {
    alert(`Performance regression detected: ${regression * 100}%`);
  }
}
```

### Automated Performance Regression Test

```yaml
# Block PR if performance regression > 10%
- name: Check performance regression
  run: |
    # Compare current performance with baseline
    if [[ $(node scripts/check-performance-regression.js) -gt 10 ]]; then
      echo "Performance regression detected. Blocking PR."
      exit 1
    fi
```

END OF PERFORMANCE TESTING STRATEGY