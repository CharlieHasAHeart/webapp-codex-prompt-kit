# Prompt: Generate `db-schemas.md`

Generate `db-schemas.md`.

## Responsibility

`db-schemas.md` defines how product data is stored.

## Include

- Purpose
- Source of Truth
- Codex Usage
- Non-Goals
- Selected database
- Storage scope
- Tables with stable `DB-*` IDs
- Fields
- Relationships
- Constraints
- Indexes
- Enums
- Migration rules
- Seed data expectations
- Sensitive data handling

## Rules

- If the project has a phased rollout, clearly separate current implementation scope from target-state schema.
- Prefer compact tables.
- Do not include API contracts.
- Do not include runtime commands.
- Reference `project-decisions.md` for shared DB rollout policy.
- Keep within the DB schema length budget.

Output complete Markdown suitable for `docs/db-schemas.md`.
