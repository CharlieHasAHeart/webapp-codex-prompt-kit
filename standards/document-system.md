# Document System

## Purpose

Define the standard document set for Codex-ready Web App development.

This file is a system-level standard. It should answer:

- which documents exist
- which documents are core
- which documents are optional
- which documents are outside the core count
- when to use the compact mode

Detailed ownership rules belong in `document-responsibilities.md`.
Generation dependency rules belong in `document-generation-order.md`.

---

## Core Principle

```text
One background document, implementation-facing specs everywhere else.
```

`product-spec.md` may provide product background.

Every other document must directly affect at least one of:

- code structure
- frontend implementation
- backend implementation
- database schema
- API contract
- UI behavior
- commands
- tests
- task order
- validation evidence
- Codex execution rules

---

## Recommended 11-Document System

Use this as the default for serious Web App development.

```text
AGENTS.md
docs/
├── product-spec.md
├── domain-model.md
├── project-decisions.md
├── architecture.md
├── frontend-design.md
├── backend-design.md
├── data-api-contract.md
├── dev-environment.md
├── execution-validation.md
└── implementation-map.md
codex-execution-report.md
```

### Core Documents

| File | System Role |
|---|---|
| `AGENTS.md` | Codex execution protocol. |
| `product-spec.md` | Product background, scope, users, non-goals, and `REQ-*`. |
| `domain-model.md` | Domain entities, relationships, rules, states, and invariants. |
| `project-decisions.md` | Shared canonical decisions. |
| `architecture.md` | System-level boundaries and dependency direction. |
| `frontend-design.md` | Frontend implementation design. |
| `backend-design.md` | Backend implementation design. |
| `data-api-contract.md` | Database and API contracts. |
| `dev-environment.md` | Container-first command environment. |
| `execution-validation.md` | Tasks, validations, milestones, and completion evidence. |
| `implementation-map.md` | ID registry and cross-document implementation map. |
| `codex-execution-report.md` | Minimal runtime record of Codex task results. |

---

## UI Document Layer

UI documents are separate from the 11 core documents.

```text
docs/ui/
├── UI_PAGE.yaml
├── UI_TOKENS.yaml
└── UI_VISUAL_SPEC.yaml
```

| File | System Role |
|---|---|
| `UI_PAGE.yaml` | Semantic page structure, routes, navigation, sections, actions, and UI states. |
| `UI_TOKENS.yaml` | Reusable design tokens. |
| `UI_VISUAL_SPEC.yaml` | Visual usage rules for layout, components, states, responsiveness, and accessibility. |

UI authoring standards live in:

```text
standards/ui-authoring-specs/
├── UI_PAGE.authoring-spec.md
├── UI_TOKENS.authoring-spec.md
├── UI_VISUAL_SPEC.authoring-spec.md
└── shadcn-tailwind-implementation-standard.md
```

---

## Compact 10-Document Mode

For smaller projects, `implementation-map.md` may be embedded into `execution-validation.md`.

Compact mode uses:

```text
AGENTS.md
docs/
├── product-spec.md
├── domain-model.md
├── project-decisions.md
├── architecture.md
├── frontend-design.md
├── backend-design.md
├── data-api-contract.md
├── dev-environment.md
└── execution-validation.md
codex-execution-report.md
```

Use compact mode only when:

- the project has few flows
- the ID registry is short
- the traceability matrix is small
- `execution-validation.md` will not become a super-document

Use the default 11-document system when:

- frontend and backend work are both non-trivial
- there are many feature flows
- many IDs need mapping
- UI pages need explicit mapping to tasks and validation
- Codex benefits from a dedicated implementation map

---

## Runtime Reporting

The default runtime report is:

```text
codex-execution-report.md
```

It should remain minimal and record:

- task status
- validation command
- validation result
- failure reason
- blockers
- final summary

Do not include `codex-metrics.json` by default.

---

## Files Not Included by Default

Do not include these in the default system:

| File | Default Treatment |
|---|---|
| `prd.md` | Replaced by `product-spec.md`. |
| `db-schemas.md` | Merged into `data-api-contract.md`. |
| `api-design.md` | Merged into `data-api-contract.md`. |
| `execution-plan.md` | Merged into `execution-validation.md`. |
| `acceptance-and-validation.md` | Merged into `execution-validation.md`. |
| `traceability-matrix.md` | Replaced by `implementation-map.md`. |
| `codex-metrics.json` | Removed by default. |
| `templates/` | Not required for the prompt-first workflow. |
| `examples/` | Optional, not part of the lightweight standard. |

These may be reintroduced only when there is a clear project-specific reason.

---

## Expansion Rule

Add a new core document only when all conditions are true:

1. It owns a clear source-of-truth area.
2. It directly affects implementation or validation.
3. It cannot be a section inside an existing document.
4. It reduces ambiguity for Codex.
5. It does not mostly duplicate another document.

Before adding a new document, ask:

```text
Can this be a section in an existing document?
Can this be represented in implementation-map.md?
Can this be represented in a UI YAML file?
Can this be a standard rather than a project document?
```

---

## System Health Checks

A generated project document set is healthy when:

- every core document has a clear implementation purpose
- only `product-spec.md` contains broad background
- frontend design is not hidden inside execution documents
- backend design is not hidden inside execution documents
- DB and API contracts are kept together by default
- validation is container-first and task-scoped
- `implementation-map.md` has no orphan IDs
- UI documents are separate from core engineering documents
- `codex-execution-report.md` is minimal
- `codex-metrics.json` is absent unless explicitly requested
