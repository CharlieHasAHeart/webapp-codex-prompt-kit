# Project Decisions Prompt

## Target File

```text
docs/project-decisions.md
```

## Purpose

Generate the shared decision source of truth for a Codex-ready Web App project.

`project-decisions.md` formalizes cross-document decisions as stable `DEC-*` entries.

It should convert relevant candidate decisions from `product-spec.md` into explicit, enforceable project decisions.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Required upstream document:

```text
docs/product-spec.md
```

Pay special attention to:

- MVP scope
- out-of-scope items
- user roles
- candidate project decisions
- assumptions
- open questions
- any technology, UI, validation, or workflow choices discussed with the user

If `product-spec.md` is unavailable, use the available project context and state assumptions.

If a decision is not yet safe to formalize, keep it as an open question or mark it as provisional.

---

## Relevant Standards

Apply only the standards relevant to this document:

```text
standards/document-system.md
standards/document-responsibilities.md
standards/document-length-budgets.md
standards/codex-ready-writing-rules.md
standards/frontend-backend-boundary.md
standards/validation-strategy.md
```

Do not restate these standards in the generated document.

---

## Output Rules

Generate only:

```text
docs/project-decisions.md
```

Do not generate other project documents.

Only create `DEC-*` IDs in this file.

Do not create:

```text
REQ-*
ENT-*
REL-*
BR-*
FE-*
BE-*
DB-*
API-*
VAL-*
TASK-*
```

Do not define detailed frontend design, backend design, database schema, API contracts, commands, tasks, or validation cases here.

---

## Required Document Structure

Use this structure unless the project clearly needs a small adjustment:

```markdown
# Project Decisions

## Purpose

## Source of Truth

## Codex Usage

## Non-Goals of This Document

## Decision Summary

## Decisions

## Provisional Decisions

## Rejected Alternatives

## Open Decision Questions
```

---

## Section Rules

### Purpose

State that this document records shared project-wide decisions that later documents must follow.

These decisions should reduce duplication and prevent inconsistent choices across documents.

---

### Source of Truth

State that this document owns:

- `DEC-*`
- shared project-wide technical decisions
- shared execution decisions
- shared UI stack decisions
- shared validation policy decisions
- shared repository layout decisions
- selected package manager
- selected major frameworks
- selected persistence direction
- selected API style
- selected development mode
- rejected alternatives for shared decisions

State that this document does not own:

- product requirements
- domain entities and business rules
- frontend implementation details
- backend implementation details
- database schema details
- API endpoint details
- exact command syntax
- task order
- validation commands
- UI page structure
- UI token values
- UI visual rules

---

### Codex Usage

Tell Codex to use this document to:

- resolve shared project choices
- avoid choosing alternative frameworks or package managers
- keep later documents consistent
- identify forbidden alternatives
- understand which decisions are final and which are provisional

Tell Codex not to infer low-level implementation details from high-level decisions.

---

### Non-Goals of This Document

Explicitly state that this document does not define:

- complete architecture
- frontend route structure
- backend service structure
- database fields
- API request/response schemas
- Docker command syntax
- test commands
- UI YAML contents
- implementation tasks

---

### Decision Summary

Provide a compact summary table.

Recommended format:

```markdown
| ID | Area | Decision | Status | Applies To |
|---|---|---|---|---|
| DEC-001 | Repository | Use `apps/web`, `apps/api`, `packages/*`. | accepted | architecture, frontend, backend, dev environment |
| DEC-002 | Development | Use container-first development. | accepted | dev environment, execution validation, AGENTS |
```

Use status values:

```text
accepted
provisional
rejected
deferred
```

---

### Decisions

Each accepted decision must use a stable `DEC-*` ID.

Recommended format:

```markdown
### DEC-001: Repository Layout

Status: accepted

Decision:
- Use a monorepo layout:
  - `apps/web`
  - `apps/api`
  - `packages/*`

Applies to:
- `architecture.md`
- `frontend-design.md`
- `backend-design.md`
- `dev-environment.md`
- `AGENTS.md`

Rationale:
- Keeps frontend, backend, and shared code separated while preserving shared contracts.

Forbidden:
- Do not mix frontend and backend implementation in the same app directory.
- Do not let `apps/web` import from `apps/api`.
- Do not let `apps/api` import from `apps/web`.

Follow-up:
- Define concrete directory structure in `architecture.md`.
```

Rules:

- Keep rationale short.
- Include `Applies to`.
- Include `Forbidden` when the decision constrains Codex behavior.
- Include `Follow-up` when later documents must refine the decision.
- Do not add implementation details that belong in later documents.

---

### Provisional Decisions

Use this section for decisions that are likely but not fully confirmed.

Recommended format:

```markdown
| Area | Provisional Decision | Reason | Needs Confirmation In |
|---|---|---|---|
| Database | Use PostgreSQL. | MVP requires durable relational data. | data-api-contract.md |
```

Rules:

- Do not assign `DEC-*` IDs to provisional decisions unless they are strong enough to be referenced downstream.
- If a provisional decision must be referenced downstream, assign a `DEC-*` ID with `Status: provisional`.
- Mark what would confirm or change the decision.

---

### Rejected Alternatives

Use this section to prevent Codex from choosing known unwanted options.

Recommended format:

```markdown
| Area | Rejected Alternative | Reason |
|---|---|---|
| Package manager | pnpm | Project standard is npm. |
| UI library | Material UI | Project standard is shadcn/ui + Tailwind. |
```

Rejected alternatives may also appear under each `DEC-*`.

---

### Open Decision Questions

List unresolved decision questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Documents |
|---|---:|---|
| Is authentication required in MVP? | yes | architecture, backend-design, data-api-contract |
```

If a question is blocking, do not pretend the decision is final.

---

## Decision Selection Rules

Include a decision only if it affects at least two of:

- architecture
- frontend design
- backend design
- data/API contract
- dev environment
- execution validation
- implementation map
- AGENTS
- UI documents
- dependencies
- validation strategy
- repository structure

Good decisions include:

- repository layout
- package manager
- container-first development
- frontend framework
- backend framework
- UI stack
- CSS/styling approach
- database
- ORM/query layer
- API style
- auth direction
- validation strategy
- deployment target
- monorepo/shared package policy
- standalone vs embedded `implementation-map.md`

Bad decisions include:

- one API field
- one database column
- one component prop
- one function name
- one test filename
- one section heading
- one small copywriting choice

---

## Recommended Common Decisions

When supported by the project context, consider formalizing these:

### Repository Layout

Default:

```text
apps/web
apps/api
packages/*
```

### Development Mode

Default:

```text
container-first development
```

### Validation Strategy

Default:

```text
task-scoped validation first
milestone/release validation for broad checks
no full lint/typecheck/mypy/build for every task by default
```

### UI Stack

Common default:

```text
shadcn/ui + Tailwind + lucide-react
```

### UI Documentation

Common default:

```text
Generate UI_PAGE.yaml, UI_TOKENS.yaml, and UI_VISUAL_SPEC.yaml when UI is in scope.
```

### Implementation Map

Common default:

```text
Use standalone implementation-map.md for non-trivial projects.
Embed it into execution-validation.md only for small projects.
```

Do not force these defaults if the user clearly chose otherwise.

---

## Writing Rules

- Use stable `DEC-*` IDs.
- Keep decisions explicit.
- Prefer short decision entries over long explanations.
- Include applies-to lists.
- Include forbidden alternatives when useful.
- Do not repeat full content from `product-spec.md`.
- Do not define requirements, entities, DB objects, API endpoints, tasks, or validations here.
- Do not include command syntax unless the decision is only about command policy.
- Mark uncertain choices as provisional or open questions.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Every accepted decision has a DEC-* ID.
[ ] Each DEC affects multiple later documents or implementation areas.
[ ] Candidate decisions from product-spec.md were reviewed.
[ ] Uncertain choices are marked provisional or open.
[ ] Forbidden alternatives are explicit where useful.
[ ] No REQ/ENT/BR/FE/BE/DB/API/VAL/TASK IDs are created here.
[ ] No detailed API schema is included.
[ ] No detailed DB schema is included.
[ ] No detailed frontend/backend implementation design is included.
[ ] Later documents can reference these DEC-* IDs.
```
