# Backend Design Prompt

## Purpose

Use this prompt to generate the backend implementation responsibility catalog for the current implementation.

The backend design document defines stable `BE-*` entries for backend responsibilities such as API handler behavior, service orchestration, repository/storage usage, artifact handling, background run execution, state transition handling, error production, security checks, and backend-side validation responsibilities.

It is a non-UI reference catalog. It must be ownership-decoupled and entry-self-contained.

It is not an API contract file, not a database schema file, not a frontend behavior file, and not an execution task file.

## Target Output

Generate exactly one document:

```text
docs/reference/backend-design.md
```

## Standards to Apply

Read only the standards listed below.

| Standard | Required? | Use For |
|---|---:|---|
| `standards/document-responsibilities.md` | yes | Enforces non-UI reference ownership, entry self-containment, and traceability without dependency. |
| `standards/flow-concepts-and-composition.md` | yes | Helps identify backend responsibilities required by Core User Flows, Side Effect Flows, artifacts, background work, state transitions, recovery behavior, and foundation readiness. |
| `standards/frontend-backend-boundary.md` | yes | Ensures backend fulfills the data/API contract without redefining frontend behavior or API ownership. |
| `standards/open-questions-policy.md` | yes | Prevents unresolved questions from entering final reference docs. |
| `standards/codex-ready-writing-rules.md` | yes | Ensures stable IDs, resolved wording, and Codex-safe reference entries. |
| `standards/document-length-budgets.md` | optional | Use to keep backend entries compact and addressable. |

## Standard Application Rules

Standards constrain how this prompt generates its target document. Standards do not create additional output targets.

Rules:
1. Read only the standards listed in this prompt.
2. Do not load all standards by default.
3. The current prompt defines the target output and required output structure.
4. Standards define reusable terminology, ownership boundaries, quality rules, and review constraints.
5. Do not copy large sections from standards into the generated document.
6. Do not generate documents requested by a standard unless this prompt explicitly targets them.
7. If required context remains unresolved under the standards, output a blocked-generation report instead of inventing missing decisions.

## Priority Rule

When generating the target document, use this priority order:

1. User-confirmed answers and corrections.
2. This prompt's target output and required output structure.
3. Required standards listed in this prompt.
4. Upstream generated project documents.
5. Prior project discussion.

If a conflict involves unresolved blockers, Open Questions leakage, unsafe scope invention, missing required decisions, or reference ownership dependency, output a blocked-generation report instead of generating a normal final document.

## Required Inputs

Use these upstream documents when available:

```text
docs/review/project-decisions.md
docs/review/question-resolution.md
docs/reference/product-spec.md
docs/reference/domain-model.md
docs/reference/architecture.md
docs/reference/data-api-contract.md
docs/reference/frontend-design.md
```

Do not require every reference document to understand this output. The generated backend entries must be self-contained in their own backend responsibility layer.

## Backend Design Ownership

`docs/reference/backend-design.md` owns:

```text
BE-*
backend responsibility catalog
API handler responsibilities
service responsibilities
repository responsibilities
storage responsibilities
artifact handling responsibilities
background job and run execution responsibilities
state transition responsibilities
error production responsibilities
security and trust-boundary responsibilities
backend-side validation responsibilities
integration adapter responsibilities
```

It must not own:

```text
product requirements
domain source definitions
architecture source rules
API request/response source contracts
database schema source definitions
frontend state behavior
frontend UI behavior
UI token values
environment command catalog
execution task sequencing
validation commands
final executable FLOW-*
TASK-*
VAL-*
Open Questions
```

## Reference Decoupling Rules

Because this is a non-UI reference catalog:

1. Every `BE-*` entry must be entry-self-contained.
2. Related IDs may be included only for traceability.
3. Do not write "see data-api-contract.md for details" as a substitute for backend responsibility content.
4. Do not copy API request/response tables into backend design.
5. Do not copy DB schema as a backend-owned source definition.
6. Do not redefine API contracts, database contracts, frontend behavior, domain rules, or architecture boundaries.
7. Backend entries may mention related flow areas, but must not perform full flow composition.

Allowed:

```markdown
Related Contracts:
- API-001
- DB-001
- ERR-001
```

Forbidden:

```markdown
BE-001 implements API-001. See API-001 for details.
```

## Flow-Aware Backend Rules

The backend design must support flow-first execution without becoming an execution plan.

Required:
- Identify backend responsibilities needed by current Core User Flows and Side Effect Flows.
- Describe how backend handlers, services, repositories, storage, and background work fulfill documented contracts without redefining those contracts.
- Describe backend responsibility for generated artifacts and intermediate artifacts when applicable.
- Describe state transition responsibilities required by current flows.
- Describe backend-side production of recoverable, failed, blocked, validation, not-found, and unauthorized errors when applicable.
- Describe recovery-support responsibilities when current scope requires retries, idempotency, cleanup, or preserved state.
- Describe security and trust-boundary responsibilities needed by backend behavior.
- Keep final executable `FLOW-*`, `TASK-*`, and `VAL-*` out of this document.

Forbidden:
- Generating a final flow catalog.
- Generating frontend behavior.
- Generating API source contract fields.
- Generating DB schema source definitions.
- Generating task order.
- Generating validation commands.

## Required Output Structure

```markdown
# Backend Design

## 1. Backend Scope

State what this document owns and what it does not own.

## 2. Backend Summary

Summarize the backend responsibility model in current-scope terms.

## 3. Backend Responsibility Catalog

### BE-001: <Backend Responsibility Name>

Type:
- api_handler / service / repository / storage / artifact_handling / background_job / state_transition / error_production / security / validation / integration_adapter / cleanup

Responsibility:
- ...

Allowed:
- ...

Forbidden:
- ...

Inputs Consumed:
- ...

Outputs / Side Effects:
- ...

Related IDs:
- ...

Flow Support:
- ...

Out of Scope:
- ...

## 4. Handler Responsibilities

Describe backend handler responsibilities for documented API contracts.

Do not redefine API request/response fields.

## 5. Service and Workflow Responsibilities

Describe backend service orchestration responsibilities.

Do not turn this into execution tasks.

## 6. Repository, Data, and Storage Responsibilities

Describe backend responsibility for using documented data/storage contracts.

Do not redefine DB schema.

## 7. Artifact Responsibilities

Describe backend handling of uploads, generated artifacts, artifact metadata, safe access, cleanup, or download support when relevant.

## 8. State Transition Responsibilities

Describe backend responsibility for state changes and lifecycle enforcement.

Do not redefine domain states as source definitions.

## 9. Error and Recovery Responsibilities

Describe backend responsibility for producing documented errors and supporting recovery behavior.

## 10. Security and Trust Boundary Responsibilities

Describe backend checks, isolation, file safety, input validation, authorization, or trust boundaries when relevant.

## 11. Out-of-Scope Backend Behavior

List backend responsibilities intentionally excluded from the current implementation.

## 12. Downstream Seeds

List concise seeds for flow composition, execution, and validation documents.

## 13. Final Readiness

Status: ready / blocked

If blocked, list missing decisions and affected downstream documents.
```

## BE Entry Requirements

Each `BE-*` entry must include:

```text
ID
name
type
responsibility
allowed
forbidden
inputs consumed
outputs or side effects
out-of-scope where useful
```

Optional but useful:

```text
related IDs
flow support
security impact
recovery behavior
artifact behavior
state transition impact
downstream seeds
```

## API Boundary Rules

Backend design may say:

```text
The backend handler validates the documented create-run request, invokes the run creation service, persists the initial state through the documented data object, and returns the documented response or error contract.
```

Backend design must not say:

```text
The create-run API request has fields ...
```

unless those fields are only referred to as traceability and are already defined in `data-api-contract.md`.

The source of truth for API request/response/error fields is always `docs/reference/data-api-contract.md`.

## Data Boundary Rules

Backend design may say:

```text
The backend repository writes and reads the documented run data object.
```

Backend design must not redefine:

```text
DB fields
database schema
ORM columns
migration details
```

unless those are implementation responsibilities and not source-of-truth schema definitions.

The source of truth for data objects and schema-like contracts is `docs/reference/data-api-contract.md`.

## Frontend Boundary Rules

Backend design must not define:

```text
frontend local state
React component structure
UI layout
UI tokens
frontend API client behavior
```

Backend design may mention frontend-visible effects only as backend outputs, such as:

```text
The backend returns a documented blocked error that the frontend can display.
```

## Writing Constraints

Use direct, resolved backend responsibility language.

Prefer:

```text
The backend creates a run record, persists the initial queued status, stores the uploaded source artifact reference, and returns the documented create-run response.
```

Avoid:

```text
The backend might create something and then the frontend handles it.
```

Avoid dependency-only wording:

```text
See API-001 for backend behavior.
```

Instead, state the backend responsibility here and use related IDs only as traceability.

## Blocked Generation Rules

Output a blocked-generation report instead of a normal backend design if:

- required backend responsibilities are unclear
- API contracts required for backend responsibilities are unresolved
- data/storage contracts required by backend behavior are unresolved
- artifact handling behavior is required but undecided
- background job or run execution behavior is required but undecided
- state transition behavior affects backend responsibilities but is unresolved
- security or trust-boundary responsibilities are required but undecided
- unresolved Open Questions would enter the final backend doc

Blocked-generation report structure:

```markdown
# Backend Design Generation Blocked

## Blocking Issues

| Issue | Decision Needed | Affected Docs | Flow Impact |
|---|---|---|---|

## Partial Safe Content

## Required User Decisions
```

## Final Checks

Before finalizing, verify:

- No unresolved Open Questions remain.
- No API request/response source contracts are defined.
- No DB schema source definitions are defined.
- No frontend behavior is defined.
- No environment command catalog is defined.
- No `TASK-*`, `VAL-*`, or final executable `FLOW-*` entries are created.
- Every `BE-*` entry is self-contained.
- Related IDs are traceability hints only.
- Backend design supports flow-first execution without becoming a flow composition document.
