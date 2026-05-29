# Open Questions Extraction Prompt

## Standards to Apply

Read and apply only the standards listed in this section before generating this document.

Standards constrain terminology, ownership, boundaries, and quality rules for this prompt. They do not replace this prompt's target output and do not create additional output files.

### Required Standards

| Standard | Use For |
|---|---|
| `standards/flow-concepts-and-composition.md` | Identifies flow-related blockers such as unclear Core User Flows, Side Effect Flows, completion signals, intermediate artifacts, and foundation readiness. |
| `standards/open-questions-policy.md` | Defines OQ lifecycle, blocking criteria, allowed locations, question categories, and final-document leakage rules. |
| `standards/codex-ready-writing-rules.md` | Keeps extracted questions concise, durable, downstream-mappable, and safe for later resolution. |

## Standard Application Rules

Apply standards as constraints, not as additional generation targets.

Rules:
1. The current prompt defines the target output and required output structure.
2. Standards define reusable terminology, ownership boundaries, and quality rules.
3. If a standard contains examples, use them as guidance, not mandatory content.
4. Do not copy large sections from standards into the generated document.
5. Do not generate documents requested by a standard unless this prompt explicitly targets them.
6. Do not load all standards by default. Use only the standards listed in this prompt or explicitly provided by the user for this step.
7. If required context remains unresolved under the standards, output a blocked-generation report instead of inventing missing decisions.

## Priority Rule

When generating the target document, use this priority order:

1. User-confirmed answers and corrections.
2. This prompt's Target Output and Required Output Structure.
3. Required standards listed in this prompt.
4. Upstream generated project documents.
5. Prior project discussion.

If this prompt and a standard appear to conflict, follow this prompt's target output. If the conflict involves unresolved blockers, Open Questions leakage, unsafe scope invention, or missing required decisions, output a blocked-generation report instead of generating a normal document.

## Role

You are ChatGPT acting as a documentation reviewer for a Codex-ready Web App project.

Your task is to extract, normalize, deduplicate, and classify unresolved questions from the existing discussion and from `docs/review/project-design-brief.md`.

This prompt is used after technical discovery and before final reference or execution documents are generated.

Do not answer the questions.

Do not create final decisions.

Do not generate final reference catalogs.

Do not generate execution tasks.

## Target Output

Generate exactly one document:

```text
docs/review/open-questions-review.md
```

This document is a review-stage artifact.

It is not a final reference catalog.

It is not an execution document.

## Document System Context

Generated projects use:

```text
docs/
├── review/
├── reference/
└── execution/
```

This prompt writes only:

```text
docs/review/open-questions-review.md
```

Downstream prompts will later generate:

```text
docs/review/question-resolution.md
docs/review/project-decisions.md
docs/reference/*
docs/execution/*
```

## Inputs

Use these inputs when available:

```text
prior project discussion
docs/review/project-design-brief.md
uploaded project materials
repository notes
user corrections
technical preferences
UI/UX notes
source and target path notes
```

Do not require all inputs to exist. Work from available context.

## Primary Objective

Extract only unresolved questions that materially affect the current implementation pass.

Do not generate questions about speculative future capabilities, roadmap ideas, full-product evolution, or possible later versions unless the user explicitly requested future planning.

The goal is to reduce noise and preserve only questions that must be answered or consciously resolved before final documents are generated.

## Framing Rules

Use current-implementation framing.

Prefer:

```text
current implementation pass
current implementation scope
scope boundary
requested capability
completion criteria
validation criteria
```

Avoid:

```text
MVP
future scope
deferred feature
roadmap
later version
full product
```

If a possible feature is not part of the current implementation pass, do not create a question about whether it should be added later.

## Open Questions Policy

Open Questions belong only in review-stage artifacts:

```text
docs/review/open-questions-review.md
docs/review/question-resolution.md
```

They must not remain in final reference or execution documents.

Use temporary `OQ-*` IDs only in this file and in `docs/review/question-resolution.md`.

Final reference and execution documents must not contain `OQ-*` IDs.

## Question Extraction Strategy

Before extracting a question, classify the uncertainty.

Use this decision path:

```text
1. Is it already answered by prior discussion?
2. Can it be resolved by the current implementation scope?
3. Can it be resolved by a safe implementation assumption?
4. Can it be resolved by an explicit scope boundary?
5. Does it materially affect the current implementation pass?
6. Does it require a user decision before final docs are generated?
```

Only create an `OQ-*` entry for uncertainties that pass steps 5 and 6.

Do not create questions only because a complete product could eventually need that topic.

## Question Categories

Use one primary category per question.

Recommended categories:

```text
product
ux-flow
ui
domain
architecture
data-api
frontend
backend
environment
validation
execution
security
workspace
migration
```

Use `ux-flow` when the question affects:

```text
Core User Flow
Supporting Interaction Flow
Interaction Effect
System Feedback
Recovery Path
State Transition
Completion Signal
```

Use `workspace` when the question affects:

```text
multi-app workspace behavior
app dock behavior
route-per-app strategy
workspace shell persistence
workspace-level state
app-level state
```

## Blocking Classification

Classify each question as:

```text
blocking
non_blocking
```

A question is `blocking` when it prevents trustworthy final document generation or task execution.

Blocking questions usually affect:

```text
requested capability
current implementation scope
core user flow
UX recovery behavior
API contract shape
data model
system boundary
repository or target path
execution model
artifact model
validation command or validation claim
security boundary
```

A question is `non_blocking` when a safe assumption or scope boundary can be used without damaging the current implementation pass.

If a question is non-blocking, include the suggested default or boundary.

## Question Quality Rules

Each question must be:

```text
specific
answerable
current-scope relevant
mapped to affected final documents
classified by category
classified by blocking status
```

Avoid broad questions such as:

```text
What should the architecture be?
How should the UI work?
What tests should we write?
Should this be scalable?
```

Prefer specific questions such as:

```text
Should proposal generation run synchronously inside the request or through a background run executor?
Should the proposal app use a single route with local panels, or separate routes for input, run detail, and artifacts?
Should a failed generation keep partial artifacts visible to the user?
Which file types are supported in the current implementation pass?
```

## UX Flow Question Rules

UX-related questions should be extracted when they affect implementation, not visual preference alone.

Extract questions about:

```text
Core User Flow steps
required visible feedback
state transitions
recovery path after failure
disabled/pending behavior
artifact availability
run status visibility
error display and retry behavior
completion signal
```

Do not extract questions about speculative visual polish unless they affect current implementation.

Good UX flow question:

```text
After a run fails, should the UI allow retry from the same inputs, or should the user start a new run?
```

Weak question:

```text
Should the app look nicer later?
```

## Scope Boundary Rules

When uncertainty can be resolved by excluding it from the current implementation pass, do not create a blocking question unless exclusion itself needs user confirmation.

Example:

```text
Authentication is not mentioned in the requested capability.
```

Prefer classification:

```text
Resolved by Scope Boundary: Authentication is not part of the current implementation pass.
```

Do not create:

```text
OQ: Should authentication be implemented later?
```

## Deduplication Rules

When multiple notes point to the same underlying question:

```text
merge them into one OQ-*
record the source phrases or source areas
explain why they were merged
```

Do not create multiple OQ entries for the same decision.

## Output Requirements

Generate `docs/review/open-questions-review.md` with this exact top-level structure:

```markdown
# Open Questions Review

## 1. Review Scope

## 2. Extracted Questions

## 3. Blocking Questions

## 4. Non-Blocking Questions

## 5. Resolved by Prior Discussion

## 6. Resolved by Current Scope or Safe Assumption

## 7. Duplicate / Merged Questions

## 8. Affected Final Documents

## 9. Readiness Summary
```

## Section 1: Review Scope

Summarize what sources were reviewed.

Use:

```markdown
## 1. Review Scope

Reviewed Sources:
- ...

Extraction Rules:
- Only current implementation pass questions were extracted.
- Future-roadmap questions were ignored unless explicitly requested by the user.
- OQ-* IDs are temporary review-stage IDs.
```

## Section 2: Extracted Questions

Use this table:

```markdown
## 2. Extracted Questions

| ID | Category | Question | Blocking? | Suggested Default / Boundary | Affected Final Docs | Notes |
|---|---|---|---:|---|---|---|
| OQ-001 | data-api | ... | yes | ... | `docs/reference/data-api-contract.md`, `docs/execution/execution-validation.md` | ... |
```

Rules:

```text
Use OQ-001, OQ-002, OQ-003 order.
Keep questions concise.
Affected final docs must use docs/review, docs/reference, or docs/execution paths.
```

## Section 3: Blocking Questions

Use this table:

```markdown
## 3. Blocking Questions

| ID | Why Blocking | Required Human Decision | Blocks |
|---|---|---|---|
| OQ-001 | ... | ... | `docs/reference/data-api-contract.md`, `docs/execution/execution-validation.md` |
```

If there are no blocking questions, write:

```text
No blocking questions remain after extraction and reduction.
```

## Section 4: Non-Blocking Questions

Use this table:

```markdown
## 4. Non-Blocking Questions

| ID | Suggested Default / Boundary | Why Non-Blocking | Affected Docs |
|---|---|---|---|
```

If there are no non-blocking questions, write:

```text
No non-blocking questions were retained.
```

## Section 5: Resolved by Prior Discussion

List uncertainties that were considered but not extracted because the prior discussion already answered them.

Use:

```markdown
## 5. Resolved by Prior Discussion

| Topic | Resolution Found in Prior Discussion | Reason No OQ Was Created |
|---|---|---|
```

## Section 6: Resolved by Current Scope or Safe Assumption

List uncertainties that were reduced without creating OQ entries.

Use:

```markdown
## 6. Resolved by Current Scope or Safe Assumption

| Topic | Resolution Type | Resolution | Reason No OQ Was Created |
|---|---|---|---|
```

Resolution Type should be one of:

```text
current_scope
safe_assumption
scope_boundary
not_current_pass
```

## Section 7: Duplicate / Merged Questions

Use this table:

```markdown
## 7. Duplicate / Merged Questions

| Source Questions / Notes | Merged Into | Reason |
|---|---|---|
```

If no duplicate questions were merged, write:

```text
No duplicate questions were identified.
```

## Section 8: Affected Final Documents

Map questions to downstream documents.

Use:

```markdown
## 8. Affected Final Documents

| Final Document | Related Questions | Expected Absorption |
|---|---|---|
| `docs/review/project-decisions.md` | OQ-001 | Convert answer into DEC-* if project-level. |
| `docs/reference/product-spec.md` | OQ-002 | Convert answer into requirement or scope boundary. |
```

Expected absorption should describe how the answer should be converted later.

Examples:

```text
DEC-* decision
REQ-* requirement
ARCH-* boundary
API-* contract
FE-* behavior
BE-* responsibility
ENV-* command rule
TASK-* implementation task
VAL-* validation claim
UI_PAGE route/action/state
UI_VISUAL_SPEC state presentation rule
```

## Section 9: Readiness Summary

End with a readiness summary.

Use:

```markdown
## 9. Readiness Summary

Status: ready_for_resolution / no_questions_remaining / blocked_until_user_answers

Summary:
- ...

Next Step:
- Run `prompts/question-resolution-prompt.md` if any OQ-* entries exist.
- Continue to `prompts/project-decisions-prompt.md` only after blocking questions are resolved.
```

Status rules:

```text
ready_for_resolution = OQ-* entries exist and should be answered.
no_questions_remaining = no OQ-* entries remain after reduction.
blocked_until_user_answers = blocking OQ-* entries exist.
```

If there are blocking questions, use `blocked_until_user_answers`.

If there are no questions, use `no_questions_remaining`.

## Path Rules

Use only new document paths:

```text
docs/review/project-design-brief.md
docs/review/open-questions-review.md
docs/review/question-resolution.md
docs/review/project-decisions.md
docs/reference/product-spec.md
docs/reference/domain-model.md
docs/reference/architecture.md
docs/reference/data-api-contract.md
docs/reference/frontend-design.md
docs/reference/backend-design.md
docs/reference/dev-environment.md
docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
docs/execution/execution-validation.md
docs/execution/AGENTS.md
docs/execution/codex-execution-report.md
```

Do not use old flat paths such as:

```text
docs/product-spec.md
docs/project-decisions.md
docs/execution-validation.md
AGENTS.md
```

## Prohibited Output

Do not generate:

```text
docs/review/question-resolution.md
docs/review/project-decisions.md
docs/reference/*
docs/execution/*
```

Do not generate:

```text
final DEC-* entries
final REQ-* entries
final ARCH-* entries
final API-* entries
final FE-* entries
final BE-* entries
final TASK-* entries
final VAL-* entries
code
implementation plan
```

Do not answer the extracted questions.

Do not turn suggested defaults into final decisions.

## Final Self-Check

Before finalizing the output, verify:

```text
[ ] Only current implementation pass questions are extracted.
[ ] Future-roadmap questions are not extracted unless explicitly requested.
[ ] Each question is specific and answerable.
[ ] Each question has one primary category.
[ ] Each question has blocking or non-blocking status.
[ ] Each question maps to affected final documents.
[ ] Questions already answered by prior discussion are not duplicated.
[ ] Questions resolvable by current scope or safe assumption are reduced.
[ ] OQ-* IDs appear only in this review artifact.
[ ] No final decisions, requirements, contracts, tasks, or validation entries are generated.
[ ] New docs/review, docs/reference, docs/execution paths are used.
```
