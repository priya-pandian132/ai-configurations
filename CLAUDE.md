# CLAUDE.md — ReschedulingEngine (Team Aurora)
 
## Context
- Service: customer rescheduling flow on DMG's Zero Touch critical path. (From the given example)
- Stack: Python, gRPC (SchedulerV2), Kafka, PostgreSQL. Flags via Statsig. (From the given example)
- Flow: POST /reschedule -> eligibility (custom DSL) -> SchedulerV2 RPC -> publish `job.rescheduled` -> NotificationService.
## Commands (verify these exist before relying on them) (From the given example)
- Test: `make test` (unit), `make test-integration`
- Lint/types: `make lint`
- Run locally: `make dev`
- Never run `./deploy.sh`. Deploys go through CI only.
## Conventions
- Every new endpoint or consumer must define: idempotency key, timeout, retry policy, and failure behavior.
- Emit a metric and a structured log for every new failure path.
- New behavior ships behind a Statsig flag with a documented rollback.
- Keep diffs small: one concern per PR. Describe what, why, and how it was tested.
## Testing bar
- Cover the failure paths, not just the happy path: error, partial failures, duplicate requests, timeouts.
- Bug fixes start with a failing test that reproduces the bug.
- Add characterization tests before changing legacy code.
## Autonomy
- Do freely: read code, run tests and lint, edit files in this repo, create branches, draft PRs.
- Ask first: schema or migration changes, new dependencies, changes to Kafka topics or event contracts, anything touching the existing code, flag rollout changes.
- Never: deploy, push to main, run commands against production, modify CI or secrets config, or commit credentials.
- If a task is ambiguous, ask one focused question, then proceed on your stated assumption.
## How to work with me
- Push back when my approach is wrong. Lead with the specific risk and a better alternative; do not simply agree.
- Say what you verified and what you assumed. Mark anything unverified.
- Before a large change, give a short plan (files, risks, test approach) and wait for a go-ahead.
- When reviewing code, check in order: correctness, idempotency, error handling, observability, tests, style.
## Secrets
- Credentials come from environment variables or the secret manager only. Never write them to files, logs, or commit messages.
- If you find a secret in the repo, stop and tell me so it can be rotated.
