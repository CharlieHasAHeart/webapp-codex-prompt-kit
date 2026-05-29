# Document System Standard

## 1. Purpose

This standard defines the document system for the WebApp Codex Prompt Kit.

It explains the generated project document layers, their intended roles, and the current generation model.

The system is organized so that ChatGPT can generate stable planning and reference documents, and Codex can later execute implementation tasks without inventing missing requirements, UI behavior, API contracts, or validation rules.

## 2. Core Document Layers

The generated project documents are organized into three major layers:

```text
docs/review/
docs/reference/
docs/execution/
```

Their roles are:

```text
review     -> connect, analyze, resolve, compose, and prepare
reference  -> define stable source catalogs and UI reference intent
execution  -> instruct Codex what to do, what to read, and how to validate
```

Review documents may connect multiple documents.

Reference documents define source-of-truth catalogs.

Execution documents define Codex task execution, validation, runtime policy, and worklog behavior.

## 3. Review Layer

Review documents live under:

```text
docs/review/
```

They are used before final reference and execution documents are generated.

Expected review documents:

```text
docs/review/project-design-brief.md
docs/review/open-questions-review.md
docs/review/question-resolution.md
docs/review/project-decisions.md
docs/review/flow-composition-review.md
docs/review/cross-document-review-report.md
```

## 4. Review Layer Responsibilities

Review documents may:

```text
summarize project context
extract unresolved questions
record user-confirmed answers
record project-level decisions
connect source documents
analyze flow composition
check whether UI surfaces support user-visible flows
report cross-document inconsistencies
```

Review documents must not:

```text
define final API contracts
define final database schema
define final frontend/backend implementation responsibilities
define final UI YAML source content
define final TASK-* or VAL-* entries
act as Codex runtime source of truth
```

## 5. Reference Layer

Reference documents live under:

```text
docs/reference/
```

They define source-of-truth catalogs for product, domain, architecture, contracts, implementation responsibilities, environment policy, and UI references.

Expected non-UI reference documents:

```text
docs/reference/product-spec.md
docs/reference/domain-model.md
docs/reference/architecture.md
docs/reference/data-api-contract.md
docs/reference/frontend-design.md
docs/reference/backend-design.md
docs/reference/dev-environment.md
```

Expected UI reference documents:

```text
docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

## 6. Non-UI Reference Responsibilities

Non-UI reference documents define stable ownership-decoupled source catalogs.

Ownership model:

```text
product-spec.md        -> REQ-*
domain-model.md        -> ENT-*, REL-*, BR-*, STATE-*
architecture.md        -> ARCH-*
data-api-contract.md   -> DB-*, API-*, ERR-*, TYPE-*
frontend-design.md     -> FE-*
backend-design.md      -> BE-*
dev-environment.md     -> ENV-*
```

Non-UI reference documents must be:

```text
ownership-decoupled
entry-self-contained
traceable without dependency
free of unresolved Open Questions
free of TASK-* and VAL-* entries
```

## 7. UI Reference Responsibilities

The UI reference system defines the user-visible shape of the Web App.

UI references are generated after non-UI reference catalogs and before flow composition.

Expected UI files:

```text
docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

Their responsibilities are:

```text
UI_PAGE.yaml
= flow-facing semantic UI surface

UI_TOKENS.yaml
= technology-agnostic design token reference

UI_VISUAL_SPEC.yaml
= visual and interaction presentation rules
```

UI references must describe:

```text
what the user can see
what the user can do
how actions trigger effects
how system status becomes visible
how failure and blocked states are shown
how recovery is offered
where artifacts appear
where completion becomes visible
```

UI references must not define:

```text
React or JSX code
className strings
Tailwind mappings
shadcn/ui requirements
CSS variable mappings
MUI or Chakra mappings
API request/response shapes
backend behavior
database schema
final TASK-* or VAL-* entries
```

## 8. UI Technology-Agnostic Rule

The current UI reference system is technology-agnostic.

It does not assume:

```text
Tailwind
shadcn/ui
CSS variables
MUI
Chakra
CSS Modules
Styled Components
Vanilla Extract
plain CSS
```

The file previously used as a concrete implementation standard:

```text
shadcn-tailwind-implementation-standard.md
```

is removed from the current active UI document system.

A future technology-specific UI implementation standard may be introduced later, but it is not part of the current system.

## 9. UI Codex Consumption Rule

Every generated UI YAML file must contain:

```yaml
codex_consumption:
```

This section is a compact runtime dictionary for Codex.

It must explain:

```text
file role
source-of-truth content
traceability-only content
what Codex should do
what Codex must not do
which files should be read together
```

Codex must read `codex_consumption` before implementing UI tasks that reference a UI YAML file.

This avoids requiring Codex to infer UI field meaning from field names alone.

## 10. Execution Layer

Execution documents live under:

```text
docs/execution/
```

Expected execution documents:

```text
docs/execution/execution-validation.md
docs/execution/AGENTS.md
docs/execution/codex-execution-report.md
```

`codex-execution-report.md` is not normally generated by ChatGPT.

It is a runtime worklog created and maintained by Codex according to `AGENTS.md`.

## 11. Execution Layer Responsibilities

`execution-validation.md` owns:

```text
final executable FLOW-* entries when used
TASK-* entries
VAL-* entries
minimal foundation tasks
flow slice task catalog
cross-flow hardening task catalog
task-scoped source references
UI task-scoped source references
flow-level validation
UI-level validation expectations
task-to-validation mapping
release validation
execution readiness
```

`AGENTS.md` owns:

```text
Codex runtime reading policy
flow-first execution policy
task execution policy
UI reference consumption policy
validation policy
blocker policy
runtime worklog policy
forbidden behavior policy
```

`codex-execution-report.md` owns:

```text
runtime task attempt notes
sources read
UI sources read when applicable
files changed
validation results
blockers
failed validation details
chronological notes
```

Execution documents must not redefine product, domain, architecture, API, frontend, backend, environment, or UI source content.

## 12. Current Generation Order

The current recommended generation order is:

```text
1. prompts/discovery-workshop-prompt.md
   -> docs/review/project-design-brief.md

2. prompts/open-questions-extraction-prompt.md
   -> docs/review/open-questions-review.md

3. prompts/question-resolution-prompt.md
   -> docs/review/question-resolution.md

4. prompts/project-decisions-prompt.md
   -> docs/review/project-decisions.md

5. prompts/product-spec-prompt.md
   -> docs/reference/product-spec.md

6. prompts/domain-model-prompt.md
   -> docs/reference/domain-model.md

7. prompts/architecture-prompt.md
   -> docs/reference/architecture.md

8. prompts/data-api-contract-prompt.md
   -> docs/reference/data-api-contract.md

9. prompts/frontend-design-prompt.md
   -> docs/reference/frontend-design.md

10. prompts/backend-design-prompt.md
    -> docs/reference/backend-design.md

11. prompts/dev-environment-prompt.md
    -> docs/reference/dev-environment.md

12. prompts/ui-page-prompt.md
    -> docs/reference/ui/UI_PAGE.yaml

13. prompts/ui-tokens-prompt.md
    -> docs/reference/ui/UI_TOKENS.yaml

14. prompts/ui-visual-spec-prompt.md
    -> docs/reference/ui/UI_VISUAL_SPEC.yaml

15. prompts/flow-composition-review-prompt.md
    -> docs/review/flow-composition-review.md

16. prompts/execution-validation-prompt.md
    -> docs/execution/execution-validation.md

17. prompts/AGENTS-prompt.md
    -> docs/execution/AGENTS.md

18. prompts/cross-document-review-prompt.md
    -> docs/review/cross-document-review-report.md
```

## 13. Why UI Comes Before Flow Composition

UI references are generated after non-UI reference catalogs because UI should not invent product behavior, API contracts, domain states, or frontend/backend responsibilities.

UI references are generated before flow composition because flow composition must check whether user-visible flows are:

```text
operable
observable
recoverable
artifact-aware
completion-visible
```

Flow composition should use UI references to check:

```text
Does each Core User Flow have a visible UI surface?
Does each primary action have an affordance?
Do pending/running/failed/blocked states have feedback?
Does recovery have a visible path?
Does an in-scope artifact have a surface?
Is completion visible to the user?
```

## 14. Runtime Worklog Policy

The previous separate report-format document is not part of the active document system.

Do not require:

```text
codex-execution-report-format.md
```

The active rule is:

```text
AGENTS.md tells Codex when and how to create/update codex-execution-report.md.
```

`codex-execution-report.md` is a factual runtime worklog, not a source-of-truth document.

## 15. Cross-Document Review Role

`cross-document-review-report.md` is a review report.

It may identify inconsistencies, blockers, missing UI flow surfaces, missing validation, ownership violations, and execution risks.

It must not silently rewrite source documents.

It should name the recommended owner file for each issue.

## 16. Active UI Standards

The active UI standard files are:

```text
standards/ui-reference-system.md
standards/ui-authoring-specs/UI_PAGE.yaml-Authoring-Specification.md
standards/ui-authoring-specs/UI_TOKENS.yaml-Authoring-Specification.md
standards/ui-authoring-specs/UI_VISUAL_SPEC.yaml-Authoring-Specification.md
```

The current UI standards do not include a concrete implementation stack standard.

## 17. Active Prompt Groups

Prompt groups:

```text
review prompts
non-UI reference prompts
UI reference prompts
execution prompts
review/check prompts
```

UI reference prompts:

```text
prompts/ui-page-prompt.md
prompts/ui-tokens-prompt.md
prompts/ui-visual-spec-prompt.md
```

Execution prompts:

```text
prompts/execution-validation-prompt.md
prompts/AGENTS-prompt.md
```

Final review prompt:

```text
prompts/cross-document-review-prompt.md
```

## 18. Ownership Safety Summary

The system is safe when:

```text
review docs connect but do not define final source content
non-UI reference docs define their own ID families
UI reference docs define technology-agnostic UI intent
execution docs define task and validation behavior
AGENTS.md defines runtime rules
codex-execution-report.md records runtime work only
```

The system is unsafe when:

```text
reference docs depend on each other for meaning
UI files define technology-specific implementation
execution docs redefine source content
Codex is asked to infer tasks from reference catalogs
Codex is asked to infer UI field meanings without codex_consumption
Open Questions leak into final reference, UI, or execution docs
```

## 19. Final Rule

The document system must support this flow:

```text
discover decisions
resolve questions
define non-UI references
define UI references
compose flows
generate execution tasks
run Codex with task-scoped reading
validate claims
record runtime work
review consistency
```

Each document must make the next step safer, not broader.
