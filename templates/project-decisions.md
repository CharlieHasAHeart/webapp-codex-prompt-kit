# Project Decisions

## Purpose

Store shared canonical decisions that multiple documents need to reference.

## Source of Truth

This document is the source of truth only for shared project-wide decisions.

It does not replace:

- `prd.md` for product requirements
- `domain-model.md` for domain rules
- `tech-stack.md` for technology selection
- `db-schemas.md` for database schema
- `api-design.md` for API contracts
- `dev-environment.md` for exact commands
- `acceptance-and-validation.md` for completion criteria
- `execution-plan.md` for task order

## Codex Usage

Codex should use this document to resolve shared decisions without searching for repeated copies across files.

Codex should not use this document to invent new requirements, APIs, tables, or tasks.

## Non-Goals

This document does not contain complete product requirements, full database schemas, full API contracts, or full task plans.

## Decision Table

| ID | Decision | Value | Applies To | Notes |
|---|---|---|---|---|
| DEC-001 | Product mode | `[value]` | frontend, backend |  |
| DEC-002 | Package manager | `[value]` | dev environment |  |
| DEC-003 | API rollout policy | `[value]` | API, execution plan |  |
| DEC-004 | Database implementation scope | `[value]` | DB, execution plan |  |

## Terminology Mapping

| Product Term | Legacy/Internal Term | User-Facing? | Notes |
|---|---|---:|---|
|  |  |  |  |

## Usage Rules

- Update this file when shared canonical decisions change.
- Do not duplicate long decision blocks in other documents.
- Other documents should reference this file where possible.
