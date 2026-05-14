# Project Decisions Prompt

## Target File

```text
docs/project-decisions.md
```

## Purpose

Generate a compact project decision reference catalog for a Codex-ready Web App project.

`project-decisions.md` owns:

```text
DEC-* shared project decisions
rejected alternatives
open decision questions
```

It exists so `execution-validation.md` can reference precise project decisions from `TASK-*`.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Recommended upstream context:

```text
Project Design Brief
docs/product-spec.md
current project discussion
uploaded project notes
```

Use `product-spec.md` for:

- MVP boundary
- user roles
- `REQ-*`
- open product questions

Use the Project Design Brief for:

- candidate project decisions
- engineering constraints
- UI direction
- execution risks
- technology preferences

If `product-spec.md` is unavailable, use the available project context and state assumptions.

If a decision is not ready to formalize, place it under `Open Decision Questions`.

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

Create only:

```text
DEC-*
```

Do not create:

```text
REQ-*
ENT-*
REL-*
BR-*
DB-*
API-*
FE-*
BE-*
TASK-*
VAL-*
```

Every `DEC-*` must be heading-addressable.

Use this heading format:

```markdown
### DEC-001: Decision Name
```

Do not write long ADR-style narratives.

Do not include implementation task details.

---

## Required Document Structure

Use this structure:

```markdown
# Project Decisions

## Decision Catalog

## Rejected Alternatives

## Open Decision Questions
```

Do not add extra sections unless they are necessary for the project.

---

## Section Rules

### Decision Catalog

Generate compact `DEC-*` entries.

Each entry should be independently readable because `TASK-*` items in `execution-validation.md` will reference individual decisions directly.

Recommended format:

```markdown
### DEC-001: Repository Layout

Status: accepted
Area: repository

Decision:
- Use a monorepo layout:
  - `apps/web`
  - `apps/api`
  - `packages/*`

Applies To:
- architecture
- frontend
- backend
- dev environment
- execution validation

Forbidden:
- Do not mix frontend and backend implementation in one app directory.
- Do not let `apps/web` import from `apps/api`.
- Do not let `apps/api` import from `apps/web`.

Rationale:
- Keeps frontend, backend, and shared app-agnostic code separated.

Follow-up:
- Concrete command paths belong in `docs/dev-environment.md`.
```

Rules:

- Use stable `DEC-*` IDs.
- Keep each decision compact.
- Include `Status`.
- Include `Area`.
- Include `Decision`.
- Include `Applies To`.
- Include `Forbidden` when the decision constrains Codex behavior.
- Include short `Rationale` only when useful.
- Include `Follow-up` only when later catalogs must refine the decision.
- Do not define detailed frontend, backend, DB, API, task, or validation content.

Recommended status values:

```text
accepted
provisional
deferred
```

Recommended decision areas:

```text
repository
development
package-manager
frontend
backend
database
api
auth
ui
validation
testing
deployment
security
```

Good `DEC-*` entries include:

- repository layout
- container-first development
- package manager
- frontend framework
- backend framework
- database
- ORM/query layer
- API style
- UI stack
- auth direction
- validation policy
- testing tools
- deployment target
- shared package policy

Bad `DEC-*` entries include:

- one API field
- one database column
- one component prop
- one function name
- one test filename
- one small copywriting choice

---

### Rejected Alternatives

List alternatives that Codex must not choose.

Recommended format:

```markdown
| Area | Rejected Alternative | Reason | Related Decision |
|---|---|---|---|
| Package manager | pnpm | Project standard is npm. | DEC-002 |
| UI library | Material UI | Project standard is shadcn/ui + Tailwind. | DEC-004 |
```

Rules:

- Keep this short.
- Include only alternatives likely to cause drift.
- If no rejected alternatives are known, write `None yet`.

---

### Open Decision Questions

List unresolved project decisions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area | Needed Before |
|---|---:|---|---|
| Should authentication be required in MVP? | yes | auth, API, backend, frontend | execution-validation.md |
```

Rules:

- Include only questions that affect later catalogs or execution tasks.
- Mark blocking questions clearly.
- Do not silently turn unknown decisions into accepted decisions.
- If a decision is likely but unconfirmed, use `Status: provisional` in the decision entry or list it here.

---

## Decision Selection Rules

Include a `DEC-*` only if it affects at least two of:

```text
architecture
frontend-design
backend-design
data-api-contract
dev-environment
execution-validation
AGENTS
UI documents
package installation
validation commands
repository structure
```

If a choice affects only one small implementation detail, do not make it a `DEC-*`.

---

## Writing Rules

- Write a reference catalog, not a long ADR collection.
- Use stable `DEC-*` headings.
- Make every `DEC-*` independently readable.
- Keep entries compact.
- Include `Forbidden` rules when they prevent Codex drift.
- Do not create non-DEC IDs.
- Do not include command catalogs.
- Do not include validation commands.
- Do not include DB schema.
- Do not include API contracts.
- Do not include frontend/backend implementation details.
- Do not include task lists.
- Mark uncertainty as provisional, deferred, or open.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] The file is a compact project decision reference catalog.
[ ] Every accepted decision has a DEC-* heading.
[ ] Every DEC-* affects multiple later catalogs or execution areas.
[ ] Uncertain decisions are provisional, deferred, or open.
[ ] Forbidden alternatives are explicit where useful.
[ ] Rejected alternatives are listed when they prevent drift.
[ ] Open decision questions are marked blocking or non-blocking.
[ ] No REQ/ENT/BR/DB/API/FE/BE/TASK/VAL IDs are created.
[ ] No implementation commands are included.
[ ] No long ADR narrative is included.
```
