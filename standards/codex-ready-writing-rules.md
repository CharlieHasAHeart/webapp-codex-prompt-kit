# Codex-Ready Writing Rules

## Purpose

Define how to write project documents so Codex can implement from them with minimal guessing.

This standard applies to all generated Web App project documents.

It exists to prevent:

- vague guidance
- duplicated source-of-truth content
- non-actionable prose
- overlong documents
- unclear validation
- command ambiguity
- frontend/backend design being hidden in execution tasks
- traceability without implementation value

---

## Core Principle

```text
Write for implementation, not explanation.
```

A document is Codex-ready when Codex can answer:

```text
What should I build?
Where should I build it?
What rules must I follow?
What command proves it works?
What source document owns this decision?
```

---

## Background Rule

Only `product-spec.md` may contain broad product background.

All other documents should focus on implementation-facing content.

If a section outside `product-spec.md` only explains why the project matters, remove it or move it to `product-spec.md`.

---

## Implementation Impact Rule

Every section outside `product-spec.md` should affect at least one of:

- source code
- directory structure
- frontend routes
- frontend components
- frontend state
- backend services
- backend repositories
- database schema
- API contracts
- UI behavior
- commands
- tests
- task order
- validation evidence
- Codex execution rules

If a section does not affect any of these, it is probably not Codex-ready.

---

## Normative Language

Use direct, enforceable language.

Prefer:

- `Must`
- `Must not`
- `Required`
- `Forbidden`
- `Default`
- `Use`
- `Do not use`

Avoid:

- `maybe`
- `could`
- `consider`
- `as needed`
- `if possible`
- `etc.`
- `ideally`
- `probably`
- `nice to have`
- `clean`
- `robust`
- `user-friendly`

These words are allowed only when the document explicitly marks something as optional or future scope.

---

## Stable ID Rules

Use stable IDs for anything that must be referenced across documents.

| ID | Owner |
|---|---|
| `REQ-*` | `product-spec.md` |
| `ENT-*` | `domain-model.md` |
| `REL-*` | `domain-model.md` |
| `BR-*` | `domain-model.md` |
| `DEC-*` | `project-decisions.md` |
| `FE-*` | `frontend-design.md` |
| `BE-*` | `backend-design.md` |
| `DB-*` | `data-api-contract.md` |
| `API-*` | `data-api-contract.md` |
| `VAL-*` | `execution-validation.md` |
| `TASK-*` | `execution-validation.md` |

Rules:

- Do not reuse an ID for a different meaning.
- Do not rename an ID casually once referenced.
- Do not define the same ID in multiple documents.
- Register mapped IDs in `implementation-map.md`.
- If a referenced ID is missing, mark it as `MISSING-ID` instead of inventing it silently.

---

## Source-of-Truth Rule

Each detail should have one source of truth.

Use this pattern:

```text
Source document defines meaning.
Implementation map registers identity and relationships.
Dependent documents reference by ID.
```

Example:

```markdown
Related:
- REQ-004
- ENT-CASE
- API-002
- VAL-004
```

Do not copy the full text of `REQ-004` into every related document.

---

## Good vs Bad Writing

### Product Requirement

Bad:

```markdown
The case list should be easy to use and show useful information.
```

Good:

```markdown
REQ-004: Case List

Users must be able to view a paginated list of cases with status, owner, updated time, and latest result summary.
```

---

### Domain Rule

Bad:

```markdown
Risk runs should be handled carefully.
```

Good:

```markdown
BR-003: No Concurrent Active Run

A case must not have more than one active risk run.

Enforced by:
- backend service guard
- transaction boundary
- validation test
```

---

### Frontend Design

Bad:

```markdown
The frontend should have a clean filter experience.
```

Good:

```markdown
FE-004: Case List Filtering

Code impact:
- `app/cases/page.tsx`
- `components/cases/case-filter-bar.tsx`
- `lib/api/cases-client.ts`

Rules:
- Filter state must be stored in URL query params.
- Empty state must render when the filtered result count is zero.
- Loading state must use the shared table skeleton.
```

---

### Backend Design

Bad:

```markdown
The backend should be modular and robust.
```

Good:

```markdown
BE-006: Risk Run Service

Code impact:
- `services/risk-run-service.ts`
- `repositories/risk-run-repository.ts`

Responsibilities:
- validate case exists
- prevent duplicate active runs
- create parameter snapshot
- create risk run record
- return structured domain errors
```

---

### Validation

Bad:

```markdown
Run all checks and make sure everything works.
```

Good:

```markdown
Required validation:
- `docker compose exec backend pytest tests/api/test_risk_runs.py`

Claim proven:
- Run trigger API prevents duplicate active runs and returns structured errors.
```

---

## Code Impact Pattern

Implementation-facing sections should include code impact when possible.

Recommended format:

```markdown
## FE-004: Case List Filtering

Code impact:
- `app/cases/page.tsx`
- `components/cases/case-filter-bar.tsx`
- `lib/api/cases-client.ts`

Rules:
- Filter state must be URL-backed.
- Pagination state must be URL-backed.
- Loading state must use shared skeleton.
```

Code impact may include:

- files
- directories
- modules
- components
- services
- repositories
- routes
- tests
- migrations
- schemas

Do not force exact filenames if the project structure is not known yet. In that case, specify module ownership instead.

---

## Reference Pattern

Use references instead of duplication.

Recommended:

```markdown
References:
- REQ-004
- ENT-CASE
- FE-004
- BE-002
- DB-CASES
- API-002
- VAL-004
```

Avoid:

```markdown
This task implements the case list, which is a feature where users can browse a paginated list of cases with status, owner, updated time...
```

The full requirement belongs in `product-spec.md`.

---

## Decision Writing Pattern

Decisions should be explicit.

Recommended:

```markdown
DEC-002: Package Manager

Decision:
- Use `npm`.

Forbidden:
- pnpm
- yarn
- bun

Applies to:
- `dev-environment.md`
- `AGENTS.md`
- frontend package scripts
```

Avoid:

```markdown
npm is probably fine for now.
```

---

## Command Writing Rules

Commands must be deterministic.

Use:

```bash
docker compose exec backend pytest tests/services/test_case_service.py
```

Avoid:

```bash
run tests
```

Rules:

- Prefer container-first commands.
- Do not provide multiple equivalent commands unless one is clearly marked as canonical.
- Do not let Codex choose between `npm`, `pnpm`, `yarn`, and `bun`.
- Do not let Codex choose between host and container commands.
- Mark forbidden host commands explicitly in `dev-environment.md`.

---

## Validation Writing Rules

Validation should be task-scoped and evidence-driven.

Each required validation command should state what it proves.

Recommended table:

```markdown
| Command | Claim Proven |
|---|---|
| `docker compose exec backend pytest tests/services/test_case_service.py` | Case service enforces required business rules. |
| `docker compose exec frontend npm run test -- CaseList.test.tsx` | Case list renders loading, empty, error, and ready states. |
```

Avoid requiring broad checks for every task:

```text
full lint
full typecheck
full mypy
full build
full E2E
```

These belong to milestone or release validation unless task-specific.

---

## UI Writing Rules

UI-related project documents must respect the UI layer.

Use:

- `UI_PAGE.yaml` for page structure, routes, sections, actions, states
- `UI_TOKENS.yaml` for design tokens
- `UI_VISUAL_SPEC.yaml` for visual rules
- `frontend-design.md` for how frontend code consumes the UI specs

Do not put these in `frontend-design.md`:

- raw color values
- full token definitions
- full page DSL
- visual class strings from `UI_VISUAL_SPEC.yaml`

Do not put these in UI YAML:

- JSX
- React hooks
- API request code
- database schema
- full Tailwind class strings

---

## Implementation Map Writing Rules

`implementation-map.md` is an index and relationship map.

It should include:

- ID registry
- source document references
- short meaning
- code impact when useful
- traceability matrix

It should not include:

- full requirements
- full business rules
- full frontend design
- full backend design
- full API schemas
- full DB schemas
- full task instructions

Recommended matrix columns:

```text
Flow | REQ | ENT/BR | FE | BE | DB | API | UI | VAL | TASK
```

---

## Section Length Rules

Sections should be short and structured.

Prefer:

- tables
- short lists
- explicit IDs
- code impact blocks
- command blocks
- references

Avoid:

- long paragraphs
- repeated rationale
- copied content from other documents
- multiple examples unless they change implementation

---

## Conflict Handling Language

When documents conflict, write deterministic rules.

Recommended:

```markdown
If `data-api-contract.md` conflicts with `frontend-design.md` about API response shape, `data-api-contract.md` wins.
```

Avoid:

```markdown
Resolve conflicts reasonably.
```

---

## Optional and Future Scope

Optional items must be clearly labeled.

Use:

```markdown
Future Scope:
- SSO integration is not part of MVP.
```

or:

```markdown
Optional:
- Add keyboard shortcuts if time allows.
```

Do not mix optional items into required implementation tasks.

---

## Checklist

Before finalizing a document, check:

```text
[ ] Does each section have a clear source-of-truth purpose?
[ ] Does each implementation-facing section affect code, commands, tests, UI, data, API, tasks, or validation?
[ ] Are stable IDs used where cross-references are needed?
[ ] Are commands explicit and container-first?
[ ] Are validation commands task-scoped?
[ ] Are broad checks limited to milestone or release validation?
[ ] Are full definitions kept in source documents instead of copied everywhere?
[ ] Is implementation-map used as an index, not a definition dump?
[ ] Is product background limited to product-spec.md?
[ ] Is vague language removed?
```

---

## Final Rule

A Codex-ready sentence should reduce guessing.

If a sentence does not reduce guessing, delete it or rewrite it into a rule, reference, command, code impact, validation claim, or task.
