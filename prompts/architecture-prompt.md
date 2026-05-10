# Prompt: Generate `architecture.md`

Generate `architecture.md`.

## Responsibility

`architecture.md` defines how the system is organized.

## Include

- Purpose
- Source of Truth
- Codex Usage
- Non-Goals
- System overview
- Application boundaries
- Module boundaries
- Layering model
- Dependency direction
- Request lifecycle
- Authorization placement
- Error handling strategy
- Integration boundaries
- Forbidden architecture patterns
- Recommended directory structure

## Rules

- Do not define complete DB schema or API contracts.
- Do not include local commands.
- Do not duplicate shared canonical decisions from `project-decisions.md`.
- Keep within the architecture length budget.

Output complete Markdown suitable for `docs/architecture.md`.
