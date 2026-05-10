# DB Schemas Prompt

```markdown
Please generate `db-schemas.md` based on `prd.md`, `domain-model.md`, `tech-stack.md`, and `architecture.md`.

Responsibility:
`db-schemas.md` tells Codex how data should be stored.

Required sections:

1. Purpose
2. Source of Truth
3. Codex Usage
4. Non-Goals
5. Database Overview
6. Tables
7. Fields
8. Primary Keys
9. Foreign Keys
10. Unique Constraints
11. Indexes
12. Enums
13. Nullable and Required Fields
14. Default Values
15. Timestamps
16. Soft Delete Policy
17. Cascade Delete Policy
18. Migration Policy
19. Seed Data Requirements
20. Sensitive Data Handling
21. Data Integrity Rules
22. Mapping to Domain Entities
23. Assumptions
24. Open Questions

Rules:
- Use database object IDs such as `DB-USERS`, `DB-PROJECTS`.
- Each table must map to domain entities or explain why it exists.
- Do not expose sensitive fields in API examples.
- Do not include product vision, endpoint details, UI design, local commands, or Codex behavior rules.

Output complete Markdown that can be saved directly as `db-schemas.md`.
```
