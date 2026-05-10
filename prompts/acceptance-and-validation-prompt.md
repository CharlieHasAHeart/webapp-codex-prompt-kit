# Acceptance and Validation Prompt

```markdown
Please generate `acceptance-and-validation.md` based on `prd.md`, `domain-model.md`, `api-design.md`, and `dev-environment.md`.

Responsibility:
`acceptance-and-validation.md` tells Codex what completed means and how to prove correctness.

Required sections:

1. Purpose
2. Source of Truth
3. Codex Usage
4. Non-Goals
5. Global Definition of Done
6. Required Validation Commands
7. Feature Acceptance Criteria
8. Given / When / Then Scenarios
9. Unit Test Requirements
10. Integration Test Requirements
11. E2E Test Requirements
12. Manual Validation Steps
13. Error Scenario Coverage
14. Boundary Conditions
15. Security Validation
16. Authorization Validation
17. Accessibility Baseline
18. Performance Baseline
19. Regression Checklist
20. Mapping to Requirements and APIs
21. Assumptions
22. Open Questions

For each validation item, use this structure:

## VAL-XXX: Feature or behavior name

### Related Requirements
### Related APIs
### Acceptance Criteria
### Required Tests
### Validation Commands
### Manual Checks

Rules:
- Use validation IDs such as `VAL-001`.
- Every core feature must have acceptance criteria and validation method.
- Use exact commands from `dev-environment.md`.
- Do not include technology rationale, full database schema, complete API request/response details, setup commands, or task order.

Output complete Markdown that can be saved directly as `acceptance-and-validation.md`.
```
