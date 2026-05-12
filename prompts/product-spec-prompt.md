# Product Spec Prompt

## Target File

```text
docs/product-spec.md
```

## Purpose

Generate the first formal project document for a Codex-ready Web App.

`product-spec.md` is the product source of truth. It defines what product should be built, what is in scope, what is out of scope, and which requirements later documents must reference.

This document may also capture early candidate project decisions discussed with the user, but it must not formalize them as `DEC-*`.

---

## Source Context

Use the available project brief, conversation context, uploaded notes, and product decisions already discussed with the user.

If no usable product context exists, ask for a concise project brief before generating.

If information is missing but the user wants to proceed, state explicit assumptions and continue.

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

Do not include prompt instructions, commentary, or implementation notes outside the target document.

Only create `REQ-*` IDs in this file.

Do not create:

```text
DEC-*
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

Candidate project decisions may be listed, but they must not use `DEC-*` IDs.

---

## Required Document Structure

Use this structure unless the project clearly needs a small adjustment:

```markdown
# Product Spec

## Purpose

## Source of Truth

## Codex Usage

## Non-Goals of This Document

## Product Summary

## Target Users

## User Problems

## MVP Scope

## Out of Scope

## User Roles

## Requirements

## User Stories

## Success Criteria

## Candidate Project Decisions

## Assumptions

## Open Questions
```

---

## Section Rules

### Purpose

State that this document defines product intent, product scope, and product requirements.

Do not describe detailed implementation design.

### Source of Truth

State that this document owns:

- product background
- target users
- user problems
- MVP scope
- non-goals
- user roles
- `REQ-*`
- user stories
- product-level success criteria
- candidate project decisions discovered during early discussion

State that this document does not own:

- formal `DEC-*` decisions
- domain entity definitions
- frontend design
- backend design
- database schema
- API contract
- command definitions
- task order
- validation commands
- UI visual rules

### Codex Usage

Tell Codex to use this document to understand:

- what must be built
- what must not be built
- which product workflows matter
- which requirements must be referenced by later documents
- which early candidate decisions should be confirmed in `project-decisions.md`

Tell Codex not to infer detailed implementation architecture from product background.

### Non-Goals of This Document

Explicitly state that this document does not define:

- database tables
- API endpoints
- frontend routes
- backend services
- command syntax
- implementation tasks
- acceptance test commands
- UI token values
- UI visual rules

### Product Summary

Provide a compact product explanation.

The summary should answer:

```text
What is the product?
Who is it for?
What core problem does the MVP solve?
```

Avoid marketing-style language.

### Target Users

List concrete user types.

Prefer a table:

```markdown
| User Type | Main Goal | Key Workflow |
|---|---|---|
| Analyst | Create and evaluate cases. | Create case, edit inputs, run assessment, review result. |
```

Avoid vague user types unless their responsibilities are defined.

### User Problems

List concrete problems the MVP solves.

Bad:

```text
Users need a better experience.
```

Good:

```text
Users currently compare assessment cases manually across spreadsheets, which makes updates and result review error-prone.
```

### MVP Scope

Define what is included in the first build.

Prefer a table:

```markdown
| MVP Area | Included Capability | Related REQ |
|---|---|---|
| Case management | Create, list, view, and edit cases. | REQ-001, REQ-002 |
```

Each MVP item should be traceable to one or more `REQ-*`.

### Out of Scope

Define what Codex must not implement.

Use strong language:

```text
Must not implement
Not part of MVP
Future scope
```

Recommended format:

```markdown
| Out-of-Scope Item | Reason | Future? |
|---|---|---|
| Enterprise SSO | Not required for MVP. | yes |
```

### User Roles

Define roles at the product level.

Recommended format:

```markdown
| Role | Description | MVP Permissions |
|---|---|---|
| Admin | Manages workspace settings and users. | Full access. |
| Analyst | Creates and edits assessment cases. | Case read/write. |
| Viewer | Reviews results. | Read-only. |
```

Do not define low-level permission enforcement here.

### Requirements

Requirements must use stable `REQ-*` IDs.

Recommended format:

```markdown
### REQ-001: Requirement Name

Type: functional / non-functional / constraint
Priority: must / should / future
MVP: yes / no

Description:
- ...

Acceptance intent:
- ...

Notes:
- ...
```

Rules:

- Each requirement should be specific enough to map to implementation.
- Avoid bundling unrelated behaviors into one requirement.
- Label future-scope requirements clearly.
- Do not describe database columns or API endpoints.
- Do not describe frontend component structure.
- Do not describe backend service structure.

### User Stories

Use user stories only when they clarify workflows.

Recommended format:

```markdown
| Story | Role | Need | Outcome | Related REQ |
|---|---|---|---|---|
| US-001 | Analyst | create a case | track assessment inputs and results | REQ-001 |
```

Do not use user stories as a replacement for requirements.

### Success Criteria

Define observable product-level success criteria.

Examples:

```text
User can create, edit, run, and review a case without using external spreadsheets.
A refreshed page preserves case inputs and latest successful result.
Core MVP workflows are covered by validation criteria in later documents.
```

Do not include exact test commands here.

### Candidate Project Decisions

Capture technical, execution, and UI decisions that emerged during early project discussion.

These are candidates, not formal `DEC-*` decisions.

Recommended format:

```markdown
| Area | Candidate Decision | Confidence | Should Confirm In | Notes |
|---|---|---|---|---|
| Repository | Use `apps/web`, `apps/api`, `packages/*` monorepo layout. | high | project-decisions.md / architecture.md | Default for this prompt kit. |
| Development | Container-first development. | high | project-decisions.md / dev-environment.md | Commands should run through Docker. |
| UI | Use shadcn/ui + Tailwind. | high | project-decisions.md / frontend-design.md | UI specs handled separately. |
| Database | Use PostgreSQL. | medium | project-decisions.md / data-api-contract.md | Confirm based on persistence needs. |
```

Rules:

- Do not assign `DEC-*` IDs here.
- Include only decisions likely to affect multiple later documents.
- If a decision is uncertain, mark confidence as `low` or `medium`.
- If no technical decisions are known yet, write `None yet`.

Good candidate decisions include:

- repository layout
- container-first development
- package manager preference
- frontend framework
- backend framework
- database
- ORM
- UI stack
- API style
- auth direction
- deployment target
- validation strategy
- whether UI YAML docs are needed
- whether standalone `implementation-map.md` is needed

Bad candidate decisions include:

- one API field
- one database column
- one component prop
- one test filename
- one service method name

### Assumptions

List assumptions that affect product scope, requirements, or candidate decisions.

Recommended format:

```markdown
| Assumption | Impact | Confirm Later? |
|---|---|---|
| The MVP is single-tenant. | Simplifies auth and data model. | yes |
```

### Open Questions

List unresolved product questions.

Each question should state whether it is blocking.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Should viewers be able to export results? | no | future scope |
| Is authentication required in MVP? | yes | roles, API, data model |
```

---

## Writing Rules

- Use clear product language.
- Use stable `REQ-*` IDs.
- Keep product background concise.
- Prefer tables for roles, scope, stories, candidate decisions, assumptions, and open questions.
- Mark future scope explicitly.
- Do not include detailed implementation design.
- Do not include command lines.
- Do not include validation commands.
- Do not include database fields.
- Do not include API endpoint definitions.
- Do not include frontend or backend module design.
- Do not create formal `DEC-*` IDs here.
- Do not create task IDs.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] The document has a clear MVP scope.
[ ] The document has explicit non-goals.
[ ] Requirements use stable REQ-* IDs.
[ ] Each REQ is specific enough to map to implementation.
[ ] Future scope is clearly labeled.
[ ] Product background is not excessive.
[ ] Candidate project decisions are captured without DEC-* IDs.
[ ] Candidate decisions only include cross-document decisions.
[ ] No DB/API/FE/BE/TASK/VAL/ENT/BR/DEC IDs are defined here.
[ ] No implementation commands are included.
[ ] Open questions are marked blocking or non-blocking.
```
