# Prompt: Generate `prd.md`

Generate `prd.md` for the current Web App project.

## Responsibility

`prd.md` tells Codex what product to build.

## Include

- Purpose
- Source of Truth
- Codex Usage
- Non-Goals
- Product background
- Target users
- User problems
- Product goals
- MVP scope
- Out of scope
- Requirements with stable `REQ-*` IDs
- Success criteria

## Rules

- Do not include database schemas, API contracts, runtime commands, or task plans.
- Use clear product decisions.
- Avoid vague language.
- Keep within the PRD length budget.
- If a requirement is future scope, label it explicitly as future scope.

Output complete Markdown suitable for `docs/prd.md`.
