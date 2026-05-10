# AGENTS Prompt

```markdown
Please generate `AGENTS.md` based on these documents:

- `prd.md`
- `domain-model.md`
- `tech-stack.md`
- `architecture.md`
- `db-schemas.md`
- `api-design.md`
- `execution-plan.md`
- `dev-environment.md`
- `acceptance-and-validation.md`

Responsibility:
`AGENTS.md` tells Codex how to work inside this repository.

Required sections:

1. Purpose
2. Instruction Priority
3. Required Reading Order
4. Work Process
5. Planning Rules
6. Implementation Rules
7. File Modification Rules
8. Dependency Management Rules
9. Database Change Rules
10. API Change Rules
11. Testing and Validation Rules
12. Documentation Sync Rules
13. Security Rules
14. Environment Variable Rules
15. Metrics and Reporting Rules
16. Assumption Policy
17. Blocker Policy
18. Completion Procedure
19. Final Response Format

Rules:
- Make `AGENTS.md` strong and operational.
- Require Codex to follow `dev-environment.md` for all commands.
- Require Codex to follow `execution-plan.md` for task order.
- Require Codex to follow `acceptance-and-validation.md` for completion standards.
- Require Codex to maintain `codex-execution-report.md` and `codex-metrics.json`.
- Explicitly forbid changing package manager, adding unnecessary dependencies, committing secrets, skipping validation, and silently changing API/DB behavior without updating documents.
- Do not include full product requirements, full API details, full DB schema, or long business explanations.

Output complete Markdown that can be saved directly as `AGENTS.md`.
```
