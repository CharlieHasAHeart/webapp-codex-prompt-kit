# Document Generation Order Standard

## 1. Purpose

This standard defines the recommended generation order for the WebApp Codex Prompt Kit.

The order is designed to help ChatGPT move from discovery to stable references, then to UI references, then to flow-first Codex execution documents.

It prevents:

```text
premature execution planning
unresolved Open Questions leaking into final docs
UI references inventing product/API/frontend/backend behavior
Codex inferring tasks from reference catalogs
layer-first implementation planning
technology-specific UI assumptions before an implementation stack is selected
```

## 2. Core Ordering Principle

Generate documents in this order:

```text
review discovery
→ question resolution
→ project decisions
→ non-UI reference catalogs
→ UI reference catalogs
→ flow composition review
→ execution validation
→ AGENTS runtime policy
→ cross-document review
```

The reason is:

```text
review docs clarify what is known
reference docs define stable source content
UI references define the user-visible shape
flow composition connects references into executable candidates
execution documents define Codex tasks and validation
AGENTS defines Codex runtime behavior
cross-document review checks consistency and readiness
```

## 3. Full Recommended Generation Order

### 1. Discovery Workshop

Prompt:

```text
prompts/discovery-workshop-prompt.md
```

Output:

```text
docs/review/project-design-brief.md
```

Role:
- Captures initial project context, user goals, constraints, rough flows, artifacts, risks, and raw design signals.

Must not:
- Create final requirements.
- Create final API contracts.
- Create final UI YAML.
- Create execution tasks.

---

### 2. Open Questions Extraction

Prompt:

```text
prompts/open-questions-extraction-prompt.md
```

Output:

```text
docs/review/open-questions-review.md
```

Role:
- Extracts unresolved decisions as temporary `OQ-*`.
- Classifies blockers and affected future documents.
- Identifies flow, UI, API, architecture, and execution risks.

Must not:
- Resolve questions without user input.
- Generate final reference entries.
- Generate execution tasks.

---

### 3. Question Resolution

Prompt:

```text
prompts/question-resolution-prompt.md
```

Output:

```text
docs/review/question-resolution.md
```

Role:
- Records user-confirmed answers.
- Maps each answer to its target owner document.
- Converts raw questions into durable decisions or reference inputs.

Must not:
- Leave unresolved questions as final content.
- Generate final reference docs by itself.

---

### 4. Project Decisions

Prompt:

```text
prompts/project-decisions-prompt.md
```

Output:

```text
docs/review/project-decisions.md
```

Role:
- Records `DEC-*` project-level decisions.
- Preserves accepted decisions, rejected alternatives, and cross-document implications.

Must not:
- Replace reference catalogs.
- Define executable tasks.

---

## 4. Non-UI Reference Generation Order

Generate non-UI references in this order:

```text
product-spec
domain-model
architecture
data-api-contract
frontend-design
backend-design
dev-environment
```

This order lets later references consume earlier context without redefining earlier owners.

---

### 5. Product Spec

Prompt:

```text
prompts/product-spec-prompt.md
```

Output:

```text
docs/reference/product-spec.md
```

Role:
- Defines `REQ-*`, product scope, product-facing flows, feedback/recovery expectations, artifacts, and completion signals.

Must not:
- Define domain schema.
- Define API payloads.
- Define UI page structure.
- Define frontend/backend implementation.
- Define execution tasks.

---

### 6. Domain Model

Prompt:

```text
prompts/domain-model-prompt.md
```

Output:

```text
docs/reference/domain-model.md
```

Role:
- Defines `ENT-*`, `REL-*`, `BR-*`, and `STATE-*`.

Must not:
- Define DB schema as source of truth.
- Define API payloads.
- Define UI structure.
- Define execution tasks.

---

### 7. Architecture

Prompt:

```text
prompts/architecture-prompt.md
```

Output:

```text
docs/reference/architecture.md
```

Role:
- Defines `ARCH-*`, system boundaries, dependency direction, runtime/storage/security rules, and architectural constraints.

Must not:
- Define API request/response fields.
- Define UI structures.
- Define frontend/backend task plans.

---

### 8. Data and API Contract

Prompt:

```text
prompts/data-api-contract-prompt.md
```

Output:

```text
docs/reference/data-api-contract.md
```

Role:
- Defines `DB-*`, `API-*`, `ERR-*`, and `TYPE-*`.

Must not:
- Define frontend UI behavior.
- Define backend service implementation.
- Define Codex tasks.

---

### 9. Frontend Design

Prompt:

```text
prompts/frontend-design-prompt.md
```

Output:

```text
docs/reference/frontend-design.md
```

Role:
- Defines `FE-*` frontend implementation responsibilities.
- Describes how frontend implementation consumes UI references when available.
- Defines frontend state, API client, feedback, recovery, artifact, and accessibility responsibilities.

Must not:
- Define UI_PAGE source structure.
- Define UI_TOKENS source tokens.
- Define UI_VISUAL_SPEC presentation rules.
- Define API request/response fields.
- Define backend behavior.
- Define concrete styling-stack rules unless a future project-specific implementation standard explicitly establishes them.

---

### 10. Backend Design

Prompt:

```text
prompts/backend-design-prompt.md
```

Output:

```text
docs/reference/backend-design.md
```

Role:
- Defines `BE-*` backend implementation responsibilities.

Must not:
- Define API contracts as source definitions.
- Define DB schema as source definitions.
- Define frontend/UI behavior.
- Define execution tasks.

---

### 11. Dev Environment

Prompt:

```text
prompts/dev-environment-prompt.md
```

Output:

```text
docs/reference/dev-environment.md
```

Role:
- Defines `ENV-*`, runtime/package/service policy, command patterns, environment variable policy, and command safety.

Must not:
- Select task-specific validation.
- Define product behavior.
- Define UI implementation standards.

---

## 5. UI Reference Generation Order

Generate UI references after non-UI references and before flow composition.

Order:

```text
ui-page
ui-tokens
ui-visual-spec
```

Reason:

```text
UI_PAGE.yaml uses product, API, and frontend context to define what the user can see and do.
UI_TOKENS.yaml uses product/UI context to define technology-agnostic visual token intent.
UI_VISUAL_SPEC.yaml uses UI_PAGE and UI_TOKENS to define presentation intent.
flow-composition-review then checks whether user-visible flows are operable, observable, recoverable, artifact-aware, and completion-visible.
```

---

### 12. UI Page

Prompt:

```text
prompts/ui-page-prompt.md
```

Output:

```text
docs/reference/ui/UI_PAGE.yaml
```

Role:
- Defines flow-facing semantic UI surface.
- Defines app shell, navigation, routes, pages, sections, actions, states, local UI state, flow surface mapping, feedback mapping, recovery paths, artifact surfaces, and completion signals.

Must include:

```yaml
codex_consumption:
```

Must not:
- Define visual token values.
- Define CSS variables.
- Define Tailwind classes.
- Define React or JSX.
- Define API request/response shapes.
- Define backend behavior.
- Assume a concrete styling stack.

---

### 13. UI Tokens

Prompt:

```text
prompts/ui-tokens-prompt.md
```

Output:

```text
docs/reference/ui/UI_TOKENS.yaml
```

Role:
- Defines technology-agnostic reusable UI token intent.
- Defines semantic color roles, typography, spacing, radius, border, shadow, layout, breakpoint, motion, z-index, status roles, and accessibility token intent.

Must include:

```yaml
codex_consumption:
```

Must not:
- Define CSS variable names.
- Define Tailwind mappings.
- Define shadcn compatibility.
- Define MUI or Chakra mappings.
- Define className strings.
- Define component implementation.
- Define page structure.
- Assume a concrete styling stack.

---

### 14. UI Visual Spec

Prompt:

```text
prompts/ui-visual-spec-prompt.md
```

Output:

```text
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

Role:
- Defines visual and interaction presentation rules.
- Defines layout presentation, surface hierarchy, component visual roles, state presentation, feedback, recovery, artifact presentation, completion signal presentation, responsive behavior, accessibility, density, and status mapping.

Must include:

```yaml
codex_consumption:
```

Must not:
- Define routes.
- Define page section source structure.
- Duplicate raw token values.
- Define CSS variables.
- Define Tailwind classes.
- Define React or JSX.
- Assume a concrete styling stack.

---

## 6. Flow Composition and Execution Order

### 15. Flow Composition Review

Prompt:

```text
prompts/flow-composition-review-prompt.md
```

Output:

```text
docs/review/flow-composition-review.md
```

Role:
- Converts reference catalogs and UI references into candidate execution flow analysis.
- Checks Core User Flows, Side Effect Flows, feedback flows, recovery flows, artifact flows, blocked flows, UI surfaces, action affordances, feedback states, recovery paths, artifact surfaces, completion signals, and foundation readiness.
- Produces task seeds and validation seeds.

Must not:
- Generate final `FLOW-*`.
- Generate final `TASK-*`.
- Generate final `VAL-*`.
- Redefine reference or UI source content.

---

### 16. Execution Validation

Prompt:

```text
prompts/execution-validation-prompt.md
```

Output:

```text
docs/execution/execution-validation.md
```

Role:
- Defines final executable `FLOW-*` where used, `TASK-*`, `VAL-*`, flow-first sequencing, validation mapping, UI-level validation, and release validation.
- Provides task-scoped source references.
- Requires UI tasks to read relevant UI YAML `codex_consumption`.

Must not:
- Redefine product, API, frontend, backend, environment, or UI source content.
- Ask Codex to infer tasks from reference catalogs or UI YAML files.
- Use P0-P10 as the top-level execution structure.

---

### 17. AGENTS Runtime Policy

Prompt:

```text
prompts/AGENTS-prompt.md
```

Output:

```text
docs/execution/AGENTS.md
```

Role:
- Defines Codex runtime policy, reading policy, flow-first execution policy, UI reference consumption policy, validation-before-completion policy, blocker policy, and worklog policy.

Must not:
- Define `TASK-*` or `VAL-*`.
- Redefine reference or UI source content.
- Treat `codex-execution-report.md` as source of truth.

---

### 18. Cross-Document Review

Prompt:

```text
prompts/cross-document-review-prompt.md
```

Output:

```text
docs/review/cross-document-review-report.md
```

Role:
- Reviews the generated document set for consistency, readiness, ownership, UI coverage, flow continuity, validation quality, runtime policy, and Open Question leakage.

Must not:
- Silently rewrite source documents.
- Generate final task or validation entries.

---

## 7. Active UI Standards

The active UI standards in the current system are:

```text
standards/ui-reference-system.md
standards/ui-authoring-specs/UI_PAGE.yaml-Authoring-Specification.md
standards/ui-authoring-specs/UI_TOKENS.yaml-Authoring-Specification.md
standards/ui-authoring-specs/UI_VISUAL_SPEC.yaml-Authoring-Specification.md
```

The current system intentionally does not include:

```text
shadcn-tailwind-implementation-standard.md
standards/ui-implementation-standards/
```

A technology-specific UI implementation standard may be added later, but it is outside the current generation order.

## 8. Removed or Inactive Files

The current active document system does not require:

```text
codex-execution-report-format.md
shadcn-tailwind-implementation-standard.md
```

`codex-execution-report.md` is still used, but only as a runtime worklog created and maintained by Codex according to `AGENTS.md`.

`shadcn-tailwind-implementation-standard.md` is removed from the active UI system because concrete styling-stack standards are deferred.

## 9. Codex Runtime Reading Model

Codex should start from:

```text
docs/execution/AGENTS.md
docs/execution/execution-validation.md
```

Codex should not read all reference files by default.

Codex should read only task-scoped sources listed under the current `TASK-*`.

For UI tasks, Codex must read:

```text
the referenced UI YAML files
each referenced UI YAML file's codex_consumption section
```

before modifying UI code.

## 10. Flow-First Execution Model

Execution planning should follow:

```text
minimal foundation tasks
→ executable flow slices
→ cross-flow hardening
→ release validation
```

It should not follow:

```text
all data first
all backend second
all frontend third
all UI fourth
wire together later
```

UI work should be included inside the relevant user-visible flow slices, not postponed as a separate full-UI phase.

## 11. Regeneration Rules

If a document changes, regenerate downstream documents that depend on it.

Examples:

```text
If product-spec.md changes:
- domain-model.md may need review
- data-api-contract.md may need review
- frontend-design.md may need review
- UI_PAGE.yaml may need review
- flow-composition-review.md must be reviewed
- execution-validation.md must be reviewed

If UI_PAGE.yaml changes:
- UI_TOKENS.yaml may need review if visual roles change
- UI_VISUAL_SPEC.yaml must be reviewed if states, surfaces, or actions change
- frontend-design.md may need review
- flow-composition-review.md must be reviewed
- execution-validation.md must be reviewed

If UI_TOKENS.yaml changes:
- UI_VISUAL_SPEC.yaml must be reviewed
- execution-validation.md may need review if UI validation claims change

If UI_VISUAL_SPEC.yaml changes:
- flow-composition-review.md may need review
- execution-validation.md may need review for UI validation claims

If flow-composition-review.md changes:
- execution-validation.md must be regenerated or reviewed
- AGENTS.md may need review if runtime policy assumptions change
```

## 12. Blocked Generation Ordering Rule

If a document cannot be generated safely because a required decision is missing:

```text
do not skip ahead by inventing content
generate the blocked-generation report required by that prompt
resolve the missing decision
regenerate the affected document
then continue downstream
```

This applies especially to:

```text
Open Questions
UI surfaces
API contracts
artifact behavior
recovery behavior
completion signals
validation command availability
styling-stack assumptions
```

## 13. Final Rule

The order is designed to preserve a safe chain:

```text
decisions
→ non-UI references
→ UI references
→ flow composition
→ execution
→ runtime policy
→ review
```

Do not move UI after execution.

Do not generate execution before UI when user-visible flows require UI surface, feedback, recovery, artifact, or completion signal information.

Do not generate technology-specific UI implementation standards unless the project has explicitly selected a styling stack.
