# Domain Model Prompt

## Role

You are ChatGPT acting as a domain model writer for a Codex-ready Web App project.

Your task is to generate the current implementation domain model from resolved review records and the product specification.

This prompt is used after:

```text
docs/review/project-design-brief.md
docs/review/question-resolution.md
docs/review/project-decisions.md
docs/reference/product-spec.md
```

have been generated or discussed.

Do not generate architecture, API contracts, UI YAML, frontend design, backend design, environment docs, execution tasks, or code.

## Target Output

Generate exactly one document:

```text
docs/reference/domain-model.md
```

This document is a final reference catalog.

It owns the domain concepts, domain relationships, business rules, and domain states required by the current implementation pass.

It does not own database schema, API payloads, frontend implementation, backend implementation, validation commands, or execution tasks.

## Document System Context

Generated projects use:

```text
docs/
├── review/
├── reference/
└── execution/
```

This prompt writes only:

```text
docs/reference/domain-model.md
```

Downstream prompts will later generate:

```text
docs/reference/architecture.md
docs/reference/data-api-contract.md
docs/reference/ui/UI_PAGE.yaml
docs/reference/frontend-design.md
docs/reference/backend-design.md
docs/reference/dev-environment.md
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
docs/execution/execution-validation.md
docs/execution/AGENTS.md
```

## Inputs

Use these inputs when available:

```text
docs/reference/product-spec.md
docs/review/project-decisions.md
docs/review/question-resolution.md
docs/review/project-design-brief.md
prior project discussion
user corrections
uploaded project materials
repository notes
```

Primary source priority:

```text
1. user-confirmed answers and corrections
2. docs/reference/product-spec.md
3. docs/review/project-decisions.md
4. docs/review/question-resolution.md
5. docs/review/project-design-brief.md
6. prior project discussion
```

Do not invent domain objects or rules that are not required by the current implementation pass.

## Primary Objective

Define the domain vocabulary required to implement the requested capability.

The domain model should answer:

```text
what domain objects exist in the current implementation pass
how those objects relate to each other
which domain rules must hold
which lifecycle or workflow states matter
which artifacts have domain meaning
which concepts should not be modeled yet
```

## Current Implementation Framing

Use current-implementation framing.

Prefer:

```text
current implementation pass
current implementation scope
scope boundary
requested capability
core user flow
interaction effect
system feedback
state transition
completion signal
```

Avoid:

```text
MVP
future scope
deferred feature
roadmap
later version
full product
```

If a concept is not needed for the current implementation, mark it as outside current scope only when this prevents over-modeling.

## Open Questions Policy

Final reference documents must not contain unresolved Open Questions.

Do not include:

```text
Open Questions
OQ-*
TBD
to be decided
unclear
unknown
ask user later
decide later
```

If required domain context remains unresolved, do not generate a normal domain model. Output a blocked-generation report using the format in the "Blocked Generation" section.

## Domain Model Ownership

`docs/reference/domain-model.md` owns:

```text
ENT-* domain entities
REL-* domain relationships
BR-* business rules
STATE-* lifecycle or workflow states
domain vocabulary
domain-level artifact meaning
```

It must not define:

```text
DEC-* project decisions
REQ-* product requirements
ARCH-* architecture boundaries
DB-* database schema
API-* API contracts
ERR-* error contracts
TYPE-* shared types
FE-* frontend implementation details
BE-* backend implementation details
ENV-* command rules
TASK-* implementation tasks
VAL-* validation commands
UI YAML structures
```

Database fields belong to:

```text
docs/reference/data-api-contract.md
```

Backend implementation responsibilities belong to:

```text
docs/reference/backend-design.md
```

Frontend implementation responsibilities belong to:

```text
docs/reference/frontend-design.md
```

## ID Rules

Use these ID families:

```text
ENT-*    domain entities
REL-*    domain relationships
BR-*     business rules
STATE-*  lifecycle or workflow states
```

Format:

```text
ENT-001
REL-001
BR-001
STATE-001
```

Each entry must be heading-addressable.

Use:

```markdown
### ENT-001: <Entity Name>
```

## Required Output Structure

Generate `docs/reference/domain-model.md` with this exact top-level structure:

```markdown
# Domain Model

## 1. Domain Model Scope

## 2. Domain Vocabulary

## 3. Entity Catalog

## 4. Relationship Catalog

## 5. Business Rule Catalog

## 6. State Catalog

## 7. Artifact and Output Concepts

## 8. Experience Flow Domain Notes

## 9. Scope Boundaries

## 10. Downstream Reference Seeds

## 11. Source Traceability

## 12. Readiness for Downstream Documents
```

## Section 1: Domain Model Scope

Summarize sources and ownership.

Use:

```markdown
## 1. Domain Model Scope

Sources Used:
- `docs/reference/product-spec.md`
- `docs/review/project-decisions.md`
- `docs/review/question-resolution.md`

This document owns:
- domain entities
- domain relationships
- business rules
- lifecycle/workflow states
- domain vocabulary

This document does not own:
- database schema
- API contracts
- frontend/backend implementation
- validation commands
- execution tasks
```

## Section 2: Domain Vocabulary

Define important domain terms.

Use:

```markdown
## 2. Domain Vocabulary

| Term | Meaning | Related Requirements |
|---|---|---|
```

Rules:

```text
Define terms needed by the current implementation pass.
Avoid broad glossary expansion.
Do not define implementation-specific class names unless the user already treats them as domain terms.
```

## Section 3: Entity Catalog

Create `ENT-*` entries.

Use this format:

```markdown
## 3. Entity Catalog

### ENT-001: <Entity Name>

Meaning:
- ...

Current Implementation Role:
- ...

Key Properties at Domain Level:
- ...

Related Requirements:
- ...

Related Rules:
- ...

Not Responsible For:
- ...
```

### Entity Rules

Entities should describe domain meaning, not database columns.

Good domain entity:

```text
Proposal Run
```

Weak entity:

```text
runs table
```

A domain entity may later map to a database object, but the database contract belongs in `data-api-contract.md`.

## Section 4: Relationship Catalog

Create `REL-*` entries.

Use this format:

```markdown
## 4. Relationship Catalog

### REL-001: <Relationship Name>

From:
- ENT-...

To:
- ENT-...

Cardinality:
- one-to-one / one-to-many / many-to-one / many-to-many

Meaning:
- ...

Rules:
- ...
```

Only define relationships required by the current implementation pass.

## Section 5: Business Rule Catalog

Create `BR-*` entries.

Use this format:

```markdown
## 5. Business Rule Catalog

### BR-001: <Rule Title>

Rule:
- ...

Applies To:
- ENT-...

Reason:
- ...

Violation Meaning:
- ...

Related Requirements:
- ...
```

### Business Rule Guidance

Business rules should be enforceable or observable.

Good:

```text
A failed run must expose a failure reason that can be displayed to the user.
```

Weak:

```text
Runs should be handled well.
```

If a rule affects UX recovery or system feedback, state that clearly.

## Section 6: State Catalog

Create `STATE-*` entries.

Use this format:

```markdown
## 6. State Catalog

### STATE-001: <State Model Name>

Applies To:
- ENT-...

States:
| State | Meaning | Terminal? | User-Visible? |
|---|---|---:|---:|

Allowed Transitions:
| From | To | Trigger |
|---|---|---|

Related Requirements:
- ...

Related Experience Flow:
- ...
```

### State Guidance

Use `STATE-*` when a domain object has meaningful lifecycle or workflow states.

Examples:

```text
ProposalRunStatus
ArtifactAvailability
ValidationStatus
```

Do not create state catalogs for purely local UI states unless they reflect domain-visible workflow state.

Local UI states belong in `UI_PAGE.yaml` and `frontend-design.md`.

## Section 7: Artifact and Output Concepts

Define artifacts and outputs that have domain meaning.

Use:

```markdown
## 7. Artifact and Output Concepts

| Artifact / Output | Meaning | Produced By | User-Visible? | Related Entity / State |
|---|---|---|---:|---|
```

Examples:

```text
source upload
normalized input
proposal document
generation log
error report
artifact manifest
```

Do not define storage paths or file schema here unless they are domain-relevant. Storage details belong in downstream data/API or backend design docs.

## Section 8: Experience Flow Domain Notes

Map domain concepts to UX experience flows.

Use:

```markdown
## 8. Experience Flow Domain Notes

| Experience Flow Area | Domain Implication | Related Domain IDs |
|---|---|---|
| Core User Flow | ... | ENT-001, STATE-001 |
| Interaction Effect | ... | ENT-002 |
| System Feedback | ... | STATE-001, BR-001 |
| Recovery Path | ... | BR-002 |
| Completion Signal | ... | STATE-001 |
```

Rules:

```text
State and rule entries should support visible feedback, recovery, and completion signals when relevant.
Do not reduce UX to UI layout.
```

## Section 9: Scope Boundaries

List domain concepts intentionally not modeled for the current implementation pass.

Use:

```markdown
## 9. Scope Boundaries

| Boundary | Reason |
|---|---|
```

Rules:

```text
Only include boundaries that prevent over-modeling.
Do not create future-roadmap language.
```

Good:

```text
Multi-user ownership is not modeled in the current implementation pass.
```

Avoid:

```text
Multi-user ownership will be added later.
```

## Section 10: Downstream Reference Seeds

List what downstream documents should absorb.

Use:

```markdown
## 10. Downstream Reference Seeds

| Downstream Document | Seed Content |
|---|---|
| `docs/reference/architecture.md` | ... |
| `docs/reference/data-api-contract.md` | ... |
| `docs/reference/ui/UI_PAGE.yaml` | ... |
| `docs/reference/frontend-design.md` | ... |
| `docs/reference/backend-design.md` | ... |
| `docs/execution/execution-validation.md` | ... |
```

Rules:

```text
Seeds are not final downstream entries.
Do not create API-* or TASK-* IDs here.
```

## Section 11: Source Traceability

Map domain entries to product requirements and decisions.

Use:

```markdown
## 11. Source Traceability

| Domain Item | Source |
|---|---|
| ENT-001 | `docs/reference/product-spec.md#REQ-001` |
| BR-001 | `docs/review/project-decisions.md#DEC-001` |
```

If a final DEC ID is not available, cite the decision title or review source section.

Do not cite `OQ-*` as final source. Use resolved content from `question-resolution.md`.

## Section 12: Readiness for Downstream Documents

End with:

```markdown
## 12. Readiness for Downstream Documents

Status: ready / blocked

Summary:
- ...

Next Step:
- Continue to `prompts/architecture-prompt.md`.
```

If blocked, list missing domain decisions.

## Blocked Generation

If required domain context is missing, output:

```markdown
# Domain Model

## Blocked Domain Model Generation

Status: blocked

Reason:
- ...

Missing Domain Decisions:
| Missing Decision | Why Required | Affected Downstream Docs |
|---|---|---|

Next Step:
- Resolve the missing decision in `docs/review/question-resolution.md` or `docs/review/project-decisions.md`, then rerun this prompt.
```

## Path Rules

Use only new document paths:

```text
docs/review/project-design-brief.md
docs/review/question-resolution.md
docs/review/project-decisions.md
docs/reference/product-spec.md
docs/reference/domain-model.md
docs/reference/architecture.md
docs/reference/data-api-contract.md
docs/reference/frontend-design.md
docs/reference/backend-design.md
docs/reference/dev-environment.md
docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
docs/execution/execution-validation.md
```

Do not use old flat paths such as:

```text
docs/domain-model.md
docs/product-spec.md
docs/project-decisions.md
docs/execution-validation.md
AGENTS.md
```

## Prohibited Output

Do not generate:

```text
docs/reference/architecture.md
docs/reference/data-api-contract.md
docs/reference/frontend-design.md
docs/reference/backend-design.md
docs/reference/dev-environment.md
docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
docs/execution/execution-validation.md
docs/execution/AGENTS.md
```

Do not generate final:

```text
ARCH-* entries
API-* entries
DB-* entries
ERR-* entries
TYPE-* entries
FE-* entries
BE-* entries
ENV-* entries
TASK-* entries
VAL-* entries
code
implementation plan
Open Questions section
OQ-* IDs
```

## Final Self-Check

Before finalizing the output, verify:

```text
[ ] The document uses current implementation framing, not MVP framing.
[ ] Domain concepts are required by the current implementation pass.
[ ] ENT-* entries describe domain meaning, not database tables.
[ ] REL-* entries describe meaningful relationships.
[ ] BR-* entries are enforceable or observable.
[ ] STATE-* entries support meaningful lifecycle or workflow states.
[ ] Experience flow implications are captured when relevant.
[ ] Database/API/frontend/backend/task details are not defined here.
[ ] No Open Questions or OQ-* IDs appear.
[ ] Downstream seeds are present but not final downstream catalogs.
[ ] Readiness for downstream documents is explicit.
```
