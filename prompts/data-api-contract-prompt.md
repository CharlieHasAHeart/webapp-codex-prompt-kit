# Data/API Contract Prompt

## Purpose

Use this prompt to generate the data and API contract catalog for the current implementation.

The data/API contract document defines stable `DB-*`, `API-*`, `ERR-*`, and `TYPE-*` entries. It is the source of truth for data structures, API routes, request payloads, response payloads, error contracts, and shared contract types.

It is a non-UI reference catalog. It must be ownership-decoupled and entry-self-contained.

## Target Output

Generate exactly one document:

```text
docs/reference/data-api-contract.md
```

## Standards to Apply

Read only the standards listed below.

| Standard | Required? | Use For |
|---|---:|---|
| `standards/document-responsibilities.md` | yes | Enforces non-UI reference ownership, entry self-containment, and traceability without dependency. |
| `standards/flow-concepts-and-composition.md` | yes | Helps identify data/API contracts needed by Core User Flows, Side Effect Flows, artifacts, state transitions, and foundation readiness. |
| `standards/frontend-backend-boundary.md` | yes | Ensures frontend and backend consume/implement this contract without redefining it. |
| `standards/open-questions-policy.md` | yes | Prevents unresolved questions from entering final reference docs. |
| `standards/codex-ready-writing-rules.md` | yes | Ensures stable IDs, resolved wording, and Codex-safe contract entries. |
| `standards/document-length-budgets.md` | optional | Use to keep contract entries compact and addressable. |

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
```

Do not require every reference document to understand this output. The generated contract entries must be self-contained in their own data/API responsibility layer.

## Data/API Contract Ownership

`docs/reference/data-api-contract.md` owns:

```text
DB-*
API-*
ERR-*
TYPE-*
data objects and persistence structures
API routes
request payloads
response payloads
error contracts
shared contract types
status enums
artifact metadata contracts
contract-level validation rules
```

It must not own:

```text
product requirements
domain meaning beyond contract naming
architecture boundaries
frontend API client implementation
frontend state behavior
backend service implementation
backend repository implementation
ORM implementation
environment commands
execution task sequencing
validation commands
final executable FLOW-*
TASK-*
VAL-*
Open Questions
```

## Reference Decoupling Rules

Because this is a non-UI reference catalog:

1. Every `DB-*`, `API-*`, `ERR-*`, and `TYPE-*` entry must be entry-self-contained.
2. Related IDs may be included only for traceability.
3. Do not write "see product-spec.md for details" as a substitute for contract content.
4. Do not copy product, domain, architecture, frontend, backend, or environment source definitions.
5. Do not redefine another document's owned content.
6. Contract entries may mention related flow areas, but must not perform full flow composition.

Allowed:

```markdown
Related Requirements:
- REQ-001
Related Domain Items:
- ENT-001
```

Forbidden:

```markdown
API-001 implements REQ-001. See REQ-001 for details.
```

## Flow-Aware Contract Rules

The data/API contract must support flow-first execution without becoming an execution plan.

Required:
- Identify contracts needed by current Core User Flows and Side Effect Flows.
- Include data/API support for product-visible feedback, recovery, state transitions, artifacts, and completion signals.
- Represent generated artifacts and intermediate artifacts when they are part of current scope.
- Define terminal and non-terminal statuses when they affect API behavior or frontend/backend coordination.
- Define stable error contracts for recoverable, failed, blocked, validation, and not-found behavior when applicable.
- Keep final executable `FLOW-*`, `TASK-*`, and `VAL-*` out of this document.

Forbidden:
- Generating a final flow catalog.
- Generating frontend client responsibilities.
- Generating backend implementation responsibilities.
- Generating task order.
- Generating validation commands.

## Required Output Structure

```markdown
# Data/API Contract

## 1. Contract Scope

State what this document owns and what it does not own.

## 2. Contract Summary

Summarize the data/API model in current-scope terms.

## 3. Shared Type Catalog

### TYPE-001: <Type Name>

Kind:
- enum / object / scalar / union / identifier / status

Purpose:
- ...

Fields / Values:
| Field or Value | Type | Required? | Meaning |
|---|---|---:|---|

Rules:
- ...

Related IDs:
- ...

Flow Support:
- ...

## 4. Data Object Catalog

### DB-001: <Data Object Name>

Purpose:
- ...

Fields:
| Field | Type | Required? | Meaning |
|---|---|---:|---|

Constraints:
- ...

Lifecycle:
- ...

Related IDs:
- ...

Flow Support:
- ...

## 5. API Contract Catalog

### API-001: <API Name>

Method:
- ...

Path:
- ...

Purpose:
- ...

Request:
| Field | Type | Required? | Meaning |
|---|---|---:|---|

Response:
| Field | Type | Required? | Meaning |
|---|---|---:|---|

Status Codes:
| Code | Meaning | Error Contract |
|---:|---|---|

Errors:
- ERR-...

State / Artifact Effects:
- ...

Related IDs:
- ...

Flow Support:
- ...

Out of Scope:
- ...

## 6. Error Contract Catalog

### ERR-001: <Error Name>

Purpose:
- ...

When Returned:
- ...

Shape:
| Field | Type | Required? | Meaning |
|---|---|---:|---|

Recovery Guidance:
- ...

Related IDs:
- ...

## 7. Contract-Level Rules

List cross-contract rules that are owned by data/API contracts.

## 8. Out-of-Scope Contracts

List data/API contracts intentionally excluded from the current implementation.

## 9. Downstream Seeds

List concise seeds for frontend, backend, flow composition, execution, and validation documents.

## 10. Final Readiness

Status: ready / blocked

If blocked, list missing decisions and affected downstream documents.
```

## Entry Requirements

Each `TYPE-*` entry must include:

```text
ID
name
kind
purpose
fields or values
rules
```

Each `DB-*` entry must include:

```text
ID
name
purpose
fields
constraints
lifecycle when applicable
```

Each `API-*` entry must include:

```text
ID
name
method
path
purpose
request
response
status codes
errors
state or artifact effects when applicable
out-of-scope where useful
```

Each `ERR-*` entry must include:

```text
ID
name
purpose
when returned
shape
recovery guidance when applicable
```

## Writing Constraints

Use direct, resolved contract language.

Prefer:

```text
The create-run API returns a stable `run_id`, the initial run status, and any validation errors in the documented error envelope.
```

Avoid:

```text
The API might return whatever the backend creates.
```

Avoid dependency-only wording:

```text
See product-spec.md for request behavior.
```

Instead, state the contract behavior here and leave product intent, frontend implementation, and backend implementation to their owner documents.

## Blocked Generation Rules

Output a blocked-generation report instead of a normal data/API contract if:

- required API behavior is unresolved
- required data persistence or storage boundary is unresolved
- required artifact contract behavior is unresolved
- status lifecycle affects contracts but is unresolved
- error shape is required but undecided
- frontend/backend contract boundary is unclear
- unresolved Open Questions would enter the final contract doc

Blocked-generation report structure:

```markdown
# Data/API Contract Generation Blocked

## Blocking Issues

| Issue | Decision Needed | Affected Docs | Flow Impact |
|---|---|---|---|

## Partial Safe Content

## Required User Decisions
```

## Final Checks

Before finalizing, verify:

- No unresolved Open Questions remain.
- No frontend API client implementation is defined.
- No backend service or repository implementation is defined.
- No ORM implementation detail is defined.
- No `TASK-*`, `VAL-*`, or final executable `FLOW-*` entries are created.
- Every `DB-*`, `API-*`, `ERR-*`, and `TYPE-*` entry is self-contained.
- Related IDs are traceability hints only.
- Data/API contracts support flow-first execution without becoming a flow composition document.
