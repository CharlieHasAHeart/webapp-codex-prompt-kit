# Document Generation Order

Generate project documents in this order:

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

## Why This Order

### `prd.md` first

The product scope must be defined before domain, architecture, database, or API decisions.

### `domain-model.md` second

The business model should drive database and API design.

### `tech-stack.md` before architecture

Architecture depends on chosen technologies.

### `architecture.md` before database and API details

Database and API decisions should follow module and dependency boundaries.

### `dev-environment.md` before acceptance and execution

Validation and task execution must reference exact commands.

### `acceptance-and-validation.md` before execution plan

Tasks should know what completion means.

### `execution-plan.md` near the end

The plan depends on product, domain, design, database, API, environment, and validation decisions.

### `AGENTS.md` last

It tells Codex how to use all other documents.
