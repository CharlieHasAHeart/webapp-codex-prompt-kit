# Document Length Budgets

Use length budgets to keep generated documents compact enough for Codex to use effectively.

| File | Target Length | Hard Limit | Notes |
|---|---:|---:|---|
| `project-decisions.md` | 2-4 pages | 6 pages | Shared decisions only. |
| `prd.md` | 6-12 pages | 15 pages | Requirements may be longer, but avoid long narrative. |
| `domain-model.md` | 5-10 pages | 12 pages | Business meaning, not DB fields. |
| `tech-stack.md` | 3-5 pages | 6 pages | Short and firm. |
| `architecture.md` | 6-10 pages | 12 pages | Boundaries and forbidden patterns. |
| `db-schemas.md` | 5-10 pages | 12 pages | Current scope first; target state may be summarized. |
| `api-design.md` | 6-10 pages | 12 pages | Compact endpoint contracts. |
| `dev-environment.md` | 4-8 pages | 10 pages | Put canonical commands near the top. |
| `acceptance-and-validation.md` | 6-10 pages | 12 pages | VAL IDs and validation commands. |
| `execution-plan.md` | 6-10 pages | 12 pages | Task order, not copied source docs. |
| `traceability-matrix.md` | 2-6 pages | 8 pages | Index only. |
| `AGENTS.md` | 3-6 pages | 8 pages | Strong rules, no product longform. |

## Compression Rules

If a document exceeds the hard limit:

1. Remove duplicated shared decisions and move them to `project-decisions.md`.
2. Replace copied sections with references.
3. Convert prose to tables.
4. Move future-state details out of current-sprint docs.
5. Keep Codex-critical rules and remove explanatory narrative.
