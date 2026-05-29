# Architecture Prompt

## Purpose

Use this prompt to generate the architecture and boundary catalog for the current implementation.

The architecture document defines stable `ARCH-*` rules for repository boundaries, runtime boundaries, dependency direction, frontend/backend separation, data and storage boundaries, artifact boundaries, configuration boundaries, security boundaries, and flow-first support boundaries.

It is a non-UI reference catalog. It must be ownership-decoupled and entry-self-contained.

## Target Output

Generate exactly one document:

```text
docs/reference/architecture.md
```

## Standards to Apply

Read only the standards listed below.

| Standard | Required? | Use For |
|---|---:|---|
| `standards/document-responsibilities.md` | yes | Enforces non-UI reference ownership, entry self-containment, and traceability without dependency. |
| `standards/flow-concepts-and-composition.md` | yes | Ensures architecture supports Core User Flows, Side Effect Flows, Foundation Readiness, and later flow composition without creating executable `FLOW-*`. |
| `standards/frontend-backend-boundary.md` | yes | Defines the separation between frontend, backend, and data/API contracts. |
| `standards/open-questions-policy.md` | yes | Prevents unresolved questions from entering final reference docs. |
| `standards/codex-ready-writing-rules.md` | yes | Ensures stable IDs, resolved wording, and Codex-safe reference entries. |
| `standards/document-length-budgets.md` | optional | Use to keep the architecture catalog compact and boundary-focused. |

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
```

Do not require every reference document to understand this output. The generated architecture entries must be self-contained in their own architecture responsibility layer.

## Architecture Ownership

`docs/reference/architecture.md` owns:

```text
ARCH-*
repository and package boundaries
runtime boundaries
dependency direction rules
frontend/backend/API separation
data access boundaries
storage and artifact boundaries
configuration boundaries
security boundaries
integration boundaries
migration boundaries when applicable
flow-first support boundaries
```

It must not own:

```text
product requirements
domain entity definitions
database schema
API request/response payload fields
frontend component behavior
backend service implementation
environment command catalogs
execution task sequencing
validation commands
final executable FLOW-*
TASK-*
VAL-*
Open Questions
```

## Reference Decoupling Rules

Because this is a non-UI reference catalog:

1. Every `ARCH-*` entry must be entry-self-contained.
2. Related IDs may be included only for traceability.
3. Do not write "see product-spec.md for details" as a substitute for architecture content.
4. Do not copy product, domain, API, frontend, backend, or environment source definitions.
5. Do not redefine another document's owned content.
6. Architecture entries may mention related flow areas, but must not perform full flow composition.

Allowed:

```markdown
Related Requirements:
- REQ-001
```

Forbidden:

```markdown
This boundary follows REQ-001. See REQ-001 for details.
```

## Flow-Aware Architecture Rules

The architecture must support flow-first execution without becoming an execution plan.

Required:
- Identify the architecture boundaries needed to support current Core User Flows and Side Effect Flows.
- Define boundaries for system effects such as artifact creation, storage, background work, status updates, and recovery.
- Define minimum reusable boundaries that support Foundation Readiness.
- Keep architecture independent of final `FLOW-*`, `TASK-*`, and `VAL-*` assembly.
- Avoid layer-first implementation instructions such as "implement all backend before frontend."

Forbidden:
- Generating a final flow catalog.
- Generating task order.
- Generating validation commands.
- Turning P0-P10 or any phase model into the architecture structure.

## Required Output Structure

```markdown
# Architecture

## 1. Architecture Scope

State what this architecture document owns and what it does not own.

## 2. Architecture Summary

Summarize the selected architecture in current-scope terms.

## 3. Boundary Catalog

### ARCH-001: <Boundary or Rule Name>

Type:
- repository_boundary / runtime_boundary / dependency_rule / data_boundary / storage_boundary / artifact_boundary / configuration_boundary / security_boundary / integration_boundary / flow_support_boundary

Rule:
- ...

Allowed:
- ...

Forbidden:
- ...

Applies To:
- ...

Rationale:
- ...

Related IDs:
- ...

Flow Support:
- ...

Out of Scope:
- ...

## 4. Dependency Direction Rules

Describe allowed dependency directions without defining implementation tasks.

## 5. Runtime and Service Boundaries

Describe runtime separation, service ownership, and allowed communication paths.

## 6. Data, Storage, and Artifact Boundaries

Describe boundaries only. Do not define DB schema or API payloads.

## 7. Frontend / Backend / Contract Boundaries

Describe how frontend and backend relate to the data/API contract without defining that contract.

## 8. Configuration and Environment Boundaries

Describe configuration ownership at an architecture level only. Command details belong to `dev-environment.md`.

## 9. Security and Safety Boundaries

Describe access, isolation, file safety, trust boundaries, and forbidden behavior where applicable.

## 10. Flow-First Support Notes

Describe how the architecture supports flow-first implementation without defining final `FLOW-*`, `TASK-*`, or `VAL-*`.

## 11. Architecture Out-of-Scope

List architecture topics intentionally excluded from the current implementation.

## 12. Downstream Seeds

List concise seeds for downstream data/API, frontend, backend, environment, flow composition, or execution documents.

## 13. Final Readiness

Status: ready / blocked

If blocked, list missing decisions and affected downstream documents.
```

## ARCH Entry Requirements

Each `ARCH-*` entry must include:

```text
ID
title
type
rule
allowed
forbidden
applies to
rationale
out-of-scope where useful
```

Optional but useful:

```text
related IDs
flow support
security impact
migration impact
downstream seeds
```

## Writing Constraints

Use direct, resolved language.

Prefer:

```text
The backend owns artifact file access. The frontend may request artifacts only through documented API contracts.
```

Avoid:

```text
Maybe artifact access should be handled by the backend.
```

Avoid dependency-only wording:

```text
See backend-design.md for backend boundaries.
```

Instead, state the architecture boundary here and leave backend implementation details to `backend-design.md`.

## Blocked Generation Rules

Output a blocked-generation report instead of a normal architecture document if:

- a required architecture decision is unresolved
- current runtime boundaries are unclear
- frontend/backend separation cannot be determined
- storage or artifact boundaries are required but undecided
- security boundaries are required but undecided
- unresolved Open Questions would enter the final architecture doc

Blocked-generation report structure:

```markdown
# Architecture Generation Blocked

## Blocking Issues

| Issue | Decision Needed | Affected Docs | Flow Impact |
|---|---|---|---|

## Partial Safe Content

## Required User Decisions
```

## Final Checks

Before finalizing, verify:

- No unresolved Open Questions remain.
- No API payload fields are defined.
- No DB schema is defined.
- No frontend or backend implementation tasks are defined.
- No `TASK-*`, `VAL-*`, or final executable `FLOW-*` entries are created.
- Every `ARCH-*` entry is self-contained.
- Related IDs are traceability hints only.
- Architecture supports flow-first execution without becoming a flow composition document.
