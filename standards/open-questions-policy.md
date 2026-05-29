# Open Questions Policy Standard

## 1. Purpose

This standard defines how unresolved questions are discovered, classified, resolved, and converted into durable project documentation in the WebApp Codex Prompt Kit.

Open Questions are temporary review-stage artifacts. They must help ChatGPT and the user remove uncertainty before final reference and execution documents are generated.

## 2. Scope

This standard applies to:

- `prompts/open-questions-extraction-prompt.md`
- `prompts/question-resolution-prompt.md`
- `prompts/project-decisions-prompt.md`
- all final reference prompts
- `prompts/flow-composition-review-prompt.md`
- `prompts/execution-validation-prompt.md`
- `prompts/cross-document-review-prompt.md`

It governs:

- where Open Questions may appear
- how `OQ-*` IDs are used
- how blocking questions are classified
- how resolved answers are converted into durable content
- how flow-related uncertainty is handled

## 3. Core Principle

Questions are temporary. Decisions, requirements, contracts, boundaries, flows, tasks, and validation entries are durable.

Open Questions must not be left for Codex to resolve during implementation.

## 4. Allowed Locations

Open Questions may appear only in review-stage question files:

```text
docs/review/open-questions-review.md
docs/review/question-resolution.md
```

`OQ-*` IDs may be used only in those review-stage files.

## 5. Forbidden Locations

Unresolved questions must not appear in final reference or execution documents:

```text
docs/reference/*.md
docs/reference/ui/*.yaml
docs/execution/*.md
```

Final reference and execution documents must not contain unresolved markers such as:

```text
Open Questions
TBD
to be decided
unclear
unknown
ask user later
decide later
pending decision
OQ-*
```

The only exception is when these phrases are described inside this policy or inside review-stage question documents.

## 6. Open Question Lifecycle

1. Uncertainty is discovered during discovery, document generation, flow composition, or review.
2. The uncertainty is extracted into `docs/review/open-questions-review.md` as an `OQ-*` entry.
3. The question is classified by category, blocking status, affected final documents, and flow impact.
4. The user resolves the question.
5. The resolution is recorded in `docs/review/question-resolution.md`.
6. The answer is converted into durable content in downstream documents.
7. Final reference and execution documents contain the resolved content, not the question.

## 7. Question Categories

Use one primary category per question.

Recommended categories:

```text
product
flow
domain
architecture
data-api
frontend
backend
ui
environment
validation
execution
security
workspace
migration
```

Use `flow` when the uncertainty affects:

- Core User Flow
- Side Effect Flow
- Supporting Interaction Flow
- Feedback Flow
- Recovery Flow
- State Transition
- Completion Signal
- Foundation Readiness
- Execution Flow composition

## 8. Flow-Aware Extraction Rule

Open Question extraction must be flow-aware.

Do not extract only feature, page, API, architecture, or environment questions. Also identify unresolved issues that materially affect:

- what the user is trying to complete
- what side effects the system must produce
- what intermediate artifacts exist
- what feedback the user sees
- how failures are recovered
- which state transitions are valid
- what completion signal proves the flow is done
- what foundation must exist before a flow can be implemented

## 9. Blocking Question Rule

A question is blocking when it prevents trustworthy final document generation or Codex execution.

A question is usually blocking when it affects:

- current implementation scope
- Core User Flow start, path, or completion
- required Side Effect Flow
- artifact creation, availability, storage, or download
- API contract shape
- data/storage boundary
- security boundary
- runtime or command policy
- validation claim or required command
- execution flow composition
- foundation readiness before a flow slice

If a blocking question remains unresolved, final reference and execution generation must stop or output a blocked-generation report.

## 10. Non-Blocking Question Rule

A question is non-blocking only when a safe default, explicit boundary, or conservative assumption can be recorded without damaging the current implementation pass.

Non-blocking questions still require a recorded resolution. They must not remain open in final documents.

Valid outcomes:

- resolved as in current scope
- resolved as outside current scope
- converted into an explicit assumption
- converted into a project decision
- converted into a validation or review note
- absorbed into an existing flow or boundary

## 11. Flow Impact Field

`docs/review/open-questions-review.md` should include a `Flow Impact` column or equivalent notes.

Use it to identify whether the question affects:

```text
Core User Flow
Side Effect Flow
Supporting Interaction Flow
Feedback Flow
Recovery Flow
State Transition
Completion Signal
Foundation Readiness
Execution Flow Candidate
None
```

If the question has no meaningful flow impact, it may still be valid, but it should not be treated as a flow blocker.

## 12. Conversion Targets

Resolved questions must become durable content in the correct target document.

| Resolution Type | Target Document | Durable Content |
|---|---|---|
| Project-level decision | `docs/review/project-decisions.md` | `DEC-*` |
| Product behavior | `docs/reference/product-spec.md` | `REQ-*`, flow descriptions, scope boundaries |
| Domain meaning | `docs/reference/domain-model.md` | `ENT-*`, `REL-*`, `BR-*`, `STATE-*` |
| Architecture boundary | `docs/reference/architecture.md` | `ARCH-*` |
| Data/API contract | `docs/reference/data-api-contract.md` | `DB-*`, `API-*`, `ERR-*`, `TYPE-*` |
| UI structure | `docs/reference/ui/UI_PAGE.yaml` | routes, pages, sections, actions, states |
| UI tokens | `docs/reference/ui/UI_TOKENS.yaml` | semantic token definitions |
| UI visual rules | `docs/reference/ui/UI_VISUAL_SPEC.yaml` | visual/state/responsive/accessibility rules |
| Frontend responsibility | `docs/reference/frontend-design.md` | `FE-*` |
| Backend responsibility | `docs/reference/backend-design.md` | `BE-*` |
| Environment rule | `docs/reference/dev-environment.md` | `ENV-*` |
| Flow composition seed | `docs/review/flow-composition-review.md` | candidate flow, grouping, readiness, validation seed |
| Execution work | `docs/execution/execution-validation.md` | `FLOW-*`, `TASK-*`, `VAL-*` |

## 13. Review-Stage Boundaries

`docs/review/open-questions-review.md` must not answer questions.

`docs/review/question-resolution.md` records user-confirmed answers and conversion targets, but it is not the final source of truth for product, domain, API, frontend, backend, UI, or execution behavior.

Final documents must absorb the resolved answers directly.

## 14. Prompt Integration Rules

Prompts that may discover uncertainty must apply this standard.

Required usage:

- `discovery-workshop-prompt.md` should surface flow-aware uncertainties.
- `open-questions-extraction-prompt.md` should extract and classify them.
- `question-resolution-prompt.md` should map answers into durable target documents.
- final reference prompts must block or report if required answers remain missing.
- `flow-composition-review-prompt.md` must not invent unresolved flow decisions.
- `execution-validation-prompt.md` must not generate tasks that require Codex to resolve open questions.
- `cross-document-review-prompt.md` must check that no unresolved questions leaked into final documents.

## 15. Review Checklist

Use this checklist when reviewing generated documents:

- Are `OQ-*` IDs limited to review-stage question files?
- Are unresolved markers absent from final reference documents?
- Are unresolved markers absent from execution documents?
- Are blocking questions explicitly identified?
- Are flow-impacting questions classified with flow impact?
- Are resolved answers mapped to durable target documents?
- Are final documents written as answers, rules, boundaries, contracts, flows, tasks, or validation entries rather than questions?
- Does `execution-validation.md` avoid asking Codex to resolve product, flow, API, architecture, validation, or environment questions?

## 16. Forbidden Practices

Do not:

- leave unresolved questions in final reference or execution documents
- ask Codex to decide product, flow, API, architecture, validation, or environment questions
- use `OQ-*` IDs in `docs/reference/*` or `docs/execution/*`
- create broad speculative questions about future capabilities
- generate questions only because a full product might eventually need the topic
- hide uncertainty inside vague assumptions

## 17. Completion Standard

The question-resolution stage is complete only when:

- every blocking question is resolved or final generation is blocked
- every non-blocking question has a recorded default, assumption, or boundary
- every resolved answer has a target document mapping
- downstream prompts can generate final documents without unresolved questions
