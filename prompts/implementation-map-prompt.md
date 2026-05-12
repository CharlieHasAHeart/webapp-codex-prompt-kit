# Implementation Map Prompt

## Target File

```text
docs/implementation-map.md
```

## Purpose

Generate the central ID registry and implementation traceability map for a Codex-ready Web App project.

`implementation-map.md` helps Codex connect requirements, domain rules, decisions, frontend design, backend design, database objects, API contracts, UI pages, validation criteria, and implementation tasks.

It should answer:

```text
Which IDs exist?
Where is each ID defined?
Which IDs belong together for each product flow?
Which code areas are likely affected?
Which requirements do not yet have implementation coverage?
```

It should not redefine the detailed content owned by other documents.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Required upstream documents:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
docs/frontend-design.md
docs/backend-design.md
docs/dev-environment.md
docs/execution-validation.md
```

Use UI documents if they exist:

```text
docs/ui/UI_PAGE.yaml
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
```

Use `product-spec.md` for:

- `REQ-*`

Use `project-decisions.md` for:

- `DEC-*`

Use `domain-model.md` for:

- `ENT-*`
- `REL-*`
- `BR-*`

Use `data-api-contract.md` for:

- `DB-*`
- `API-*`

Use `frontend-design.md` for:

- `FE-*`

Use `backend-design.md` for:

- `BE-*`

Use `execution-validation.md` for:

- `VAL-*`
- `TASK-*`

Use UI documents for:

- UI page IDs
- section IDs
- action IDs
- state IDs

If an upstream document is unavailable, state the missing input and generate a partial map only if enough IDs exist.

---

## Relevant Standards

Apply only the standards relevant to this document:

```text
standards/document-system.md
standards/document-responsibilities.md
standards/document-length-budgets.md
standards/codex-ready-writing-rules.md
```

Do not restate these standards in the generated document.

---

## Output Rules

Generate only:

```text
docs/implementation-map.md
```

Do not generate other project documents.

Do not create new source IDs.

This file may register existing IDs, but must not invent new:

```text
REQ-*
ENT-*
REL-*
BR-*
DEC-*
FE-*
BE-*
DB-*
API-*
VAL-*
TASK-*
```

If a needed ID is missing, write:

```text
MISSING-ID
```

Do not silently create the missing ID.

Do not copy full definitions from source documents.

Use short meanings and references only.

---

## Required Document Structure

Use this structure unless the project clearly needs a small adjustment:

```markdown
# Implementation Map

## Purpose

## Source of Truth

## Codex Usage

## Non-Goals of This Document

## Map Summary

## ID Registry

## Flow Traceability Matrix

## Requirement Coverage

## API Coverage

## UI Coverage

## Validation Coverage

## Task Coverage

## Missing or Weak Links

## Assumptions

## Open Questions
```

---

## Section Rules

### Purpose

State that this document centralizes ID registration and implementation relationships.

Do not describe detailed source definitions.

---

### Source of Truth

State that this document owns:

- central ID registry
- short ID meanings
- source document references
- cross-document flow mapping
- coverage checks
- missing link detection

State that this document does not own:

- full product requirement definitions
- full domain rule definitions
- formal project decisions
- frontend design details
- backend design details
- database schema details
- API contract details
- validation instructions
- task implementation instructions
- UI YAML definitions

---

### Codex Usage

Tell Codex to use this document to:

- find all IDs related to a feature flow
- identify which source document owns each ID
- avoid implementing a requirement without checking related domain, API, UI, validation, and task links
- detect orphan IDs
- detect missing implementation or validation coverage
- navigate source documents quickly before editing code

Tell Codex to read the source document for details instead of relying only on the short meaning in this map.

---

### Non-Goals of This Document

Explicitly state that this document does not define:

- product requirements
- business rules
- API schemas
- DB schemas
- frontend implementation
- backend implementation
- validation commands
- task instructions
- UI structure

---

## Map Summary

Provide a compact summary.

Recommended format:

```markdown
This map registers project IDs and connects them across implementation flows.

Codex should use it as an index, not as a replacement for source documents.
```

---

## ID Registry

Create a central registry.

Recommended format:

```markdown
| ID | Type | Name | Short Meaning | Source Document | Code Impact |
|---|---|---|---|---|---|
| REQ-001 | requirement | Case creation | Users can create a case. | product-spec.md | frontend, API, backend, DB |
| ENT-001 | entity | Case | Assessment record. | domain-model.md | backend, DB, API, frontend |
| API-001 | API | List cases | Returns paginated case list. | data-api-contract.md | apps/api, apps/web |
```

Rules:

- Keep `Short Meaning` brief.
- `Source Document` must point to the owner document.
- `Code Impact` should be short and directional.
- Do not copy the full source definition.

Recommended ID types:

```text
requirement
decision
entity
relationship
business_rule
frontend
backend
database
api
validation
task
ui_page
ui_section
ui_action
ui_state
```

---

## Flow Traceability Matrix

Create a matrix mapping product flows to implementation.

Recommended columns:

```text
Flow | REQ | ENT/BR | DEC | FE | BE | DB | API | UI | VAL | TASK
```

Recommended format:

```markdown
| Flow | REQ | ENT/BR | DEC | FE | BE | DB | API | UI | VAL | TASK |
|---|---|---|---|---|---|---|---|---|---|---|
| Case list | REQ-004 | ENT-001, BR-002 | DEC-001 | FE-001 | BE-001 | DB-CASES | API-001 | cases-list | VAL-001 | TASK-004 |
```

Rules:

- Use existing IDs only.
- Use `MISSING-ID` when an expected mapping is absent.
- Keep each row focused on a user-visible or system-critical flow.
- Do not expand into full definitions.

---

## Requirement Coverage

Check whether each `REQ-*` has implementation coverage.

Recommended format:

```markdown
| REQ | FE | BE | DB/API | UI | VAL | TASK | Coverage Status |
|---|---|---|---|---|---|---|---|
| REQ-004 | FE-001 | BE-001 | API-001, DB-CASES | cases-list | VAL-001 | TASK-004 | covered |
```

Coverage status values:

```text
covered
partial
missing
deferred
out_of_scope
```

Rules:

- MVP `must` requirements should not be missing validation or tasks.
- Future requirements may be marked `deferred`.

---

## API Coverage

Check whether each `API-*` has consumers, backend implementation, validation, and tasks.

Recommended format:

```markdown
| API | Frontend Consumer | Backend Owner | Validation | Task | Coverage Status |
|---|---|---|---|---|---|
| API-001 | FE-001 | BE-001 | VAL-001 | TASK-004 | covered |
```

Rules:

- Read-only APIs should still have validation.
- Mutating APIs should map to backend rules and DB objects.

---

## UI Coverage

Check whether UI pages/actions/states are mapped to frontend design and tasks.

Recommended format:

```markdown
| UI ID | Type | FE | API | VAL | TASK | Coverage Status |
|---|---|---|---|---|---|---|
| cases-list | page | FE-001 | API-001 | VAL-002 | TASK-006 | covered |
```

Rules:

- UI actions that call APIs should map to `API-*`.
- UI states that represent errors should map to API error behavior when relevant.
- If UI documents are not present, state that UI coverage is unavailable.

---

## Validation Coverage

Check whether validations map to tasks and source IDs.

Recommended format:

```markdown
| VAL | Proves | Related IDs | Task | Coverage Status |
|---|---|---|---|---|
| VAL-001 | Case list API returns paginated data. | REQ-004, API-001, DB-CASES | TASK-004 | covered |
```

Rules:

- Each `VAL-*` should have at least one task.
- Each must-priority task should have at least one `VAL-*`.

---

## Task Coverage

Check whether tasks map to source IDs and validation.

Recommended format:

```markdown
| TASK | Type | References | Required VAL | Coverage Status |
|---|---|---|---|---|
| TASK-004 | api | REQ-004, API-001, BE-001, DB-CASES | VAL-001 | covered |
```

Rules:

- Tasks without source references are weak.
- Tasks without validation are weak unless they are documentation-only or explicitly deferred.

---

## Missing or Weak Links

List detected gaps.

Recommended format:

```markdown
| Issue | Severity | Affected IDs | Recommended Fix |
|---|---|---|---|
| Missing validation for case detail page. | high | REQ-005, FE-002, API-002 | Add VAL-* and update execution-validation.md. |
```

Severity values:

```text
low
medium
high
blocking
```

Use this section to make inconsistencies visible.

---

## Assumptions

List assumptions made while generating the map.

Recommended format:

```markdown
| Assumption | Impact | Confirm Later? |
|---|---|---|
| UI_PAGE.yaml is not available. | UI coverage is partial. | yes |
```

---

## Open Questions

List unresolved mapping questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Which FE item owns case export? | no | FE/TASK mapping |
```

---

## Writing Rules

- Use this document as an index and map.
- Do not redefine source content.
- Use existing IDs only.
- Use `MISSING-ID` for missing references.
- Keep short meanings brief.
- Prefer tables.
- Include coverage status.
- Identify orphan or weak links.
- Do not include validation commands unless needed as a short reference to `VAL-*`.
- Do not create new source-of-truth content.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] ID registry includes all major IDs from source documents.
[ ] Every registry row has a source document.
[ ] Flow matrix uses existing IDs only.
[ ] Missing IDs are marked as MISSING-ID.
[ ] MVP requirements have task and validation coverage.
[ ] APIs have frontend/backend/validation/task coverage where relevant.
[ ] UI coverage is included when UI docs exist.
[ ] Weak links are listed clearly.
[ ] No full definitions are copied from source documents.
[ ] No new source IDs are invented here.
```
