# ADR-0004 — Branch strategy

**Area:** ci · **Status:** active · **Date:** 2026-06-17

## Context

`main` is protected and all changes integrate through pull requests, but the branch-naming
convention still had to be fixed. An early attempt named branches by Conventional Commit type
(`feat/…`, `ci/…`), which confused two different layers: Conventional Commits governs commit
messages, not branch names.

## Decision

**GitHub Flow**: `main` is protected; all changes go through a short-lived branch → pull request
→ rebase/squash merge (merge commits disabled, linear history).

**Branch naming:** `<issue-number>-<short-description>`, created with the **"Create a branch"**
button on the issue. This is the name GitHub generates natively (lowercase kebab-case, hyphen
separator, no type prefix — e.g. issue #1 → `1-test-branch-name`). The description is editable;
keep it short. Creating the branch from the issue links issue ↔ branch ↔ PR and closes the issue
on merge.

**Every branch starts from an issue** — there is no "trivial change" exception. Large work is a
**parent issue** broken into **sub-issues**; each leaf sub-issue gets its own branch and PR, and
the parent closes when its children are done (the parent usually has no branch of its own). So
every branch maps to an issue, but not every issue has a branch.

## Rejected alternatives

- **Conventional-Commit-type prefix** (`feat/…`, `ci/…`) — a layering error: Conventional Commits
  covers commit messages only and says nothing about branches, and a single branch carries commits
  of several types, making "the branch type" ambiguous.
- **GitFlow** (`develop`, `release/**`, `hotfix/**`) — no parallel maintained versions and no
  environments that `develop` would represent.
- **Trunk-based with direct pushes to `main`** — loses the pre-merge check gate and PR
  traceability.

## Consequences

- Work, branches, pull requests and the project board stay linked end to end, with a parent /
  sub-issue hierarchy.
- The native "Create a branch" button produces compliant names with no manual effort; any branch
  not starting with `<number>-` signals a deviation.
- Atomic Conventional Commits are preserved in the linear history (commit messages are unchanged
  — they remain a commit-level convention).
