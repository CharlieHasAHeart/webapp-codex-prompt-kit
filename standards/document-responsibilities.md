# Document Responsibilities

This file defines the responsibility boundary for each Codex-ready project document.

## What Codex Should Build

### `prd.md`

Defines product goals, users, requirements, workflows, scope, non-goals, and success criteria.

It must not define database schemas, API contracts, technology choices, commands, or Codex behavior rules.

### `domain-model.md`

Defines domain concepts, entities, relationships, states, lifecycles, business rules, and invariants.

It must not define SQL, ORM schemas, API endpoints, UI components, package managers, or runtime commands.

## How Codex Should Design It

### `tech-stack.md`

Defines technology choices and forbidden alternatives.

It must not define exact commands, database fields, API request/response bodies, implementation tasks, or Codex rules.

### `architecture.md`

Defines system structure, module boundaries, layers, dependency direction, authentication flow, authorization strategy, and architectural constraints.

It must not define complete database schema, complete API contract, local commands, or detailed task order.

### `db-schemas.md`

Defines storage structure: tables, fields, keys, constraints, indexes, enums, migrations, seed data, and sensitive data handling.

It must not define product vision, endpoint details, UI design, or runtime commands.

### `api-design.md`

Defines API contract: routes, methods, request shapes, response shapes, errors, auth, pagination, filtering, sorting, and sensitive field policy.

It must not define ORM implementation, UI design, local commands, or task order.

## How Codex Should Execute and Prove Completion

### `AGENTS.md`

Defines Codex behavior rules inside the repository.

It must not contain full product, API, or database documentation.

### `execution-plan.md`

Defines implementation order, milestones, tasks, dependencies, validation per task, and reporting requirements.

It must not contain product vision, full DB schema, full API bodies, or long local environment explanations.

### `dev-environment.md`

Defines runtime versions, package manager policy, install/run/build/lint/typecheck/test/migration/seed commands, environment variables, allowed commands, and forbidden commands.

It must not contain product requirements, API contracts, database fields, or business rules.

### `acceptance-and-validation.md`

Defines completion criteria, required tests, validation commands, error scenarios, security checks, and regression checks.

It must not contain technology rationale, full database schema, full API details, setup instructions, or task order.

## Runtime Documents

### `codex-execution-report.md`

Records Codex execution progress, commands, failures, validation results, assumptions, blockers, rework, files modified, and final notes.

### `codex-metrics.json`

Stores machine-readable execution metrics for prompt evolution and comparison.
