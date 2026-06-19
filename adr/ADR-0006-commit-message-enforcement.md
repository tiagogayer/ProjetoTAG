# ADR-0006 — Commit message enforcement

**Area:** ci · **Status:** active · **Date:** 2026-06-17

## Context

Commit messages follow the project's convention `<type>(<area>): description (#<issue>)`
(Conventional Commits, a short subject, a mandatory body, and the issue number at the end of the
subject). Branch protection guards `main` but does not check the message convention, so without
an automated check it would rely on manual discipline. The repository already runs CI that lints
Markdown, YAML and links.

## Decision

**[gitlint](https://github.com/jorisroovers/gitlint)** runs as a dedicated CI job that lints
every commit on a pull request — no local setup is required. It checks the commit type, keeps the
scope optional, limits the subject length, requires the `(#<issue>)` suffix, and requires a
non-empty body. Its configuration lives in `.gitlint` at the repository root; its version is
pinned in the workflow.

## Rejected alternatives

- **commitlint** — more capable on rules, but the issue-number suffix would still need a custom
  pattern and it brings the Node ecosystem with no gain over the requirements; gitlint covers the
  type, subject length, body and the `(#<issue>)` suffix natively, in Python (MIT) — a lighter fit,
  not an unsuitable tool.
- **conventional-pre-commit** — a lighter checker, but it validates neither subject length nor the
  issue-number suffix, leaving two requirements unenforced.
- **commitizen / cocogitto** — release-management toolboxes; far broader than message linting.

## Consequences

- The whole gate is in CI: you commit and push, and the pull request reports any bad message.
  Fixing a message means amending the commit (not just adding another file).
- No local toolchain is required to contribute.
- The gate validates the PR's commits; with the default rebase merge they are preserved on `main`,
  but a **squash** merge (the noisy-branch exception, see
  [ADR-0004](ADR-0004-branch-strategy.md)) produces a new commit whose message is composed at merge
  time and is not covered by the check — the author must keep it convention-compliant.
- The gitlint version is pinned in the workflow (outside Dependabot's pre-commit ecosystem), so it
  is bumped manually — cheap, given gitlint's low release cadence.
