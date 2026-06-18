# ADR-0002 — How pre-commit runs in CI

**Area:** ci · **Status:** active · **Date:** 2026-06-15

## Context

CI must run pre-commit to execute the lint hooks (see [ADR-0001](ADR-0001-lint-tools.md)). Three
ways to invoke pre-commit in the workflow were evaluated, against the project principle that all
automation stays declarative and version-controlled.

## Decision

pre-commit is executed directly in the GitHub Actions workflow using `actions/setup-python`
(node24 runtime) + `pip install pre-commit` + `pre-commit run --all-files`. No cache, no
third-party action wrapping pre-commit. The approach is explicit, auditable, and uses only
standard actions already in the dependency graph.

## Rejected alternatives

- **`pre-commit/action`** — maintenance-only, and it embeds `actions/cache@v4` compiled for
  node20, which GitHub is deprecating; using it reintroduces node20 transitively.
- **pre-commit.ci (SaaS)** — requires installing a GitHub App whose permissions are not publicly
  audited, and its configuration lives outside the repository, contradicting the
  declarative/version-controlled principle.

## Consequences

- No node20 components remain in the pipeline.
- If CI becomes slow as more hooks are added, `actions/cache@v5` (node24) can be added.
