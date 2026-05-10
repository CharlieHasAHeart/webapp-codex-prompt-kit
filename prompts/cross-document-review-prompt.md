# Prompt: Cross-Document Review

Review the generated project documents for Codex readiness.

## Documents

Review:

- `project-decisions.md`
- `prd.md`
- `domain-model.md`
- `tech-stack.md`
- `architecture.md`
- `db-schemas.md`
- `api-design.md`
- `dev-environment.md`
- `acceptance-and-validation.md`
- `execution-plan.md`
- `traceability-matrix.md`
- `AGENTS.md`

## Check

1. Requirement conflicts
2. Terminology drift
3. Technology conflicts
4. Command conflicts
5. DB/API mismatches
6. Missing validation criteria
7. Task order issues
8. Source-of-truth ambiguity
9. Duplicated canonical decisions that should move to `project-decisions.md`
10. Traceability gaps in `traceability-matrix.md`
11. Missing IDs or invalid ID references
12. Document length budget violations
13. Places where Codex still has to guess

## Output Format

```markdown
# Cross-Document Review

## Critical Issues

## Warnings

## Traceability Gaps

## Length Budget Issues

## Duplicated Decisions

## Missing Information

## Recommended Fixes

## Codex Readiness Score
```
