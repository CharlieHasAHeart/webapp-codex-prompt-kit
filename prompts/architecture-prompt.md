# Architecture Prompt

```markdown
Please generate `architecture.md` based on `prd.md`, `domain-model.md`, and `tech-stack.md`.

Responsibility:
`architecture.md` tells Codex how the system should be organized.

Required sections:

1. Purpose
2. Source of Truth
3. Codex Usage
4. Non-Goals
5. System Overview
6. Application Boundaries
7. Module Boundaries
8. Recommended Directory Structure
9. Layering Model
10. Dependency Direction
11. Request Lifecycle
12. Authentication Flow
13. Authorization Strategy
14. Error Handling Strategy
15. Logging Strategy
16. Configuration Strategy
17. Caching Strategy
18. Background Jobs, if applicable
19. Third-Party Integration Boundaries
20. Architecture Constraints
21. Forbidden Architecture Patterns
22. Assumptions
23. Open Questions

Rules:
- Use deterministic rules for module boundaries and dependency direction.
- State where business logic must live.
- State where database access is allowed and forbidden.
- State where authorization checks must happen.
- Do not include full database schema, full API contract, local commands, detailed implementation tasks, or UI design specs.

Output complete Markdown that can be saved directly as `architecture.md`.
```
