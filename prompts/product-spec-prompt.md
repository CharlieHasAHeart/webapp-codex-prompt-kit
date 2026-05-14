# Product Spec Prompt

## Target File

```text
docs/product-spec.md
```

## Purpose

Generate a compact product requirement reference catalog for a Codex-ready Web App project.

`product-spec.md` owns:

```text
MVP boundary
user roles
REQ-* requirement entries
open product questions
```

It exists so `execution-validation.md` can reference precise product requirements from `TASK-*`.

---

## Source Context

Use the available conversation context and the output from `discovery-workshop-prompt.md` when available.

Recommended upstream context:

```text
Project Design Brief
current project discussion
uploaded project notes
```

If no usable product context exists, ask for a concise project brief before generating.

If information is incomplete but the user wants to proceed, state explicit assumptions inside the generated file only when those assumptions affect requirements.

---

## Relevant Standards

Apply only the standards relevant to this document:

```text
standards/document-system.md
standards/document-responsibilities.md
standards/document-length-budgets.md
standards/codex-ready-writing-rules.md
```

Do not restate these standards in the generated document.

---

## Output Rules

Generate only:

```text
docs/product-spec.md
```

Do not generate other project documents.

Create only:

```text
REQ-*
```

Do not create:

```text
DEC-*
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

Every `REQ-*` must be heading-addressable.

Use this heading format:

```markdown
### REQ-001: Requirement Name
```

Do not write a long PRD narrative.

Do not include implementation design.

Do not include command lines.

---

## Required Document Structure

Use this structure:

```markdown
# Product Spec

## MVP Boundary

## User Roles

## Requirement Catalog

## Open Questions
```

Do not add extra sections unless they are necessary for the project.

---

## Section Rules

### MVP Boundary

Define what is included, excluded, future, or unknown.

Recommended format:

```markdown
| Area | Status | Notes |
|---|---|---|
| Case management | included | Core MVP workflow. |
| Export | future | Not required for MVP. |
| Enterprise SSO | excluded | Not part of current scope. |
```

Use these status values:

```text
included
excluded
future
unknown
```

Rules:

- Keep this section short.
- Use it to prevent overbuilding.
- Do not define implementation details.
- If a boundary affects a requirement, reference it in the relevant `REQ-*` entry.

---

### User Roles

Define product-level roles only.

Recommended format:

```markdown
| Role | Goal | MVP Permissions |
|---|---|---|
| Analyst | Create and evaluate cases. | Create, read, update own cases. |
| Viewer | Review results. | Read-only access. |
```

Rules:

- Keep roles product-level.
- Do not define backend middleware, frontend guards, or database permission logic.
- If auth/permission details are unknown, state them in `Open Questions`.

---

### Requirement Catalog

Generate compact `REQ-*` entries.

Each entry should be independently readable because `TASK-*` items in `execution-validation.md` will reference individual requirements directly.

Recommended format:

```markdown
### REQ-001: Create Case

Type: functional
Priority: must
MVP: yes

Actor:
- Analyst

Requirement:
- The user can create a new case with the minimum required information.

Acceptance Intent:
- A created case persists and can be opened from the case list.

Out of Scope:
- Bulk import.
- Advanced templates.

Related Workflow:
- Case creation
```

Rules:

- Use stable `REQ-*` IDs.
- Keep each entry compact.
- Split unrelated behaviors into separate requirements.
- Use direct product language.
- Mark future requirements clearly.
- Include `Out of Scope` inside a requirement when needed to prevent overbuilding.
- Do not define database fields.
- Do not define API endpoints.
- Do not define frontend components.
- Do not define backend services.
- Do not define validation commands.
- Do not define task IDs.

Recommended requirement types:

```text
functional
non-functional
constraint
security
usability
data
workflow
```

Recommended priorities:

```text
must
should
could
future
```

---

### Open Questions

List unresolved product questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Is authentication required in MVP? | yes | roles, API, backend, frontend |
```

Rules:

- Include only questions that affect requirements, scope, roles, or execution planning.
- Mark blocking questions clearly.
- Do not hide uncertainty inside requirement text.

---

## Writing Rules

- Write a reference catalog, not a narrative PRD.
- Keep the file compact.
- Use stable `REQ-*` headings.
- Make every `REQ-*` independently readable.
- Keep `MVP Boundary` short.
- Keep `User Roles` short.
- Use `Open Questions` for unresolved product decisions.
- Do not create non-REQ IDs.
- Do not include implementation commands.
- Do not include validation commands.
- Do not include DB schema.
- Do not include API contracts.
- Do not include frontend/backend implementation details.
- Separate MVP from future scope.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] The file is a compact product requirement reference catalog.
[ ] MVP boundary is explicit.
[ ] User roles are defined at product level.
[ ] Every requirement has a REQ-* heading.
[ ] Every REQ-* is compact and independently readable.
[ ] Future scope is clearly marked.
[ ] Open questions are marked blocking or non-blocking.
[ ] No DB/API/FE/BE/TASK/VAL/ENT/BR/DEC IDs are created.
[ ] No implementation commands are included.
[ ] No long PRD narrative is included.
```
