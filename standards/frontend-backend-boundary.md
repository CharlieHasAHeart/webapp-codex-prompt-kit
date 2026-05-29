# Frontend / Backend Boundary Standard

## 1. Purpose

This standard defines how frontend and backend responsibilities must be separated in Codex-ready Web App documentation and implementation.

It prevents boundary drift where:

- frontend documents define backend contracts
- backend documents redefine frontend behavior
- frontend and backend independently invent incompatible API shapes
- execution tasks mix unrelated frontend and backend work
- Codex implements cross-boundary shortcuts without an explicit contract

## 2. Scope

This standard applies to:

- `docs/reference/architecture.md`
- `docs/reference/data-api-contract.md`
- `docs/reference/frontend-design.md`
- `docs/reference/backend-design.md`
- `docs/reference/ui/UI_PAGE.yaml`
- `docs/execution/execution-validation.md`
- `docs/execution/AGENTS.md`
- `docs/review/flow-composition-review.md`
- `docs/review/cross-document-review-report.md`

It also applies to prompts that generate those documents.

## 3. Core Principle

Frontend and backend communicate through documented contracts.

The primary contract owner is:

```text
docs/reference/data-api-contract.md
```

Frontend implementation consumes API, error, and shared type contracts.

Backend implementation fulfills API, error, and shared type contracts.

Neither side may invent or redefine the shared contract in its own implementation reference.

## 4. Source-of-Truth Ownership

| Area | Owner |
|---|---|
| Product behavior | `docs/reference/product-spec.md` |
| Domain entities, relationships, rules, and states | `docs/reference/domain-model.md` |
| Architecture boundaries | `docs/reference/architecture.md` |
| Data objects, API contracts, error contracts, shared types | `docs/reference/data-api-contract.md` |
| UI semantic structure | `docs/reference/ui/UI_PAGE.yaml` |
| UI tokens | `docs/reference/ui/UI_TOKENS.yaml` |
| UI visual rules | `docs/reference/ui/UI_VISUAL_SPEC.yaml` |
| Frontend implementation responsibilities | `docs/reference/frontend-design.md` |
| Backend implementation responsibilities | `docs/reference/backend-design.md` |
| Environment command patterns | `docs/reference/dev-environment.md` |
| Flow composition analysis | `docs/review/flow-composition-review.md` |
| Execution tasks and validation | `docs/execution/execution-validation.md` |
| Codex runtime policy | `docs/execution/AGENTS.md` |

## 5. Rule Catalog

### FB-RULE-001: API Contracts Belong Only to Data/API Contract

Requirement:
- API routes, request fields, response fields, status codes, error shapes, and shared contract types must be defined in `docs/reference/data-api-contract.md`.

Required:
- Frontend design references `API-*`, `ERR-*`, and `TYPE-*` entries.
- Backend design references and implements `API-*`, `ERR-*`, and `TYPE-*` entries.

Forbidden:
- Defining a new endpoint in `frontend-design.md` or `backend-design.md`.
- Copying full request/response schemas into frontend or backend design as a new source of truth.

Check:
- Every frontend or backend API reference points to an existing `API-*` entry.
- No frontend/backend document contains an unmatched request or response shape.

### FB-RULE-002: Frontend Consumes Contracts

Requirement:
- Frontend design owns how the browser implements UI behavior, state management, API client usage, error display, feedback, recovery, and accessibility.

Required:
- Reference `UI_PAGE.yaml` for page/action/state semantics.
- Reference `API-*` for calls.
- Reference `ERR-*` for error display behavior.
- Reference `TYPE-*` for contract values such as status, artifact type, or validation issue.

Forbidden:
- Defining backend service orchestration.
- Defining database fields.
- Defining API response bodies as a frontend-owned contract.
- Inventing business rules not owned by `domain-model.md`.

Check:
- Frontend entries can be implemented without redefining backend behavior.

### FB-RULE-003: Backend Fulfills Contracts

Requirement:
- Backend design owns API handler responsibilities, service orchestration, data/storage access, artifact handling, error production, security enforcement, and recovery support.

Required:
- Reference `API-*` for endpoint obligations.
- Reference `DB-*` for persistence or file-backed contracts.
- Reference `ERR-*` for error production.
- Reference `TYPE-*` for serialized contract values.
- Reference `ENT-*`, `BR-*`, and `STATE-*` for domain meaning and rules.

Forbidden:
- Defining React component behavior.
- Defining UI page structure.
- Redefining frontend state management.
- Redefining API request/response shapes.

Check:
- Backend entries can be implemented without inventing UI behavior.

### FB-RULE-004: Error Contracts Are Shared, Display and Production Are Separate

Requirement:
- Error contract shape belongs to `ERR-*` entries in `data-api-contract.md`.

Required:
- Backend design describes when and why an `ERR-*` is returned.
- Frontend design describes how each relevant `ERR-*` is displayed and recovered from.

Forbidden:
- Frontend and backend documents defining separate incompatible error payloads.

Check:
- Every user-visible error flow maps to both backend production and frontend display/recovery behavior.

### FB-RULE-005: Shared Types Must Not Drift

Requirement:
- Shared contract values must be defined as `TYPE-*` entries when used across frontend and backend.

Required:
- Frontend uses `TYPE-*` to render statuses, artifacts, validation issues, and other shared values.
- Backend serializes responses according to `TYPE-*`.

Forbidden:
- Frontend-only or backend-only enum values that conflict with the contract.

Check:
- Status values and artifact values used in UI states match `TYPE-*` exactly.

### FB-RULE-006: UI Semantics Are Not Backend Responsibilities

Requirement:
- UI routes, pages, sections, actions, and states belong to `UI_PAGE.yaml` and frontend design.

Required:
- Backend may support UI flows through APIs, status values, artifacts, and errors.
- Backend must not define page layout, navigation hierarchy, or component structure.

Forbidden:
- Backend design entries that prescribe React components, page sections, or visual layout.

Check:
- Backend references UI only as a consumer of API-supported behavior, not as a structure it owns.

### FB-RULE-007: Domain Rules Are Not Invented by Frontend or Backend

Requirement:
- Domain meaning and business rules belong to `domain-model.md`.

Required:
- Backend enforces relevant `BR-*` rules.
- Frontend reflects relevant `STATE-*` and `BR-*` outcomes through visible UI behavior.

Forbidden:
- Frontend or backend documents becoming the first source of a business rule.

Check:
- Business rules appearing in FE/BE docs trace back to `BR-*` or product requirements.

### FB-RULE-008: Flow-First Execution Must Preserve Boundaries

Requirement:
- Execution flow slices may include frontend, API, backend, data/storage, artifact, feedback, recovery, and validation work, but they must not blur ownership.

Required:
- A flow slice references the contract owner for API/data/error/type details.
- A flow slice references frontend design for browser behavior.
- A flow slice references backend design for server behavior.

Forbidden:
- A flow slice inventing contracts not present in `data-api-contract.md`.
- A flow slice mixing broad unrelated frontend and backend work without flow justification.

Check:
- Each flow slice explains why cross-layer work belongs to one executable flow.

### FB-RULE-009: Cross-Cutting Integration Tasks Must Be Explicit

Requirement:
- Cross-cutting tasks are allowed only when they validate a boundary or an end-to-end flow.

Allowed:
- API contract integration checks.
- Frontend/backend error consistency checks.
- Artifact download smoke validation.
- Recovery flow integration validation.

Forbidden:
- A vague task such as “implement backend and frontend” without a named flow or boundary reason.

Check:
- Cross-cutting tasks map to a named Execution Flow, contract boundary, or validation claim.

### FB-RULE-010: Validation Commands Belong to Execution Validation

Requirement:
- Task-specific validation selection belongs in `execution-validation.md`.
- Reusable command patterns belong in `dev-environment.md`.

Required:
- Frontend/backend design may describe what should be testable conceptually.
- `VAL-*` entries define final commands and claims proven.

Forbidden:
- Frontend or backend design choosing final task-specific validation commands.

Check:
- No FE/BE reference entry owns a validation command that should be a `VAL-*` entry.

## 6. Flow-Aware Boundary Pattern

A valid Execution Flow should preserve this separation:

| Flow Layer | Owner | Example Responsibility |
|---|---|---|
| Product goal | `product-spec.md` | User can submit source material and receive a created run. |
| Domain meaning | `domain-model.md` | A run has a lifecycle state. |
| API/data contract | `data-api-contract.md` | `API-*` accepts a create-run request and returns documented response fields. |
| UI semantics | `UI_PAGE.yaml` | Submit action, progress section, artifact section. |
| Frontend behavior | `frontend-design.md` | Submit form, show pending state, display errors, retry. |
| Backend behavior | `backend-design.md` | Validate request, create run, persist state, produce errors. |
| Execution task | `execution-validation.md` | Implement and validate the named flow slice. |

## 7. Required Practices

- Use `data-api-contract.md` as the contract bridge between frontend and backend.
- Reference IDs instead of duplicating owned definitions.
- Keep frontend and backend responsibilities independently readable.
- Use flow-first tasks to assemble cross-layer behavior only when the flow requires it.
- Preserve ownership even inside end-to-end flow slices.

## 8. Forbidden Practices

- Do not define API contracts in frontend or backend design.
- Do not define UI structure in backend design.
- Do not define backend service behavior in frontend design.
- Do not create broad “frontend + backend” tasks unless they implement or validate a named flow.
- Do not let `codex-execution-report.md` introduce new contracts, tasks, requirements, or decisions.

## 9. Prompt Integration

Prompts that generate architecture, data/API contract, frontend design, backend design, flow composition review, execution validation, AGENTS, or cross-document review should use this standard when boundary clarity affects the output.

Prompt usage rule:
- Use this standard when generating documents that reference both frontend and backend responsibilities.
- Do not load this standard for purely UI token or visual token generation unless frontend/backend boundary concerns are relevant.

## 10. Review Checklist

Use this checklist during cross-document review:

- Are API routes and payloads defined only in `data-api-contract.md`?
- Do frontend responsibilities consume rather than redefine API contracts?
- Do backend responsibilities fulfill rather than redefine API contracts?
- Are shared statuses, error codes, and artifact types traced to `TYPE-*` / `ERR-*`?
- Does every user-visible error have backend production and frontend display/recovery support?
- Do flow slice tasks preserve source-of-truth ownership while assembling cross-layer work?
- Are cross-cutting tasks tied to a named flow, boundary, or validation claim?
- Are validation commands owned by `execution-validation.md`, not FE/BE reference docs?
