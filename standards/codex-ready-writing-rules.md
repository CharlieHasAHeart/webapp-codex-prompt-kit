# Codex-Ready Writing Rules

## Purpose

This standard defines how to write documents that are easy for Codex to execute from with minimal context.

The document system uses:

```text
reference catalogs
execution spine
task-scoped reading
container-first validation
```

Codex-ready writing should make each task executable without requiring Codex to read the entire document set.

---

## Core Rule

Write for execution, not for storytelling.

Good Codex-ready writing is:

```text
explicit
compact
heading-addressable
task-scoped
implementation-facing
source-of-truth aware
```

Avoid:

```text
long narrative
hidden assumptions
duplicated definitions
vague instructions
broad references to whole documents
requirements mixed with implementation tasks
```

---

## Heading-Addressable IDs

Every Markdown ID that may be referenced by a task must be a heading.

Use this format:

```markdown
### REQ-001: Create Case
### DEC-001: Repository Layout
### ENT-001: Case
### REL-001: Case Owns Results
### BR-001: No Concurrent Active Run
### STATE-001: Case Status
### ARCH-001: Repository Layout
### DB-001: cases
### API-001: List Cases
### ERR-001: Validation Error
### TYPE-001: Pagination Response
### FE-001: Case List Page
### BE-001: Case Query Service
### ENV-001: Container-First Command Policy
### TASK-001: Initialize Repository Structure
### VAL-001: Case List API Contract Validation
```

Do not bury IDs inside paragraphs or table cells if they need to be referenced directly.

---

## Task-Scoped Reference Style

`TASK-*` entries should reference specific sections, IDs, or YAML keys.

Good:

```text
docs/data-api-contract.md#API-001
docs/domain-model.md#BR-001
docs/backend-design.md#BE-004
docs/dev-environment.md#ENV-010
docs/ui/UI_PAGE.yaml#cases_list
```

Avoid:

```text
docs/data-api-contract.md
docs/domain-model.md
docs/backend-design.md
all UI docs
all standards
```

Full-document references should be rare and must be justified by the task.

---

## Reference Catalog Entry Style

Reference catalog entries should be short enough to read independently.

A good entry usually includes:

```text
ID
short purpose or meaning
rules or constraints
related IDs
open questions if needed
```

A good implementation catalog entry may also include:

```text
code impact
inputs
out of scope
```

Avoid entries that require reading the whole document to understand them.

---

## Use IDs Instead of Repeating Definitions

When a definition already exists, reference its ID.

Good:

```markdown
Inputs:
- API-001
- ERR-001
- BR-002
```

Avoid:

```markdown
This task should use the API that lists cases with page and status filters, and should also return validation errors in the shape defined earlier...
```

The detailed definition belongs in the owner catalog.

---

## Source-of-Truth Ownership

Write each fact in its owner document only.

Ownership examples:

```text
REQ-* -> product-spec.md
DEC-* -> project-decisions.md
ENT/REL/BR/STATE -> domain-model.md
ARCH-* -> architecture.md
DB/API/ERR/TYPE -> data-api-contract.md
FE-* -> frontend-design.md
BE-* -> backend-design.md
ENV-* -> dev-environment.md
TASK/VAL -> execution-validation.md
UI pages/routes/actions/states -> UI_PAGE.yaml
UI tokens -> UI_TOKENS.yaml
UI visual rules -> UI_VISUAL_SPEC.yaml
Codex runtime policy -> AGENTS.md
```

If another document needs the fact, it should reference the owner ID.

---

## Writing Requirements

Use direct, testable language.

Prefer:

```text
must
must not
required
forbidden
allowed
out of scope
```

Avoid:

```text
should probably
might want to
consider
maybe
as appropriate
etc.
```

Use uncertainty explicitly:

```text
unknown
provisional
deferred
open question
```

Do not hide uncertainty inside confident language.

---

## Task Writing Rules

Every implementation task in `execution-validation.md` should include:

```text
Phase
Type
Priority
Depends On
Goal
Read scope
Read before this task
Implementation Scope
Expected Code Impact
Out of Scope
Required Validation
Completion Rule
```

Task writing must avoid:

```text
broad implementation instructions without boundaries
missing validation
missing source references
full-document reads
future-scope work
silent assumptions
```

---

## Validation Writing Rules

Every required validation command should include a claim proven.

Good:

```markdown
| Command | Claim Proven |
|---|---|
| `docker compose exec api npm run test -- cases-api.test.ts` | API-001 returns paginated cases and documented errors. |
```

Avoid:

```markdown
| Command | Claim Proven |
|---|---|
| `npm test` | Tests pass. |
```

Validation should be:

```text
container-first
task-scoped
evidence-driven
minimal but meaningful
```

---

## Command Writing Rules

Commands should come from `docs/dev-environment.md`.

Use exact command patterns.

Good:

```bash
docker compose exec api npm run test -- cases-api.test.ts
```

Avoid host commands by default:

```bash
npm test
npm install
pytest
mypy
```

unless explicitly allowed by `ENV-*`.

---

## Code Impact Writing Rules

When possible, include expected code impact.

Good:

```markdown
Expected Code Impact:
- `apps/api/src/routes/cases.ts`
- `apps/api/src/services/case-query-service.ts`
- `apps/api/src/repositories/case-repository.ts`
- `apps/api/src/tests/cases-api.test.ts`
```

Rules:

- Code impact should guide Codex.
- Code impact should not over-specify every helper file.
- Code impact may be approximate when the repo is not initialized.
- If approximate, mark it as assumed.

---

## Out-of-Scope Writing Rules

Use `Out of Scope` to prevent overbuilding.

Good:

```markdown
Out of Scope:
- Do not implement export.
- Do not change API-001 response shape.
- Do not add authentication if MVP auth is deferred.
```

Every complex task should include out-of-scope boundaries.

---

## Open Question Writing Rules

Open questions should be compact and marked blocking or non-blocking.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Is authentication required in MVP? | yes | API, backend, frontend |
```

Rules:

- Use open questions for real uncertainty.
- Do not create fake certainty.
- Blocking questions should stop execution if they affect a current task.

---

## YAML Writing Rules

YAML documents should use stable keys.

For UI YAML:

```text
page IDs
route IDs
section IDs
action IDs
state IDs
token keys
visual rule keys
```

should be stable enough to reference from `TASK-*`.

Avoid:

```text
long comments
prose-heavy fields
React code
Tailwind class strings
API schemas
DB schemas
backend logic
```

---

## Anti-Patterns

Avoid these patterns:

```text
A reference catalog reads like a long essay.
A task says "read all docs first".
A task references a full document instead of a specific ID.
frontend-design.md defines API response shapes.
backend-design.md defines DB columns.
execution-validation.md redefines business rules.
dev-environment.md chooses task-specific validation.
AGENTS.md contains product requirements.
UI specs contain React code.
Validation commands are broad and do not prove a specific claim.
```

---

## Preferred Tables

Use tables for compact structured data.

Good table uses:

```text
MVP boundary
user roles
open questions
DB fields
API request params
API errors
task dependencies
task-to-validation mapping
command catalogs
review issues
```

Avoid tables when they make an entry harder to read.

---

## Minimal Narrative Rule

Narrative is allowed only when it improves execution clarity.

Keep narrative short.

Long reasoning should remain in discovery discussion or working notes, not runtime documents.

---

## Review Checklist

Before accepting a generated document, verify:

```text
[ ] IDs are heading-addressable where needed.
[ ] Entries are compact and independently readable.
[ ] Source-of-truth ownership is respected.
[ ] Definitions are referenced instead of duplicated.
[ ] Tasks reference specific sources.
[ ] Tasks include implementation scope and out-of-scope boundaries.
[ ] Validation commands have claim proven.
[ ] Commands are container-first unless explicitly allowed.
[ ] Open questions are visible.
[ ] No document requires Codex to infer missing tasks from another document.
```
