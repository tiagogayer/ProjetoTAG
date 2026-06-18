# ADR-0008 — Validation runs entirely in CI

**Area:** ci · **Status:** active · **Date:** 2026-06-17

## Context

This repository has three automated checks — file linting (Markdown/YAML), link checking, and
commit-message linting — and a ruleset that requires them before a merge. The open question was
where validation runs: in local git hooks on the contributor's machine, or in CI. The first
attempt wired commit-message linting as a local hook; this ADR settles it the other way.

## Decision

**All validation runs exclusively in CI** (GitHub Actions), in the `docs` workflow: file linting
in the `lint` job (`pre-commit run --all-files`), link checking in the `links` job, and
commit-message linting in the `commits` job (gitlint). There is **no local gate** — local git or
pre-commit hooks are not part of the model; running them on a machine is, at most, an optional
personal convenience. `.pre-commit-config.yaml` and `.gitlint` exist only as configuration that
the CI consumes.

## Rejected alternatives

- **A local hook gate** — rejected: it requires installing a toolchain (Python, pre-commit, Node)
  on the contributor's machine and creates a "passed locally but CI differs" path. Catching issues
  before pushing is a real benefit, but it does not justify the local setup here.
- **A hybrid (optional local + CI) as the model** — rejected: keeping local in the contract
  reintroduces the duplication and divergence this decision avoids. Anyone may still install hooks
  privately, but that is outside the model.

## Consequences

- No setup is needed to contribute: `git commit` and `git push`, and the pull request reports any
  failure.
- Fixing a file is a new commit; fixing a bad message is `git commit --amend` — accepted friction.
- The effective gate is the ruleset's required checks (see ADR-0003); the three `docs` jobs are
  those checks.
- Message feedback arrives after the push (on the pull request), not before the commit — accepted
  in exchange for zero setup.
