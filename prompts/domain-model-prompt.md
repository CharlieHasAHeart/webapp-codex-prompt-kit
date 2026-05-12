# Domain Model Prompt

## Target File

```text
docs/domain-model.md
```

## Purpose

Generate the domain source of truth for a Codex-ready Web App project.

`domain-model.md` translates product requirements into business concepts that code must preserve:

- domain vocabulary
- entities
- relationships
- business rules
- state machines
- lifecycles
- invariants
- domain-level ownership and permission meaning

It should describe the business world, not the storage layer, API layer, frontend layer, or backend module structure.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Required upstream documents:

```text
docs/product-spec.md
docs/project-decisions.md
```

Use `product-spec.md` for:

- product scope
- `REQ-*`
- user roles
- user workflows
- success criteria
- out-of-scope items

Use `project-decisions.md` only for decisions that affect domain interpretation.

If an upstream document is unavailable, use the available context and state assumptions.

If domain meaning is unclear and blocks modeling, ask the minimum necessary blocking questions.

---

## Relevant Standards

Apply only the standards relevant to this document:

```text
standards/document-responsibilities.md
standards/document-length-budgets.md
standards/codex-ready-writing-rules.md
```

Do not restate these standards in the generated document.

---

## Output Rules

Generate only:

```text
docs/domain-model.md
```

Do not generate other project documents.

Only create these IDs in this file:

```text
ENT-*
REL-*
BR-*
```

You may reference existing:

```text
REQ-*
DEC-*
```

Do not create:

```text
FE-*
BE-*
DB-*
API-*
VAL-*
TASK-*
```

Do not define database tables, API endpoints, frontend components, backend service files, tasks, or validation commands here.

---

## Required Document Structure

Use this structure unless the project clearly needs a small adjustment:

```markdown
# Domain Model

## Purpose

## Source of Truth

## Codex Usage

## Non-Goals of This Document

## Domain Glossary

## Core Entities

## Relationships

## Business Rules

## State Machines

## Lifecycles

## Domain Permissions and Ownership

## Invariants

## Forbidden Interpretations

## Assumptions

## Open Questions
```

---

## Section Rules

### Purpose

State that this document defines the domain concepts and rules that implementation must preserve.

Do not describe storage, API, frontend, or backend implementation details.

### Source of Truth

State that this document owns:

- domain terms
- `ENT-*`
- `REL-*`
- `BR-*`
- state machines
- lifecycle rules
- domain invariants
- domain-level ownership and permission meaning
- forbidden domain interpretations

State that this document does not own:

- database table definitions
- database field details
- API request/response schemas
- frontend routes
- frontend state management
- backend service file structure
- task order
- validation commands

### Codex Usage

Tell Codex to use this document to understand:

- which domain objects exist
- which relationships matter
- which business rules must be enforced
- which state transitions are allowed
- which invariants later backend, DB, API, and validation documents must preserve

Tell Codex not to infer database schema or API response shape from this document.

### Non-Goals of This Document

Explicitly state that this document does not define:

- database tables or columns
- API endpoints or payloads
- frontend routes or components
- backend services or repositories
- validation commands
- implementation tasks

---

## Domain Glossary

Define important business terms.

Recommended format:

```markdown
| Term | Meaning | Related IDs |
|---|---|---|
| Case | A project or assessment record being evaluated. | ENT-001 |
```

Rules:

- Keep definitions concise.
- Avoid implementation details.
- Use terms consistently across later documents.

---

## Core Entities

Use stable `ENT-*` IDs.

Recommended format:

```markdown
### ENT-001: Case

Definition:
- A case represents ...

Domain attributes:
- name
- status
- owner
- created time

Owned by:
- workspace / user / project

Related requirements:
- REQ-001
- REQ-004
```

Rules:

- Domain attributes are conceptual, not database fields.
- Do not define column types.
- Do not define API payloads.
- Avoid treating UI views as domain entities unless they represent real business concepts.

---

## Relationships

Use stable `REL-*` IDs.

Recommended format:

```markdown
### REL-001: Case Has Parameter Values

From: ENT-001 Case
To: ENT-002 Parameter Value
Cardinality: one-to-many

Meaning:
- A case owns the parameter values used for assessment.

Related rules:
- BR-002
```

Rules:

- Define business relationship meaning.
- Avoid storage implementation details.
- Do not specify foreign key names here.

---

## Business Rules

Use stable `BR-*` IDs.

Recommended format:

```markdown
### BR-001: No Concurrent Active Run

Rule:
- A case must not have more than one active risk run.

Enforced by:
- backend service
- transaction boundary
- database constraint where applicable

Related:
- ENT-001
- ENT-004
- REQ-028
```

Rules:

- Every business rule must be enforceable.
- Use direct language: `must`, `must not`, `required`, `forbidden`.
- Avoid vague rules such as "should be handled carefully".
- It is acceptable to mention likely enforcement layers, but do not define their implementation.

---

## State Machines

Define stateful domain concepts only when state affects implementation.

Recommended format:

```markdown
### State Machine: Case Status

Entity: ENT-001 Case

States:
| State | Meaning |
|---|---|
| draft | Case exists but required inputs are incomplete. |
| ready | Required inputs are complete. |
| archived | Case is no longer active. |

Allowed Transitions:
| From | To | Trigger | Rule |
|---|---|---|---|
| draft | ready | required inputs completed | BR-003 |

Forbidden Transitions:
| From | To | Reason |
|---|---|---|
| archived | ready | Archived cases cannot be reactivated in MVP. |
```

Do not add state machines when simple attributes are enough.

---

## Lifecycles

Define lifecycle sequences for important domain objects or workflows.

Use this section for flows such as:

- case creation
- run creation
- result generation
- invitation lifecycle
- approval lifecycle
- export lifecycle

Recommended format:

```markdown
### Lifecycle: Risk Run

1. User triggers run.
2. System validates case readiness.
3. System creates parameter snapshot.
4. System creates active run.
5. System computes result.
6. System marks run as completed or failed.
7. Latest successful result becomes visible.
```

Rules:

- Describe domain lifecycle, not implementation task order.
- Do not include code file names.
- Do not include validation commands.

---

## Domain Permissions and Ownership

Define ownership and permission meaning at the domain level.

Recommended format:

```markdown
| Concept | Ownership Meaning | Permission Meaning |
|---|---|---|
| Case | A case belongs to one workspace. | Users must belong to the workspace to access the case. |
| Result | A result belongs to one case. | Result visibility follows case visibility. |
```

Rules:

- Do not define frontend guards here.
- Do not define backend middleware here.
- Do not define exact API auth rules here.

---

## Invariants

List rules that must always remain true.

Recommended format:

```markdown
| Invariant | Related IDs |
|---|---|
| A result must belong to exactly one case. | ENT-001, ENT-005 |
| A completed run must have an immutable parameter snapshot. | ENT-004, BR-006 |
```

Invariants should later map to backend tests, DB constraints, API behavior, or validation criteria.

---

## Forbidden Interpretations

Explicitly state what Codex must not assume.

Examples:

```markdown
- A risk run is not the same as a risk result.
- A parameter definition is not the same as a case parameter value.
- The latest result means the latest successful result, not the latest attempted run.
```

This section is important when terms are easy to confuse.

---

## Assumptions

List assumptions made while generating the domain model.

Include only assumptions that affect domain meaning.

Recommended format:

```markdown
| Assumption | Domain Impact | Confirm Later? |
|---|---|---|
| Cases belong to one workspace. | Affects ownership and permissions. | yes |
```

---

## Open Questions

List unresolved domain questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Can archived cases be restored? | no | Case lifecycle |
```

---

## Writing Rules

- Use stable `ENT-*`, `REL-*`, and `BR-*` IDs.
- Reference `REQ-*` and `DEC-*` where useful.
- Keep domain attributes conceptual.
- Make business rules enforceable.
- Use forbidden interpretations to prevent term confusion.
- Do not define database columns.
- Do not define API payloads.
- Do not define frontend components.
- Do not define backend services.
- Do not define tasks or validation commands.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Core business entities have ENT-* IDs.
[ ] Important relationships have REL-* IDs.
[ ] Business rules have BR-* IDs.
[ ] Rules are enforceable.
[ ] State machines are defined only where state matters.
[ ] Invariants are explicit.
[ ] Domain permissions and ownership are clear.
[ ] No DB table or field definitions are included.
[ ] No API request/response schemas are included.
[ ] No FE/BE/DB/API/TASK/VAL IDs are defined here.
[ ] Requirements are referenced but not copied in full.
[ ] Ambiguous terms have forbidden interpretations.
```
