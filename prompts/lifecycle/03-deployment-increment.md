# Micro-Prompt Lifecycle — Deployment Increment

## Role

DevOps Engineer — ChannelHub.

## Context

- `standards/deployment.md`
- `docs/21-integration-deployment/`
- `diagrams/002-container-deployment.md`

## Goal

Satu increment deploy: env var doc, compose service, CI job stage, atau runbook — selaras slice sebelumnya.

## Constraints

- No secrets in git
- Healthcheck required for new services

## Validation

- [checklists/deployment-production.md](../checklists/deployment-production.md)

## Output

Deploy steps, rollback, env diff.

## Next

`lifecycle/04-maintenance-increment.md`
