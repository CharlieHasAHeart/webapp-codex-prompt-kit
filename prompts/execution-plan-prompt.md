# Execution Plan Prompt

```markdown
Please generate `execution-plan.md` based on these documents:

- `prd.md`
- `domain-model.md`
- `tech-stack.md`
- `architecture.md`
- `db-schemas.md`
- `api-design.md`
- `dev-environment.md`
- `acceptance-and-validation.md`

Responsibility:
`execution-plan.md` tells Codex the order of implementation.

Required sections:

1. Purpose
2. Source of Truth
3. Codex Usage
4. Non-Goals
5. Implementation Strategy
6. Milestones
7. Tasks
8. Task Dependencies
9. Parallelizable Work
10. Sequential Work
11. Validation per Milestone
12. Reporting Requirements
13. Risks
14. Assumptions
15. Open Questions

For each task, use this structure:

## TASK-XXX: Task name

### Goal
### References
### Preconditions
### Expected Changes
### Files or Areas to Modify
### Steps
### Validation
### Definition of Done
### Reporting Requirements

Rules:
- Use task IDs such as `TASK-001`.
- Each task must reference relevant requirement, entity, database, API, and validation IDs when available.
- Include required updates to `codex-execution-report.md` and `codex-metrics.json`.
- Do not include product vision, full database field definitions, full API bodies, long local environment explanations, or general Codex behavior rules.

Output complete Markdown that can be saved directly as `execution-plan.md`.
```
