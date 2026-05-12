# Document Generation Order

## Purpose

Define the dependency order for generating Codex-ready Web App documents.

This file should only define generation order and dependency rules.

Detailed document ownership belongs in `document-responsibilities.md`.
Detailed document set definition belongs in `document-system.md`.

---

## Core Principle

```text
Generate source documents before dependent documents.
```

A document should be generated only after the documents it depends on are stable enough to reference.

---

## Standard Generation Order

| Step | File | Depends On | Unlocks |
|---:|---|---|---|
| 1 | `product-spec.md` | Project brief | `domain-model.md`, `project-decisions.md`, `architecture.md` |
| 2 | `domain-model.md` | `product-spec.md` | `backend-design.md`, `data-api-contract.md`, `execution-validation.md` |
| 3 | `project-decisions.md` | `product-spec.md`, `domain-model.md` | `architecture.md`, `dev-environment.md`, `AGENTS.md` |
| 4 | `architecture.md` | `product-spec.md`, `domain-model.md`, `project-decisions.md` | `frontend-design.md`, `backend-design.md` |
| 5 | `frontend-design.md` | `architecture.md`, `project-decisions.md` | UI docs, `data-api-contract.md`, `execution-validation.md` |
| 6 | `backend-design.md` | `architecture.md`, `domain-model.md` | `data-api-contract.md`, `execution-validation.md` |
| 7 | `data-api-contract.md` | `domain-model.md`, `frontend-design.md`, `backend-design.md` | frontend/backend implementation tasks, validation, implementation map |
| 8 | `dev-environment.md` | `project-decisions.md`, `architecture.md`, technology choices | `execution-validation.md`, `AGENTS.md` |
| 9 | UI docs | `product-spec.md`, `frontend-design.md`, UI standards | frontend tasks, UI validation, implementation map |
| 10 | `execution-validation.md` | all design, contract, environment, and UI docs | `implementation-map.md`, `AGENTS.md` |
| 11 | `implementation-map.md` | all source IDs and tasks | `AGENTS.md`, Codex handoff |
| 12 | `AGENTS.md` | full document set | Codex execution |

---

## UI Generation Order

When UI is in scope, generate UI documents in this order:

| Step | File | Depends On | Reason |
|---:|---|---|---|
| 1 | `UI_PAGE.yaml` | product scope, frontend routing direction | Defines semantic page structure first. |
| 2 | `UI_TOKENS.yaml` | UI stack, product visual direction | Defines reusable tokens before visual usage rules. |
| 3 | `UI_VISUAL_SPEC.yaml` | `UI_PAGE.yaml`, `UI_TOKENS.yaml` | Defines how structure uses tokens visually. |

UI docs should be generated before `execution-validation.md` so UI tasks and validations can reference them.

---

## Compact Mode Order

For small projects, `implementation-map.md` may be embedded into `execution-validation.md`.

Compact order:

| Step | File |
|---:|---|
| 1 | `product-spec.md` |
| 2 | `domain-model.md` |
| 3 | `project-decisions.md` |
| 4 | `architecture.md` |
| 5 | `frontend-design.md` |
| 6 | `backend-design.md` |
| 7 | `data-api-contract.md` |
| 8 | `dev-environment.md` |
| 9 | UI docs |
| 10 | `execution-validation.md` with embedded ID registry and matrix |
| 11 | `AGENTS.md` |

Use compact mode only when the embedded registry and matrix will remain short.

---

## Regeneration Rules

If an upstream document changes, downstream documents may need review or regeneration.

| Changed Document | Review or Regenerate |
|---|---|
| `product-spec.md` | all downstream documents |
| `domain-model.md` | backend design, data/API contract, execution validation, implementation map |
| `project-decisions.md` | architecture, frontend design, backend design, dev environment, AGENTS |
| `architecture.md` | frontend design, backend design, data/API contract, execution validation |
| `frontend-design.md` | UI docs, data/API contract, execution validation, implementation map |
| `backend-design.md` | data/API contract, execution validation, implementation map |
| `data-api-contract.md` | frontend design, backend design, execution validation, implementation map |
| `dev-environment.md` | execution validation, AGENTS |
| UI docs | frontend design, execution validation, implementation map |
| `execution-validation.md` | implementation map, AGENTS |
| `implementation-map.md` | AGENTS |

---

## Cross-Document Review Timing

Run cross-document review after these are generated:

```text
execution-validation.md
implementation-map.md
AGENTS.md
```

The review should check:

- source-of-truth consistency
- ID ownership
- missing IDs
- frontend/backend boundary violations
- DB/API drift
- validation command quality
- UI document boundary issues
- document bloat
- unnecessary duplicated content

---

## Anti-Patterns

### 1. Generating execution tasks before design

Bad:

```text
product-spec.md -> execution-validation.md -> frontend-design.md
```

Why bad:

- tasks become vague
- frontend/backend design gets hidden in execution docs
- validation commands are not grounded

### 2. Generating implementation map before source IDs exist

Bad:

```text
product-spec.md -> implementation-map.md -> domain-model.md
```

Why bad:

- map invents IDs
- source documents become inconsistent
- traceability becomes unreliable

### 3. Generating AGENTS too early

Bad:

```text
AGENTS.md -> all other docs
```

Why bad:

- reading order may be wrong
- command rules may be missing
- UI and validation rules may be incomplete

### 4. Generating visual spec before tokens

Bad:

```text
UI_PAGE.yaml -> UI_VISUAL_SPEC.yaml -> UI_TOKENS.yaml
```

Why bad:

- visual rules cannot reference stable token names

---

## Final Rule

If a document must reference IDs, commands, UI pages, validation rules, or implementation areas, generate it only after those references exist.
