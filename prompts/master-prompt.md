# Master Prompt

Use this prompt at the beginning of a new Web project conversation with ChatGPT.

```markdown
I want to prepare a complete Web development task for Codex.

Before Codex starts implementation, I want to generate the following 10 Codex-ready project documents:

## What Codex should build
1. `prd.md`
2. `domain-model.md`

## How Codex should design it
3. `tech-stack.md`
4. `architecture.md`
5. `db-schemas.md`
6. `api-design.md`

## How Codex should execute and prove completion
7. `AGENTS.md`
8. `execution-plan.md`
9. `dev-environment.md`
10. `acceptance-and-validation.md`

Your role is not to write application code. Your role is to help me convert this project idea into Codex-ready engineering context.

Follow these rules:

1. Prefer decisions over suggestions.
2. Avoid vague wording such as: maybe, could, should consider, as needed, if appropriate, possibly, later, optional, 可以, 可能, 建议, 尽量, 视情况.
3. Use strong execution language: Must, Must not, Use, Do not use, Required, Forbidden, Default, Out of scope.
4. If information is missing but not blocking, create an `Assumptions` section.
5. If information is missing and blocks implementation, create an `Open Questions` section.
6. Add IDs to important items where useful:
   - requirements: `REQ-*`
   - business rules: `BR-*`
   - entities: `ENT-*`
   - database objects: `DB-*`
   - APIs: `API-*`
   - validation items: `VAL-*`
   - tasks: `TASK-*`
7. Every document must include:
   - `Purpose`
   - `Source of Truth`
   - `Codex Usage`
   - `Non-Goals`
8. Optimize for Codex execution, not human presentation.
9. Do not include UI design documents unless I explicitly ask for them.
10. If a decision is required, propose a default decision and clearly mark it.

Project background:

[PASTE PROJECT IDEA HERE]

First, do not generate all 10 documents yet. Start by producing:

1. Project Understanding
2. Key Assumptions
3. Blocking Questions
4. Recommended Defaults
5. Proposed Document Generation Plan
```
