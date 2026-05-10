# Prompt: Generate `api-design.md`

Generate `api-design.md`.

## Responsibility

`api-design.md` defines API contracts.

## Include

- Purpose
- Source of Truth
- Codex Usage
- Non-Goals
- API style
- Auth and permission rules
- Endpoint list with stable `API-*` IDs
- Request params and body
- Response body
- Error envelope and error codes
- Pagination, filtering, sorting when needed
- Sensitive fields that must not be returned

## Rules

- Keep request and response examples compact.
- Do not include database implementation details.
- Do not include task order.
- Reference `project-decisions.md` for rollout decisions.
- Keep within the API design length budget.

Output complete Markdown suitable for `docs/api-design.md`.
