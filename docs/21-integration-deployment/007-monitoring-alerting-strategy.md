# ChannelHub Monitoring and Alerting Strategy

## Purpose

Menetapkan strategi monitoring dan alerting untuk observability system ChannelHub di production.

---

## AI TRIGGER

### Tujuan Task
Menetapkan monitoring dan alerting strategy dengan APM, log aggregation, dan alerting rules.

### Konteks yang Perlu Dipahami AI
- Monitoring: Metrics, logs, traces
- APM: Application Performance Monitoring (Datadog, New Relic)
- Log aggregation: ELK stack, CloudWatch Logs
- Alerting: Rules, escalation, notification
- Metrics: Response time, error rate, resource usage

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/02-product-architecture/006-observability-architecture.md (observability)
- docs/13-backend-foundation/008-backend-observability-standard.md (backend observability)
- .channelhub/OUTPUT-CONFIG.md (output directory structure)

### File/Folder yang Perlu Diperiksa
- docs/21-integration-deployment/005-infrastructure-as-code.md (IaC)

### Langkah Implementasi
1. Setup APM (Datadog/New Relic)
2. Setup log aggregation (ELK/CloudWatch)
3. Define alerting rules
4. Define alerting channels
5. Setup dashboards
6. Define escalation procedures

### Kriteria Keberhasilan (Definition of Done)
- APM terkonfigurasi dengan proper instrumentation
- Log aggregation terkonfigurasi dengan proper parsing
- Alerting rules terdefinisi dengan threshold
- Alerting channels terkonfigurasi
- Dashboards terkonfigurasi untuk key metrics
- Escalation procedures terdokumentasi

### Prompt Implementasi
```
Anda akan mengimplementasikan monitoring dan alerting untuk ChannelHub.

Baca docs/21-integration-deployment/007-monitoring-alerting-strategy.md untuk memahami strategy monitoring.

APM Setup (Datadog Example):
1. Install Datadog agent:
   - Install Datadog agent pada EKS nodes
   - Configure untuk Node.js, PostgreSQL, Redis
   - Enable distributed tracing

2. Application instrumentation:
   - Add Datadog APM to NestJS
   - Add custom metrics
   - Add distributed tracing
   - Add error tracking

3. Key metrics to monitor:
   - API response time (p50, p95, p99)
   - Error rate
   - Request rate
   - Database query time
   - Cache hit rate
   - Queue depth
   - Worker throughput

Log Aggregation (ELK Stack):
1. Elasticsearch setup:
   - Elasticsearch cluster untuk log storage
   - Index patterns untuk different services
   - Retention policy (30 days)

2. Logstash setup:
   - Log parsing dan transformation
   - Field extraction
   - Enrichment

3. Kibana setup:
   - Dashboards untuk log analysis
   - Log query dan visualization
   - Alert integration

Alternative: CloudWatch Logs:
- CloudWatch Logs Insights
- CloudWatch Logs Metrics
- CloudWatch Alarms

Alerting Rules:
1. Critical alerts (PagerDuty):
   - Application down (> 5 minutes)
   - Error rate > 5%
   - Database down
   - Redis down
   - Security incident detected

2. Warning alerts (Slack):
   - API response time p95 > 1s
   - Database query p95 > 200ms
   - Queue backlog > 100
   - CPU > 85%
   - Memory > 90%

3. Info alerts (Email):
   - Weekly performance report
   - Monthly capacity report
   - Backup completion
   - Scheduled maintenance reminder

Alerting Channels:
- PagerDuty: Critical alerts, on-call rotation
- Slack: Warning alerts, team communication
- Email: Info alerts, reports
- SMS: Critical alerts untuk on-call

Dashboard Setup:
1. System Overview Dashboard:
   - Application health
   - Database health
   - Cache health
   - Queue status
   - Resource usage

2. Performance Dashboard:
   - API response time
   - Database query time
   - Cache hit rate
   - External API latency

3. Business Dashboard:
   - Total reservations
   - Active users
   - OTA sync status
   - Payment success rate

4. Security Dashboard:
   - Failed login attempts
   - Security events
   - API abuse
   - Access logs

Escalation Procedures:
1. Define severity levels (SEV1-SEV4)
2. Define response time per severity
3. Define escalation path
4. Define communication channels
5. Define post-incident process

Pastikan monitoring dan alerting terkonfigurasi dengan proper instrumentation dan escalation.
```

---

---

## AI TRIGGER

### Tujuan Task
Menetapkan monitoring dan alerting strategy dengan APM, log aggregation, dan alerting rules.

### Konteks yang Perlu Dipahami AI
- Monitoring: Metrics, logs, traces
- APM: Application Performance Monitoring (Datadog, New Relic)
- Log aggregation: ELK stack, CloudWatch Logs
- Alerting: Rules, escalation, notification
- Metrics: Response time, error rate, resource usage

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/02-product-architecture/006-observability-architecture.md (observability)
- docs/13-backend-foundation/008-backend-observability-standard.md (backend observability)
- .channelhub/OUTPUT-CONFIG.md (output directory structure)

### File/Folder yang Perlu Diperiksa
- docs/21-integration-deployment/005-infrastructure-as-code.md (IaC)

### Langkah Implementasi
1. Setup APM (Datadog/New Relic)
2. Setup log aggregation (ELK/CloudWatch)
3. Define alerting rules
4. Define alerting channels
5. Setup dashboards
6. Define escalation procedures

### Kriteria Keberhasilan (Definition of Done)
- APM terkonfigurasi dengan proper instrumentation
- Log aggregation terkonfigurasi dengan proper parsing
- Alerting rules terdefinisi dengan threshold
- Alerting channels terkonfigurasi
- Dashboards terkonfigurasi untuk key metrics
- Escalation procedures terdokumentasi

### Prompt Implementasi
```
Anda akan mengimplementasikan monitoring dan alerting untuk ChannelHub.

Baca docs/21-integration-deployment/007-monitoring-alerting-strategy.md untuk memahami strategy monitoring.

APM Setup (Datadog Example):
1. Install Datadog agent:
   - Install Datadog agent pada EKS nodes
   - Configure untuk Node.js, PostgreSQL, Redis
   - Enable distributed tracing

2. Application instrumentation:
   - Add Datadog APM to NestJS
   - Add custom metrics
   - Add distributed tracing
   - Add error tracking

3. Key metrics to monitor:
   - API response time (p50, p95, p99)
   - Error rate
   - Request rate
   - Database query time
   - Cache hit rate
   - Queue depth
   - Worker throughput

Log Aggregation (ELK Stack):
1. Elasticsearch setup:
   - Elasticsearch cluster for log storage
   - Index patterns for different services
   - Retention policy (30 days)

2. Logstash setup:
   - Log parsing and transformation
   - Field extraction
   - Enrichment

3. Kibana setup:
   - Dashboards for log analysis
   - Log query and visualization
   - Alert integration

Alternative: CloudWatch Logs:
- CloudWatch Logs Insights
- CloudWatch Logs Metrics
- CloudWatch Alarms

Alerting Rules:
1. Critical alerts (PagerDuty):
   - Application down (> 5 minutes)
   - Error rate > 5%
   - Database down
   - Redis down
   - Security incident detected

2. Warning alerts (Slack):
   - API response time p95 > 1s
   - Database query p95 > 200ms
   - Queue backlog > 100
   - CPU > 85%
   - Memory > 90%

3. Info alerts (Email):
   - Weekly performance report
   - Monthly capacity report
   - Backup completion
   - Scheduled maintenance reminder

Alerting Channels:
- PagerDuty: Critical alerts, on-call rotation
- Slack: Warning alerts, team communication
- Email: Info alerts, reports
- SMS: Critical alerts for on-call

Dashboard Setup:
1. System Overview Dashboard:
   - Application health
   - Database health
   - Cache health
   - Queue status
   - Resource usage

2. Performance Dashboard:
   - API response time
   - Database query time
   - Cache hit rate
   - External API latency

3. Business Dashboard:
   - Total reservations
   - Active users
   - OTA sync status
   - Payment success rate

4. Security Dashboard:
   - Failed login attempts
   - Security events
   - API abuse
   - Access logs

Escalation Procedures:
1. Define severity levels (SEV1-SEV4)
2. Define response time per severity
3. Define escalation path
4. Define communication channels
5. Define post-incident process

Pastikan monitoring dan alerting terkonfigurasi dengan proper instrumentation dan escalation.
```

---

## APM Setup (Datadog)

### Agent Installation

```yaml
# datadog-agent-daemonset.yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: datadog-agent
  namespace: default
spec:
  selector:
    matchLabels:
      app: datadog-agent
  template:
    metadata:
      labels:
        app: datadog-agent
    spec:
      containers:
      - name: datadog-agent
        image: gcr.io/datadog/agent:latest
        env:
        - name: DD_API_KEY
          valueFrom:
            secretKeyRef:
              name: datadog-secret
              key: api-key
        - name: DD_SITE
          value: "ap-southeast-1.datadoghq.com"
        - name: DD_LOGS_ENABLED
          value: "true"
        - name: DD_PROCESS_AGENT_ENABLED
          value: "true"
        - name: DD_KUBELET_HOSTNAME
          valueFrom:
            fieldRef:
              fieldPath: spec.nodeName
        resources:
          requests:
            cpu: 200m
            memory: 256Mi
          limits:
            cpu: 200m
            memory: 256Mi
        volumeMounts:
        - name: proc
          mountPath: /host/proc
          readOnly: true
        - name: cgroup
          mountPath: /host/cgroup
          readOnly: true
        - name: sysfs
          mountPath: /host/sys/fs
          readOnly: true
      volumes:
      - name: proc
        hostPath:
          path: /proc
      - name: cgroup
        hostPath:
          path: /sys/fs/cgroup
      - name: sysfs
        hostPath:
          path: /sys/fs
```

### Application Instrumentation

```typescript
// src/common/monitoring/datadog.service.ts
import { Injectable, OnModuleInit } from '@nestjs/common';
import { Tracer, Span } from 'dd-trace';

@Injectable()
export class DatadogService implements OnModuleInit {
  private tracer: Tracer;

  constructor() {
    this.tracer = new Tracer({ serviceName: 'channelhub-backend' });
  }

  onModuleInit() {
    // Initialize Datadog APM
  }

  startSpan(operationName: string): Span {
    return this.tracer.startSpan(operationName);
  }

  recordMetric(name: string, value: number, tags: Record<string, string> = {}) {
    // Record custom metric
  }

  recordError(error: Error, context: Record<string, any> = {}) {
    // Record error with context
  }
}
```

### Distributed Tracing

```typescript
// src/common/interceptors/tracing.interceptor.ts
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable()
export class TracingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const span = datadogService.startSpan(context.getHandler().name);

    return next.handle().pipe(
      tap(() => {
        span.finish();
      }),
      tap((error) => {
        span.setTag('error', error.message);
        span.finish();
      })
    );
  }
}
```

---

## Log Aggregation (ELK Stack)

### Elasticsearch Configuration

```yaml
# elasticsearch.yml
cluster.name: channelhub-logs
node.name: ${HOSTNAME}
network.host: 0.0.0.0
http.port: 9200
discovery.type: single-node
path.data: /usr/share/elasticsearch/data
bootstrap.memory_lock: true
```

### Logstash Configuration

```conf
# logstash.conf
input {
  beats {
    port => 5044
  }
}

filter {
  grok {
    match => { "message" => "%{TIMESTAMP_ISO8601:timestamp} %{LOGLEVEL:level} %{GREEDYDATA:json}" }
  }
  
  if [level] == "ERROR" {
    mutate { add_tag => ["error"] }
  }
}

output {
  elasticsearch {
    hosts => ["elasticsearch:9200"]
    index => "channelhub-%{+YYYY.MM.dd}"
  }
}
```

### Kibana Dashboards

**Log Analysis Dashboard:**
- Error rate over time
- Top error messages
- Error by service
- Error by severity

**Performance Dashboard:**
- Request time distribution
- Slow query logs
- External API latency logs
- Queue processing logs

---

## CloudWatch Alternative

### CloudWatch Logs Insights

```sql
-- Example query for error rate
SELECT @timestamp, service, COUNT(*) as error_count
FROM cloudfront_logs
WHERE httpStatus >= 400
GROUP BY service, @timestamp
ORDER BY @timestamp DESC
LIMIT 100
```

### CloudWatch Metrics

```yaml
# cloudwatch-metrics.yml
metrics:
  - name: APIResponseTime
    namespace: ChannelHub/API
    statistic: p95
    period: 60
    evaluation_periods: 5
    threshold: 500
    comparison: greater-than-threshold
```

---

## Alerting Rules

### Critical Alerts (PagerDuty)

**Application Down:**
```yaml
name: Application Down
condition: avg(error_rate) > 0.05 for 5 minutes
severity: critical
channel: pagerduty
message: "Application error rate > 5% for 5 minutes"
```

**Database Down:**
```yaml
name: Database Down
condition: avg(database_up) == 0 for 2 minutes
severity: critical
channel: pagerduty
message: "Database is down"
```

**Redis Down:**
```yaml
name: Redis Down
condition: avg(redis_up) == 0 for 2 minutes
severity: critical
channel: pagerduty
message: "Redis is down"
```

**Security Incident:**
```yaml
name: Security Incident
condition: avg(failed_login_rate) > 0.1 for 5 minutes
severity: critical
channel: pagerduty
message: "High failed login rate detected"
```

### Warning Alerts (Slack)

**Slow API:**
```yaml
name: Slow API
condition: avg(api_response_time_p95) > 1000 for 10 minutes
severity: warning
channel: slack
message: "API p95 response time > 1s for 10 minutes"
```

**Slow Database:**
```yaml
name: Slow Database
condition: avg(db_query_time_p95) > 200 for 10 minutes
severity: warning
channel: slack
message: "Database p95 query time > 200ms for 10 minutes"
```

**Queue Backlog:**
```yaml
name: Queue Backlog
condition: avg(queue_depth) > 100 for 5 minutes
severity: warning
channel: slack
message: "Queue backlog > 100 for 5 minutes"
```

**High CPU:**
```yaml
name: High CPU
condition: avg(cpu_usage) > 85 for 15 minutes
severity: warning
channel: slack
message: "CPU usage > 85% for 15 minutes"
```

### Info Alerts (Email)

**Weekly Performance Report:**
```yaml
name: Weekly Performance Report
schedule: weekly
severity: info
channel: email
message: "Weekly performance report generated"
```

**Backup Completion:**
```yaml
name: Backup Completion
condition: backup_status == success
severity: info
channel: email
message: "Database backup completed successfully"
```

---

## Alerting Channels

### PagerDuty

**Configuration:**
- Service: ChannelHub
- Escalation policy: Primary → Secondary → Manager
- On-call schedule: Weekly rotation
- Notification methods: SMS, Phone, Email

**Integration:**
- Datadog → PagerDuty
- CloudWatch → SNS → PagerDuty
- Custom alerts → PagerDuty API

### Slack

**Channels:**
- #incidents: Real-time incident communication
- #alerts: Automated alerts
- #on-call: On-call communication
- #engineering: Engineering team

**Integration:**
- Datadog → Slack webhook
- CloudWatch → SNS → Slack
- Custom alerts → Slack API

### Email

**Recipients:**
- Engineering team
- On-call engineer
- Management (for SEV1)

**Integration:**
- SNS → Email
- CloudWatch → Email
- Custom alerts → Email

---

## Dashboard Setup

### System Overview Dashboard

**Metrics:**
- Application uptime
- Database uptime
- Redis uptime
- Queue worker status
- External API status

**Visualization:**
- Status indicators (green/yellow/red)
- Uptime percentage
- Incident count
- Active incidents

### Performance Dashboard

**Metrics:**
- API response time (p50, p95, p99)
- Database query time (p50, p95, p99)
- Cache hit rate
- External API latency
- Throughput (requests per second)

**Visualization:**
- Time series graphs
- Histograms
- Heatmaps
- Percentile graphs

### Business Dashboard

**Metrics:**
- Total reservations (hourly/daily/weekly)
- Active users
- OTA sync status
- Payment success rate
- Revenue

**Visualization:**
- Line charts for trends
- Bar charts for comparison
- Pie charts for distribution
- KPI cards for summary

### Security Dashboard

**Metrics:**
- Failed login attempts
- Security events
- API abuse attempts
- Access logs
- Vulnerability scan results

**Visualization:**
- Timeline of security events
- Geographic distribution of access
- Attack type breakdown
- Top sources of attacks

---

## Escalation Procedures

### Severity Definitions

**SEV1 - Critical:**
- Impact: System-wide outage
- Response: 15 minutes
- Escalation: Immediate to CTO
- Communication: All stakeholders

**SEV2 - High:**
- Impact: Major feature down
- Response: 30 minutes
- Escalation: 15 minutes to VP Engineering
- Communication: Internal + Support

**SEV3 - Medium:**
- Impact: Minor feature down
- Response: 1 hour
- Escalation: 30 minutes to Team Lead
- Communication: Internal only

**SEV4 - Low:**
- Impact: Noise, documentation
- Response: 24 hours
- Escalation: Create ticket
- Communication: None needed

### Escalation Matrix

| Severity | Response Time | Escalation Time | Escalation To |
|----------|---------------|-----------------|---------------|
| SEV1 | 15 minutes | Immediate | CTO, VP Engineering |
| SEV2 | 30 minutes | 15 minutes | Engineering Manager |
| SEV3 | 1 hour | 30 minutes | Team Lead |
| SEV4 | 24 hours | N/A | Create ticket |

---

## On-Call Setup

### PagerDuty Schedule

**Rotation:** Weekly  
**Primary:** On-call engineer  
**Secondary:** Backup engineer  

**Responsibilities:**
- Monitor alerts 24/7
- Respond to incidents within SLA
- Escalate if needed
- Document incidents

### On-Call Tools

**PagerDuty:**
- Alert management
- On-call scheduling
- Escalation policy
- Incident tracking

**Slack:**
- #on-call channel
- #incidents channel
- Direct messages

**Runbook Access:**
- All runbooks accessible
- Emergency contacts available
- Access credentials available

END OF MONITORING ALERTING STRATEGY