# Deployment Production Checklist

## Purpose

Validasi deploy production ChannelHub.

## Infrastructure

- [ ] Docker topology sesuai docs/21-integration-deployment/002-docker-compose-production.md
- [ ] DB & Redis tidak exposed publik
- [ ] TLS / reverse proxy configured
- [ ] Healthchecks defined for all services

## Configuration

- [ ] Environment variables per docs/21-integration-deployment/004-environment-deployment.md
- [ ] Secrets dari secret manager, bukan git
- [ ] Feature flags / config validated

## Pipeline

- [ ] Security scan stage executed
- [ ] Image tagged & immutable
- [ ] Smoke test post-deploy

## References

- standards/deployment.md
- prompts/lifecycle/03-deployment-increment.md
- diagrams/005-cicd-lifecycle.md
