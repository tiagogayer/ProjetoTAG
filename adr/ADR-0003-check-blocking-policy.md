# ADR-0003 — Check blocking policy

**Area:** ci · **Status:** active · **Date:** 2026-06-15

## Context

Merging into `main` is gated by required status checks. Those checks must protect day-to-day
quality without ever blocking a critical fix — documentation can wait; a production bug cannot.

## Decision

- The `docs` workflow's active validation jobs are **required** for merging into `main` —
  currently lint, link-check, and commit-message linting (see
  [ADR-0006](ADR-0006-commit-message-enforcement.md) and
  [ADR-0008](ADR-0008-validation-in-ci.md)).
- They are **not required on the working branch** — a push to a work branch never blocks; only
  the merge to `main` is gated.
- The repository admin bypass is **enabled**, so a critical fix can always be merged without
  waiting for checks.
- No required reviewer: with a single maintainer, self-approval is not real review, so the CI
  checks are the quality gate. A required reviewer would be added if a second contributor joined.

## Rejected alternatives

- **Path-filter + required without bypass** — GitHub keeps a required check as "pending" even
  when the job was skipped by a path filter, which can deadlock a PR.
- **Advisory-only checks** — lose the merge gate, which is the whole point of the protection.

## Consequences

- Quality is enforced in normal flow while a guaranteed escape hatch (admin bypass) keeps
  emergencies unblocked.
- The process stays auditable through the PR history.
