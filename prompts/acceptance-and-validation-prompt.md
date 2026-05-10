# Prompt: Generate `acceptance-and-validation.md`

Generate `acceptance-and-validation.md`.

## Responsibility

`acceptance-and-validation.md` defines what completion means and how correctness is proven.

## Include

- Purpose
- Source of Truth
- Codex Usage
- Non-Goals
- Global Definition of Done
- Required validation commands
- Feature acceptance criteria with stable `VAL-*` IDs
- Required tests
- Manual validation checks
- Error and edge cases
- Security and authorization checks
- Regression checklist

## Rules

- Every core feature should have a `VAL-*` ID.
- Reference exact commands from `dev-environment.md`.
- Do not include implementation task order.
- Reference `project-decisions.md` for shared thresholds or policies.
- Keep within the acceptance length budget.

Output complete Markdown suitable for `docs/acceptance-and-validation.md`.
