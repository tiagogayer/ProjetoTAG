# Architecture Decision Records (ADRs)

This project records each significant technical decision as an ADR — a short, durable note
explaining **why** something was decided, not just what. One file per decision, so the
reasoning stays auditable as the project evolves.

## Record format

Each ADR has four sections:

- **Context** — the problem and the constraints that forced a decision.
- **Decision** — what was chosen.
- **Rejected alternatives** — the options considered and why they were not chosen.
- **Consequences** — what the decision implies going forward.

Records are kept concise, focused on the decision and its rationale.

## Index fields

- **ID** — stable identifier `ADR-NNNN`; also how ADRs cross-reference each other.
- **Title** — short name of the decision.
- **Area** — the part of the project the decision governs (e.g. `ci`, `docs`).
- **Status** — `active`, or `superseded by NNNN` once a later ADR replaces it.

| ID | Title | Area | Status |
| --- | --- | --- | --- |
| [ADR-0001](ADR-0001-lint-tools.md) | Lint tools | ci | active |
| [ADR-0002](ADR-0002-pre-commit-runner.md) | How pre-commit runs in CI | ci | active |
| [ADR-0003](ADR-0003-check-blocking-policy.md) | Check blocking policy | ci | active |
| [ADR-0004](ADR-0004-branch-strategy.md) | Branch strategy | ci | active |
| [ADR-0005](ADR-0005-diagram-tool.md) | Diagram tool | docs | active |
| [ADR-0006](ADR-0006-commit-message-enforcement.md) | Commit message enforcement | ci | active |
| ADR-0007 | Internal-only decision, not published | — | active |
| [ADR-0008](ADR-0008-validation-in-ci.md) | Validation runs entirely in CI | ci | active |
