# Domain Model Prompt

## Target File

```text
docs/domain-model.md
```

## Purpose

Generate a compact domain reference catalog for a Codex-ready Web App project.

`domain-model.md` owns:

```text
ENT-* domain entities
REL-* domain relationships
BR-* business rules
STATE-* state concepts when needed
open domain questions
```

It exists so `execution-validation.md` can reference precise domain concepts and rules from `TASK-*`.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Recommended upstream context:

```text
Project Design Brief
docs/product-spec.md
docs/project-decisions.md
current project discussion
uploaded project notes
```

Use `product-spec.md` for:

- MVP boundary
- user roles
- `REQ-*`
- open product questions

Use `project-decisions.md` for:

- `DEC-*`
- decisions that affect domain ownership, auth, tenancy, persistence, or workflow behavior

If upstream documents are unavailable, use the available context and state assumptions.

If a domain concept is unclear and affects later implementation tasks, list it under `Open Domain Questions`.

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

Create only:

```text
ENT-*
REL-*
BR-*
STATE-*
```

Do not create:

```text
REQ-*
DEC-*
DB-*
API-*
FE-*
BE-*
TASK-*
VAL-*
```

You may reference existing:

```text
REQ-*
DEC-*
```

Every catalog ID must be heading-addressable.

Use these heading formats:

```markdown
### ENT-001: Entity Name
### REL-001: Relationship Name
### BR-001: Business Rule Name
### STATE-001: State Concept Name
```

Do not write a long domain narrative.

Do not include implementation design.

Do not include database schema, API contracts, command lines, or validation commands.

---

## Required Document Structure

Use this structure:

```markdown
# Domain Model

## Entity Catalog

## Relationship Catalog

## Business Rule Catalog

## State Catalog

## Open Domain Questions
```

If the project has no meaningful state concepts, keep `State Catalog` and write `None required for MVP`.

Do not add extra sections unless they are necessary for the project.

---

## Section Rules

### Entity Catalog

Generate compact `ENT-*` entries.

Recommended format:

```markdown
### ENT-001: Case

Meaning:
- A case is the main assessment record created and managed by a user.

Key Domain Attributes:
- name
- status
- owner
- created time
- latest result reference

Ownership:
- Belongs to one workspace or user, depending on the project decision.

Related:
- REQ-001
- REQ-004
- DEC-001
```

Rules:

- Keep attributes conceptual, not database fields.
- Do not include column types.
- Do not define API response fields.
- Do not define frontend components.
- Do not define backend services.
- Include ownership only when it affects access, persistence, or workflow behavior.
- Each `ENT-*` should be independently readable.

---

### Relationship Catalog

Generate compact `REL-*` entries only for relationships that affect implementation.

Recommended format:

```markdown
### REL-001: Case Owns Results

From:
- ENT-001 Case

To:
- ENT-003 Result

Cardinality:
- one-to-many

Meaning:
- A case may have multiple generated results over time.

Related Rules:
- BR-003
```

Rules:

- Use relationships when they affect DB, API, backend logic, UI behavior, or validation.
- Do not define foreign key names here.
- Do not duplicate database schema.
- If a relationship is obvious and has no implementation impact, omit it.

---

### Business Rule Catalog

Generate compact `BR-*` entries.

Recommended format:

```markdown
### BR-001: No Concurrent Active Run

Rule:
- A case must not have more than one active run at the same time.

Enforcement Expectation:
- Backend service must enforce this rule.
- Data layer may support it with a constraint when practical.

Failure Behavior:
- Return a conflict-style error through the documented API error envelope.

Related:
- REQ-008
- ENT-001
- STATE-002
```

Rules:

- Every `BR-*` must be enforceable.
- Use direct language: `must`, `must not`, `required`, `forbidden`.
- Include expected failure behavior when useful.
- Do not define exact API error codes unless they already exist.
- Do not define database constraints in detail.
- Do not define validation commands.
- Each `BR-*` should be independently readable.

---

### State Catalog

Generate `STATE-*` entries only when state affects implementation.

Recommended format:

```markdown
### STATE-001: Case Status

Applies To:
- ENT-001 Case

States:
| State | Meaning |
|---|---|
| draft | Case exists but required inputs are incomplete. |
| ready | Case is ready for the next workflow step. |
| archived | Case is no longer active. |

Allowed Transitions:
| From | To | Trigger | Related Rule |
|---|---|---|---|
| draft | ready | Required inputs completed. | BR-002 |

Forbidden Transitions:
| From | To | Reason |
|---|---|---|
| archived | ready | Archived cases cannot be reactivated in MVP. |
```

Rules:

- Use `STATE-*` only for meaningful lifecycle or workflow state.
- Do not create state entries for simple display labels.
- Keep state definitions compact.
- Do not define database enum implementation here.
- Do not define frontend state management here.

---

### Open Domain Questions

List unresolved domain questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Can archived cases be restored? | no | case lifecycle |
| Can two users edit the same case at the same time? | yes | concurrency, backend, UI |
```

Rules:

- Include only questions that affect entities, relationships, rules, states, ownership, or implementation tasks.
- Mark blocking questions clearly.
- Do not hide uncertainty inside entity or rule text.

---

## Catalog Design Rules

The generated file should behave like a task-scoped reference catalog.

This means:

- each ID entry must be short enough to read independently
- each ID entry must have a stable Markdown heading
- each ID entry should include related upstream IDs when useful
- task authors should be able to reference entries like:

```text
docs/domain-model.md#ENT-001
docs/domain-model.md#BR-001
docs/domain-model.md#STATE-001
```

Avoid broad narrative sections that Codex would need to read globally.

---

## Writing Rules

- Write a reference catalog, not a narrative domain document.
- Use stable heading-addressable IDs.
- Keep every entry compact and independently readable.
- Keep attributes conceptual.
- Make business rules enforceable.
- Reference existing `REQ-*` and `DEC-*` where useful.
- Do not create non-domain IDs.
- Do not include DB column types.
- Do not include API contracts.
- Do not include frontend/backend implementation details.
- Do not include implementation tasks.
- Do not include validation commands.
- Use `Open Domain Questions` for unresolved domain decisions.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] The file is a compact domain reference catalog.
[ ] Core entities have ENT-* headings.
[ ] Important relationships have REL-* headings.
[ ] Enforceable business rules have BR-* headings.
[ ] Meaningful state concepts have STATE-* headings when needed.
[ ] Every ID entry is independently readable.
[ ] IDs are heading-addressable.
[ ] Domain attributes are conceptual, not DB fields.
[ ] No DB/API/FE/BE/TASK/VAL IDs are created.
[ ] No implementation commands are included.
[ ] Open domain questions are marked blocking or non-blocking.
```
