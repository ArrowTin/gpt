# CI/CD Lifecycle Diagram

## Purpose

Pipeline quality gate dari commit hingga deploy.

```mermaid
flowchart LR
  Commit[Git Push] --> CI[CI Workflow]
  CI --> Lint[Lint / Typecheck]
  Lint --> Test[Unit Integration Tests]
  Test --> Sec[Security Scan]
  Sec --> Build[Docker Build]
  Build --> Stage[Deploy Staging]
  Stage --> Smoke[Smoke Tests]
  Smoke --> Prod[Deploy Production]
  Prod --> Mon[Monitoring Alerts]
```

## References

- docs/21-integration-deployment/003-ci-cd-pipeline.md
- checklists/deployment-production.md
- checklists/testing-release.md
