# API Design Prompt

```markdown
Please generate `api-design.md` based on `prd.md`, `domain-model.md`, `architecture.md`, and `db-schemas.md`.

Responsibility:
`api-design.md` tells Codex the API contract between client and server or between services.

Required sections:

1. Purpose
2. Source of Truth
3. Codex Usage
4. Non-Goals
5. API Style
6. Authentication
7. Authorization Rules
8. Common Request Rules
9. Common Response Rules
10. Error Format
11. Endpoint List
12. Endpoint Details
13. Pagination
14. Filtering
15. Sorting
16. Idempotency
17. Rate Limiting, if applicable
18. API Versioning, if applicable
19. Sensitive Field Policy
20. Mapping to Requirements and Domain Entities
21. Assumptions
22. Open Questions

For each endpoint, use this structure:

## API-XXX: METHOD /path

### Purpose
### Related Requirements
### Related Domain Entities
### Auth
### Request Params
### Request Body
### Response Body
### Errors
### Notes

Rules:
- Use API IDs such as `API-001`.
- Define exact request and response shapes.
- Define error codes and HTTP status codes.
- Do not include ORM code, UI component design, local commands, or task order.

Output complete Markdown that can be saved directly as `api-design.md`.
```
