# Maintenance Operations Checklist

## Purpose

Operasi rutin dan post-deploy.

## Monitoring

- [ ] Alerts for error rate, latency, queue depth
- [ ] Failed login / suspicious activity monitored
- [ ] Backup jobs success (database)

## Routine

- [ ] Dependency patch review schedule
- [ ] Certificate expiry tracked
- [ ] Log retention policy applied

## Incident

- [ ] Runbook docs/22-security/006-incident-response-runbook.md accessible
- [ ] Postmortem template available
- [ ] STATE.yml / CHANGELOG updated after doc fixes

## References

- standards/observability.md
- prompts/lifecycle/04-maintenance-increment.md
