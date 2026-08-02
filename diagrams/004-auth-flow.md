# Authentication Flow Diagram

## Purpose

Alur login dan token selaras frontend-backend integration.

```mermaid
sequenceDiagram
  participant U as User Browser
  participant FE as Next.js
  participant API as NestJS Auth
  participant ID as Identity Store

  U->>FE: Login form
  FE->>API: POST /auth/login
  API->>ID: Validate user tenant role
  ID-->>API: OK
  API-->>FE: access + refresh token
  FE->>API: API call + Bearer + X-Tenant-Id
  API-->>FE: 401 expired
  FE->>API: POST /auth/refresh
  API-->>FE: new access token
  FE->>API: retry original request
```

## References

- docs/21-integration-deployment/001-frontend-backend-integration.md
- docs/16-api-contract/005-api-authentication-standard.md
- docs/22-security/002-authentication-hardening.md
