# Micro-Prompt Lifecycle — Testing Increment

## Role

QA / Platform Engineer — ChannelHub.

## Context

- `standards/testing.md`
- Slice completed in lifecycle/01
- `docs/21-integration-deployment/003-ci-cd-pipeline.md` if CI impact

## Goal

Tambahkan atau verifikasi test plan + automated tests untuk slice yang sama (doc: test matrix; app: unit/integration).

## Constraints

- Include tenant isolation negative case if data access
- No flaky retries on mutations without idempotency

## Validation

- [checklists/testing-release.md](../checklists/testing-release.md)

## Output

Test list, CI impact, gaps.

## Next

`lifecycle/03-deployment-increment.md` if deployable.
