# Cross-Document Review Prompt

```markdown
Please review the following 10 project documents for Codex readiness:

1. `prd.md`
2. `domain-model.md`
3. `tech-stack.md`
4. `architecture.md`
5. `db-schemas.md`
6. `api-design.md`
7. `dev-environment.md`
8. `acceptance-and-validation.md`
9. `execution-plan.md`
10. `AGENTS.md`

Review goals:

1. Detect requirement conflicts.
2. Detect terminology drift.
3. Detect technology stack conflicts.
4. Detect command conflicts.
5. Detect domain-model and database mismatches.
6. Detect API and database mismatches.
7. Detect API and acceptance criteria mismatches.
8. Detect execution-plan ordering problems.
9. Detect unclear or unverifiable acceptance criteria.
10. Detect missing AGENTS rules.
11. Detect places where Codex still has to guess.

Output format:

## Critical Issues
Issues that must be fixed before Codex starts.

## Warnings
Issues that may cause implementation drift.

## Missing Information
Information Codex still needs.

## Suggested Fixes
Concrete changes to make.

## Traceability Gaps
Missing REQ → BR/ENT → DB → API → VAL → TASK links.

## Codex Readiness Score
Give a 0-100 score and explain why.

Do not rewrite all documents unless I ask. First produce the review.
```
