# Traceability Matrix

## Purpose

Provide a compact `REQ → Domain Entity/Rule → DB → API → VAL → TASK` mapping for core MVP flows to reduce implementation drift.

## Source of Truth

This document is the source of truth only for cross-document traceability mappings.

It does not redefine requirements, domain rules, database schema, API contracts, validation criteria, or implementation tasks.

Source documents:

- Requirements: `prd.md`
- Domain entities and rules: `domain-model.md`
- Database objects: `db-schemas.md`
- API contracts: `api-design.md`
- Acceptance criteria: `acceptance-and-validation.md`
- Implementation tasks: `execution-plan.md`

## Codex Usage

Codex should use this document to:

- Find all source references for a core MVP flow.
- Avoid implementing a task without checking related REQ, domain, DB, API, VAL, and TASK references.
- Verify that each implementation task has a corresponding validation target.
- Detect missing or inconsistent mappings during document review.

Codex should not use this document to:

- Infer new requirements.
- Invent new domain entities.
- Add database tables not defined in `db-schemas.md`.
- Add APIs not defined in `api-design.md`.
- Treat a task as complete without checking `acceptance-and-validation.md`.

## Non-Goals

This document is an index. It is not a replacement for the source documents.

## MVP Traceability Matrix

| Flow | REQ | Domain Entity/Rule | DB | API | VAL | TASK |
|---|---|---|---|---|---|---|
| Example flow | REQ-001 | ENT-EXAMPLE, BR-001 | DB-EXAMPLES | API-001 | VAL-001 | TASK-001 |

## Coverage Checks

- Every MVP flow must map to at least one REQ.
- Every MVP flow must map to at least one VAL.
- Every MVP flow must map to at least one TASK.
- Every TASK that implements product behavior should appear in this matrix.
- Every API required for MVP behavior should appear in this matrix.
- Every VAL for MVP behavior should appear in this matrix.
- Missing references must be marked as `MISSING-ID`.

## Usage Rules

- Update this matrix when any REQ/API/VAL/TASK mapping changes.
- Keep mappings consistent with source documents.
- Do not use this matrix to introduce new IDs that do not exist in source documents.
