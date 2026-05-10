# Codex-Ready Writing Rules

These rules apply to all generated project documents.

## 1. Prefer Decisions Over Suggestions

Bad:

```text
You may consider using pnpm.
```

Good:

```text
Use pnpm. Do not use npm, yarn, or bun.
```

## 2. Use Must / Must Not Language

Use:

- Must
- Must not
- Use
- Do not use
- Required
- Forbidden
- Default
- Out of scope

Avoid:

- maybe
- could
- should consider
- if needed
- as appropriate
- optional
- 可以
- 可能
- 建议
- 尽量
- 视情况

## 3. Separate Assumptions and Open Questions

Use `Assumptions` for non-blocking defaults.

Use `Open Questions` for decisions that block implementation.

## 4. Add IDs to Important Items

Use stable IDs:

- `REQ-*` for requirements
- `BR-*` for business rules
- `ENT-*` for entities
- `DB-*` for database objects
- `API-*` for APIs
- `VAL-*` for validation items
- `TASK-*` for tasks

## 5. Make Documents Traceable

A core requirement should ideally map across:

```text
REQ → BR/ENT → DB → API → VAL → TASK
```

## 6. Make Validation Explicit

Every core feature must include:

- acceptance criteria
- required tests
- validation command or manual check

## 7. Make Commands Copy-Pasteable

Commands must be exact.

Bad:

```text
Run tests.
```

Good:

```bash
pnpm test
```

## 8. State Forbidden Alternatives

When a tool choice is fixed, explicitly forbid common alternatives.

Example:

```text
Use pnpm. Do not use npm install, npm run, yarn, or bun.
```

## 9. Define Source of Truth

Each document must define what it owns and what it does not own.

## 10. Optimize for Execution

The goal is not elegant prose. The goal is to reduce Codex guessing, rework, and command mistakes.
