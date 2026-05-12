# Document Length Budgets

## Purpose

Define practical length budgets for Codex-ready Web App documents.

This standard exists to prevent:

- oversized documents
- repeated source-of-truth content
- execution plans becoming design documents
- implementation maps becoming full specifications
- Codex wasting context on low-value narrative

Length budgets are not cosmetic rules. They are implementation-quality rules.

A document should be long enough to remove ambiguity, but short enough for Codex to find and use the important parts.

---

## Core Principle

```text
Compact does not mean vague.
Detailed does not mean long.
```

Each document should prioritize:

- decisions over explanation
- tables over long prose
- references over duplication
- code impact over narrative
- validation evidence over generic quality claims

---

## Recommended Core Document Budgets

| File | Target Length | Hard Limit | Notes |
|---|---:|---:|---|
| `product-spec.md` | 6-10 pages | 14 pages | Only broad background document. Requirements should still be concise. |
| `domain-model.md` | 5-8 pages | 10 pages | Domain entities, rules, states, and invariants. No DB/API detail. |
| `project-decisions.md` | 2-4 pages | 6 pages | Shared decisions only. Avoid becoming a second architecture doc. |
| `architecture.md` | 5-8 pages | 10 pages | System boundaries and dependency direction. No FE/BE internal detail. |
| `frontend-design.md` | 5-8 pages | 10 pages | Frontend implementation design. Include code impact. |
| `backend-design.md` | 5-8 pages | 10 pages | Backend implementation design. Include code impact. |
| `data-api-contract.md` | 8-14 pages | 18 pages | DB and API are combined, so this may be the longest document. |
| `dev-environment.md` | 4-7 pages | 9 pages | Commands should be explicit, not exhaustive. |
| `execution-validation.md` | 6-10 pages | 14 pages | Tasks and validation only. Do not hide FE/BE design here. |
| `implementation-map.md` | 4-8 pages | 12 pages | ID registry and matrix only. Do not copy full definitions. |
| `AGENTS.md` | 3-6 pages | 8 pages | Codex rules only. No product or design longform. |
| `codex-execution-report.md` | 1-3 pages | 5 pages | Runtime report. Keep minimal. |

---

## UI Document Budgets

UI YAML files are separate from the core engineering document count.

| File | Target Length | Hard Limit | Notes |
|---|---:|---:|---|
| `UI_PAGE.yaml` | 100-350 lines | 600 lines | Semantic page structure only. |
| `UI_TOKENS.yaml` | 120-350 lines | 600 lines | Reusable tokens only. |
| `UI_VISUAL_SPEC.yaml` | 180-500 lines | 800 lines | Visual usage rules only. |

The UI authoring specs in `standards/ui-authoring-specs/` may be longer because they are standards, not generated project documents.

---

## Compact Mode Budgets

When using 10-document compact mode, `implementation-map.md` is embedded into `execution-validation.md`.

In compact mode:

| File | Target Length | Hard Limit |
|---|---:|---:|
| `execution-validation.md` with embedded implementation map | 8-12 pages | 16 pages |

Use compact mode only if the embedded ID registry and traceability matrix remain short.

If `execution-validation.md` exceeds the hard limit, split out:

```text
implementation-map.md
```

---

## What Counts as Bloat

A document is bloated when it contains:

- repeated product background outside `product-spec.md`
- repeated decisions already in `project-decisions.md`
- repeated ID relationships that belong in `implementation-map.md`
- frontend implementation details outside `frontend-design.md`
- backend implementation details outside `backend-design.md`
- DB/API details outside `data-api-contract.md`
- command catalogs outside `dev-environment.md`
- long motivational explanations
- multiple paragraphs that do not affect implementation or validation

---

## Compression Rules

If a document exceeds its hard limit, apply these steps in order.

### 1. Remove Narrative

Remove explanations that do not change implementation.

Bad:

```markdown
This feature is important because users need a smooth and intuitive experience when browsing cases.
```

Better:

```markdown
REQ-004: Users must be able to browse cases with pagination, status, owner, updated time, and latest result summary.
```

### 2. Convert Prose to Tables

Bad:

```markdown
The case list page should include a header, filters, a table, empty state, loading state, and error state.
```

Better:

```markdown
| Section | Purpose |
|---|---|
| header | Page title and primary action |
| filters | Route-backed query controls |
| cases_table | Paginated case records |
| empty_state | No cases or no matching filters |
```

### 3. Move Shared Decisions

If a decision appears in more than one document, move it to:

```text
project-decisions.md
```

Other documents should reference the decision ID.

### 4. Move Relationships

If a section mainly says how IDs relate to each other, move it to:

```text
implementation-map.md
```

### 5. Replace Copies with References

Do not copy full API, DB, UI, or validation definitions across documents.

Use references:

```markdown
Related:
- API-004
- DB-CASE-PARAMETER-VALUES
- VAL-006
```

### 6. Split Only When Necessary

Do not split documents just because they are important.

Split only when a document has multiple source-of-truth areas that cannot remain clear together.

---

## Minimum Useful Detail

Do not reduce documents below usefulness.

A document is too short when Codex still has to guess:

- which files to modify
- which framework or package manager to use
- which service owns a rule
- which endpoint owns a response
- which validation command proves completion
- which UI state belongs in URL vs local state
- which task implements a requirement

Short but vague is worse than slightly longer and executable.

---

## Per-Document Budget Guidance

## `product-spec.md`

Allowed to include background, but background should still be purposeful.

Prioritize:

- `REQ-*`
- MVP scope
- non-goals
- roles
- user stories
- success criteria

Avoid:

- long market context
- implementation ideas
- UI layout details
- API/DB details

---

## `domain-model.md`

Prioritize:

- `ENT-*`
- `REL-*`
- `BR-*`
- state machines
- invariants

Avoid:

- storage schema
- API payloads
- frontend state
- task lists

---

## `project-decisions.md`

Prioritize:

- decision table
- value
- applies to
- forbidden alternatives

Avoid:

- rationale longer than one short paragraph per decision
- product requirement restatement
- architecture duplication

---

## `architecture.md`

Prioritize:

- boundaries
- dependency direction
- request lifecycle
- auth/error boundary
- top-level directory structure

Avoid:

- frontend page details
- backend service method details
- DB/API schemas

---

## `frontend-design.md`

Prioritize:

- `FE-*`
- code impact
- routing
- components
- state management
- API client
- form handling
- UI document consumption

Avoid:

- duplicating `UI_PAGE.yaml`
- raw token values
- backend service rules

---

## `backend-design.md`

Prioritize:

- `BE-*`
- code impact
- services
- repositories
- transactions
- permissions
- validation placement
- jobs and integrations

Avoid:

- DB field tables
- API response schemas
- frontend details

---

## `data-api-contract.md`

Prioritize:

- `DB-*`
- `API-*`
- schemas
- constraints
- request/response
- error envelope
- permissions
- DB/API mapping

Avoid:

- backend service implementation
- frontend client implementation
- product narrative

---

## `dev-environment.md`

Prioritize:

- container-first commands
- service names
- package managers
- allowed commands
- forbidden host commands
- task/milestone/release command patterns

Avoid:

- every possible command
- unrelated tool documentation
- product narrative

---

## `execution-validation.md`

Prioritize:

- `TASK-*`
- `VAL-*`
- task dependencies
- expected code impact
- required validation
- claim proven

Avoid:

- frontend/backend design detail
- full API/DB contract
- full command catalog
- broad "run everything" validation

---

## `implementation-map.md`

Prioritize:

- ID registry
- short meaning
- source document
- code impact when useful
- traceability matrix
- coverage checks

Avoid:

- full requirement text
- full business rules
- full API schema
- full DB schema
- full task definitions

---

## `AGENTS.md`

Prioritize:

- reading order
- source-of-truth hierarchy
- command rules
- validation rules
- conflict handling
- report format

Avoid:

- product background
- design details
- long examples
- duplicated document content

---

## Final Rule

A generated document should be:

```text
short enough to scan,
specific enough to execute,
structured enough to verify.
```
