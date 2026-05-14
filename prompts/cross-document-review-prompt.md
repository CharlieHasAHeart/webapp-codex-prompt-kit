# Cross Document Review Prompt

## Target Output

```text
cross-document-review-report.md
```

This prompt does not generate a core project document by default.

It generates a review report that checks whether the reference catalogs, execution spine, and AGENTS runtime policy are consistent and ready for Codex execution.

---

## Purpose

Review the Codex-ready Web App document set before handing it to Codex.

The review should check whether:

```text
reference catalogs are compact and heading-addressable
execution-validation.md is complete enough to build a full Web App
TASK-* entries use task-scoped source references
AGENTS.md enforces execution-validation-first execution
validation is task-scoped and container-first
documents do not conflict or duplicate ownership
```

This prompt should not rewrite all documents unless the user explicitly asks.

---

## Source Context

Use the available conversation context and the generated project documents.

Review these documents when available:

```text
AGENTS.md
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
docs/ui/UI_PAGE.yaml
docs/frontend-design.md
docs/backend-design.md
docs/dev-environment.md
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
docs/execution-validation.md
codex-execution-report.md
```

If some documents are missing, report that clearly.

---

## Relevant Standards

Apply only the standards relevant to this review:

```text
standards/document-system.md
standards/document-responsibilities.md
standards/document-generation-order.md
standards/document-length-budgets.md
standards/codex-ready-writing-rules.md
standards/frontend-backend-boundary.md
standards/validation-strategy.md
standards/codex-execution-report-format.md
standards/webapp-execution-spine.md
standards/ui-authoring-strategy.md
```

For UI documents, also use:

```text
standards/ui-authoring-specs/UI_PAGE.authoring-spec.md
standards/ui-authoring-specs/UI_TOKENS.authoring-spec.md
standards/ui-authoring-specs/UI_VISUAL_SPEC.authoring-spec.md
standards/ui-authoring-specs/shadcn-tailwind-implementation-standard.md
```

Do not restate these standards in full.

---

## Output Rules

Generate only the review report.

Do not generate revised project documents unless the user asks.

Do not invent missing IDs.

Do not silently fix conflicts.

When a fix is needed, identify:

```text
affected document
affected section or ID
issue
recommended change
severity
```

---

## Required Report Structure

Use this structure:

```markdown
# Cross Document Review Report

## Summary

## Review Scope

## Overall Readiness

## Blocking Issues

## Reference Catalog Review

## Execution Spine Review

## Task-Scoped Reading Review

## Source-of-Truth Conflict Review

## Missing or Weak ID Review

## Frontend / Backend Boundary Review

## Data / API Contract Review

## UI Document Review

## Validation Review

## AGENTS Runtime Policy Review

## Document Length and Bloat Review

## Recommended Fix Order

## Final Verdict
```

---

## Review Rules

### Summary

Give a concise summary of the review result.

Include:

```text
number of blocking issues
number of high-priority issues
whether the document set is ready for Codex
whether Codex can begin implementation safely
```

---

### Review Scope

List which files were reviewed.

Recommended format:

```markdown
| File | Present? | Notes |
|---|---:|---|
| AGENTS.md | yes | reviewed |
| docs/execution-validation.md | yes | reviewed |
| docs/ui/UI_PAGE.yaml | no | UI page coverage unavailable |
```

---

### Overall Readiness

Use one of these statuses:

```text
ready
ready_with_minor_fixes
not_ready
partial_review_only
```

Explain the status in 2-5 bullets.

---

### Blocking Issues

List issues that must be fixed before Codex starts.

Recommended format:

```markdown
| Severity | Issue | Affected Files | Required Fix |
|---|---|---|---|
| blocking | TASK-014 references API-003, but API-003 is not defined. | execution-validation.md, data-api-contract.md | Define API-003 or update the task reference. |
```

Severity values:

```text
blocking
high
medium
low
```

---

## Reference Catalog Review

Check whether non-execution documents are compact reference catalogs.

Review:

```text
product-spec.md as REQ catalog
project-decisions.md as DEC catalog
domain-model.md as ENT/REL/BR/STATE catalog
architecture.md as ARCH catalog
data-api-contract.md as DB/API/ERR/TYPE catalog
frontend-design.md as FE catalog
backend-design.md as BE catalog
dev-environment.md as ENV catalog
UI YAML files as UI references
```

Check:

```text
IDs are heading-addressable
entries are independently readable
entries are compact
catalogs avoid long narrative
catalogs do not redefine other catalogs
catalogs include open questions where needed
```

Recommended format:

```markdown
| File | Catalog Role | Status | Issues |
|---|---|---|---|
| docs/data-api-contract.md | DB/API/ERR/TYPE | pass | None |
| docs/frontend-design.md | FE | partial | FE-003 lacks code impact. |
```

---

## Execution Spine Review

This is the most important review section.

Check whether `docs/execution-validation.md` is a complete execution spine.

Review:

```text
P0 Project Bootstrap
P1 Development Environment
P2 Shared Contracts and Types
P3 Data Layer
P4 Backend API Foundation
P5 Backend Feature Workflows
P6 Frontend App Shell
P7 Frontend Feature Workflows
P8 UI System and Interaction States
P9 Cross-Cutting Hardening
P10 Final Validation and Handoff
```

Check:

```text
every phase is evaluated
not-applicable phases have reasons
foundation tasks exist
product workflow tasks exist
task dependencies are practical
TASK-* coverage is enough to build a full Web App
tasks do not require Codex to infer missing engineering work
```

Recommended format:

```markdown
| Phase | Status | Coverage | Issue |
|---|---|---|---|
| P0 Project Bootstrap | required | covered | None |
| P3 Data Layer | required | partial | DB migrations are listed, but seed data task is missing. |
| P8 UI System and Interaction States | required | weak | Error/empty/permission states are not task-covered. |
```

Also list missing task categories:

```markdown
| Missing Task Category | Impact | Recommended Fix |
|---|---|---|
| API client base | Frontend pages may duplicate fetch logic. | Add a P6 task for frontend API client base. |
```

---

## Task-Scoped Reading Review

Check every `TASK-*`.

Each implementation task should include:

```text
Read scope
Read before this task
Do not read unless needed, when useful
Implementation Scope
Expected Code Impact, when possible
Out of Scope
Required Validation
Completion Rule
```

Check source references:

```text
references point to specific headings, IDs, or YAML keys
references avoid full-document reads
sources are relevant to the task
required vs optional reading is clear
```

Recommended format:

```markdown
| Task | Status | Issue | Recommended Fix |
|---|---|---|---|
| TASK-012 | pass | None | None |
| TASK-018 | partial | Reads full `docs/backend-design.md`. | Replace with `docs/backend-design.md#BE-004`. |
```

---

## Source-of-Truth Conflict Review

Check for duplicated or conflicting ownership.

Examples:

```text
frontend-design.md defines API response shapes
backend-design.md defines DB fields
execution-validation.md redefines API contracts
architecture.md defines command catalogs
dev-environment.md defines task-specific validation
UI_PAGE.yaml includes Tailwind classes
UI_TOKENS.yaml includes page structure
UI_VISUAL_SPEC.yaml includes React code
```

Recommended format:

```markdown
| Conflict | Source of Truth | Conflicting Location | Recommendation |
|---|---|---|---|
| API response shape duplicated | data-api-contract.md | frontend-design.md#FE-003 | Keep shape only in API-001 and reference it from FE-003. |
```

---

## Missing or Weak ID Review

Check ID ownership and references.

Review these IDs:

```text
REQ-*
DEC-*
ENT-*
REL-*
BR-*
STATE-*
ARCH-*
DB-*
API-*
ERR-*
TYPE-*
FE-*
BE-*
ENV-*
TASK-*
VAL-*
UI IDs
```

Report:

```text
undefined referenced IDs
IDs defined in the wrong file
duplicate IDs
orphan IDs
missing links between important IDs
weak entries that are too vague for task execution
```

Recommended format:

```markdown
| Issue | ID | Location | Recommendation |
|---|---|---|---|
| Referenced but undefined | ERR-004 | TASK-022 | Define ERR-004 or update the task reference. |
| Weak entry | BE-003 | backend-design.md | Add Code Impact and Rules. |
```

---

## Frontend / Backend Boundary Review

Check:

```text
apps/web does not import backend internals
apps/api does not import frontend code
packages/* stays app-agnostic
frontend consumes API-* contracts rather than inventing API shapes
backend implements API-* contracts rather than redefining them
database access is backend-only by default
```

Report boundary problems.

---

## Data / API Contract Review

Check:

```text
core DB objects exist for persisted data
core API endpoints exist for frontend workflows
request shapes are explicit
response shapes are explicit
error contracts are explicit
auth/permission expectations are explicit
pagination/filtering/sorting are defined where needed
sensitive data exposure rules exist where needed
DB/API entries reference related domain and requirement IDs
```

Recommended format:

```markdown
| Area | Status | Issue | Recommendation |
|---|---|---|---|
| API response shapes | pass | None | None |
| Error contracts | partial | Missing conflict error for duplicate run. | Add ERR-* and reference it from API-* and TASK-*. |
```

---

## UI Document Review

If UI docs exist, check:

```text
UI_PAGE.yaml defines semantic routes, pages, sections, actions, states
UI_TOKENS.yaml defines semantic reusable tokens
UI_VISUAL_SPEC.yaml references token names instead of raw values
UI docs do not include JSX, React hooks, backend logic, DB schema, or full Tailwind class strings
frontend-design.md consumes UI docs without duplicating them
execution-validation.md references UI docs only for UI tasks
```

If UI docs are missing, say whether that blocks implementation.

---

## Validation Review

Check:

```text
every must-priority implementation task has required validation
every validation command has a claim proven
commands are container-first
validation is task-scoped
broad checks are reserved for milestone/release validation
dev-environment.md supports commands used by execution-validation.md
codex-execution-report rules include sources read
```

Recommended format:

```markdown
| Task / VAL | Status | Issue | Recommendation |
|---|---|---|---|
| TASK-010 / VAL-004 | pass | None | None |
| TASK-021 | fail | No required validation. | Add targeted frontend test or review validation. |
```

---

## AGENTS Runtime Policy Review

Check whether `AGENTS.md` supports the execution-spine model.

It must say:

```text
Codex primary runtime docs are AGENTS.md and execution-validation.md
other docs are task-scoped reference catalogs
Codex must not read the full document set by default
Codex must not infer tasks from reference catalogs
Codex must follow TASK-* dependencies and scopes
Codex must update codex-execution-report.md
```

Recommended format:

```markdown
| Policy Area | Status | Issue | Recommendation |
|---|---|---|---|
| Primary runtime docs | pass | None | None |
| Task-scoped reading | partial | Does not forbid full-document reads. | Add explicit reading policy. |
```

---

## Document Length and Bloat Review

Check whether documents are too long, too vague, or duplicative.

Report:

```text
narrative that should be removed
sections that should be moved
duplicated definitions
source-of-truth drift
entries that should be split
catalogs that are not compact enough
```

Recommended format:

```markdown
| File | Issue | Recommendation |
|---|---|---|
| frontend-design.md | Contains long routing strategy narrative. | Convert to FE-* entries and reference UI_PAGE.yaml. |
```

---

## Recommended Fix Order

Give a practical fix order.

Example:

```markdown
1. Fix undefined ID references in execution-validation.md.
2. Add missing P0-P10 task coverage.
3. Add missing task-scoped reading references.
4. Remove duplicated API shapes from frontend/backend catalogs.
5. Update AGENTS.md reading policy.
6. Re-run this review.
```

---

## Final Verdict

End with one of:

```text
Ready for Codex.
Ready after minor fixes.
Not ready for Codex.
Partial review only because required documents are missing.
```

Include a concise explanation.

---

## Writing Rules

- Be specific.
- Prefer tables.
- Do not rewrite full documents unless asked.
- Do not invent missing IDs.
- Use severity labels.
- Keep recommendations actionable.
- Identify source-of-truth owner for each issue.
- Distinguish blocking issues from improvement suggestions.
- Focus strongly on execution spine completeness.
- Focus strongly on task-scoped reading.
- Do not include long commentary.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Missing files are listed.
[ ] Blocking issues are separated from non-blocking issues.
[ ] Reference catalog format is reviewed.
[ ] Execution spine completeness is reviewed.
[ ] P0-P10 phase coverage is reviewed.
[ ] Task-scoped reading is reviewed.
[ ] Undefined or duplicate IDs are identified.
[ ] Source-of-truth conflicts are identified.
[ ] Validation command issues are identified.
[ ] UI boundary issues are identified when UI docs exist.
[ ] AGENTS runtime policy is reviewed.
[ ] Final readiness verdict is clear.
[ ] Recommendations are actionable.
```
