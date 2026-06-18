# ADR-0001 — Lint tools

**Area:** ci · **Status:** active · **Date:** 2026-06-15

## Context

The documentation workflow must validate YAML, Markdown and links on every commit to `main`.
The required capability is fixed; the concrete tool choice is made here. The selection criterion
is a well-documented, portable standard with shallow, reversible coupling to GitHub Actions.

## Decision

Linting is orchestrated by [pre-commit](https://pre-commit.com/) with two hooks:

- **[yamllint](https://github.com/adrienverge/yamllint)** for YAML validation.
- **[markdownlint-cli2](https://github.com/DavidAnson/markdownlint-cli2)** for Markdown linting.

Link checking runs as a separate GitHub Actions job using
[lychee](https://github.com/lycheeverse/lychee-action).

Tool configurations live in `.markdownlint-cli2.jsonc` and `.yamllint.yml` at the repository
root and are auto-discovered by the hooks. Each tool implements a well-documented, portable
standard (CommonMark/MD001+ for Markdown; yamllint's documented ruleset for YAML), and coupling
to GitHub Actions is shallow — each action is a replaceable shell over the actual tool (swap in
2 lines if needed).

## Rejected alternatives

- **Renovate** — disproportionate for a single maintainer with two hooks; Dependabot already
  covers the `pre-commit` ecosystem natively.
- **Super/MegaLinter** — non-official, too broad for the current scope.

## Consequences

- Hook versions are tracked automatically by Dependabot's native `pre-commit` ecosystem support,
  leaving no orphaned pins.
- The same lint runs locally (`pre-commit run`) and in CI with no extra configuration (shift-left).
- The shallow, reversible coupling keeps the CI provider easy to change later.
