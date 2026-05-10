# Prompt: Generate `domain-model.md`

Generate `domain-model.md` based on `prd.md`.

## Responsibility

`domain-model.md` defines the business world: entities, relationships, states, lifecycles, and business rules.

## Include

- Purpose
- Source of Truth
- Codex Usage
- Non-Goals
- Domain glossary
- Core entities with stable `ENT-*` IDs
- Relationships with stable `REL-*` IDs
- Business rules with stable `BR-*` IDs
- State machines and lifecycles when needed
- Forbidden interpretations

## Rules

- Do not define database tables or API endpoints.
- Do not include implementation commands.
- Keep business meaning separate from storage design.
- Keep within the domain model length budget.

Output complete Markdown suitable for `docs/domain-model.md`.
