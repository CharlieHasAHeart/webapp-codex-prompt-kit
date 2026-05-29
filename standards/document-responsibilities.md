# Document Responsibilities Standard

## 1. Purpose

This standard defines responsibility boundaries for generated documents in the WebApp Codex Prompt Kit.

It ensures that review documents, reference catalogs, UI reference files, and execution documents each serve their own role without duplicate ownership, hidden dependencies, or unsafe Codex execution assumptions.

## 2. Scope

This standard applies to:

```text
docs/review/*.md
docs/reference/*.md
docs/reference/ui/*.yaml
docs/execution/*.md
```

It also defines how generated project documents relate to prompt-kit standards and UI authoring specifications.

## 3. Core Principle

Generated documents are split into three roles:

```text
review     -> connect, explain, resolve, compose, and prepare
reference  -> define stable source catalogs and UI reference intent
execution  -> instruct Codex what to do, what to read, and how to validate it
```

Review documents may connect.

Reference documents must define.

Execution documents must direct.

UI reference documents define the user-visible shape of the application, but they do not define a concrete frontend styling stack.

## 4. Document Role Separation

### DR-001: Document Role Separation Rule

Requirement:
- Each generated document must belong to one role: review, reference, or execution.

Required:
- Review docs may summarize, connect, map, and analyze.
- Reference docs define stable source-of-truth entries or stable UI reference intent.
- Execution docs define Codex runtime behavior, tasks, validation, and worklog policy.

Forbidden:
- Turning a reference catalog into a review narrative.
- Turning a review document into a final source-of-truth catalog.
- Turning an execution document into a product, API, domain, frontend, backend, UI, or environment source catalog.
- Turning UI reference files into React code, CSS class catalogs, or styling-stack implementation standards.

Applies To:
- All generated documents.

Check:
- A reviewer can identify whether the file is review, reference, or execution without ambiguity.

---

## 5. Review Document Responsibilities

### DR-002: Review Documents May Connect Rule

Requirement:
- Review documents may intentionally connect multiple future documents.

Allowed:
- Summarizing project context.
- Mapping Open Questions to affected documents.
- Mapping resolutions to target documents.
- Recording cross-document decisions.
- Performing flow composition analysis.
- Reporting cross-document consistency issues.
- Checking whether UI surfaces, actions, feedback, recovery paths, artifacts, and completion signals support the intended flows.

Forbidden:
- Treating review documents as Codex default runtime context.
- Defining final source-of-truth entries owned by reference documents.
- Defining final `FLOW-*`, `TASK-*`, or `VAL-*` entries unless the review document explicitly reports existing entries.
- Leaving unresolved review questions inside final reference or execution documents.

Applies To:
- `docs/review/project-design-brief.md`
- `docs/review/open-questions-review.md`
- `docs/review/question-resolution.md`
- `docs/review/project-decisions.md`
- `docs/review/flow-composition-review.md`
- `docs/review/cross-document-review-report.md`

Check:
- Review documents can reference and map many files, but final source-of-truth content is still converted into the correct owner document.

---

## 6. Non-UI Reference Document Responsibilities

### DR-003: Non-UI Reference Decoupling Rule

Requirement:
- Non-UI reference catalogs must be ownership-decoupled.

Applies To:
- `docs/reference/product-spec.md`
- `docs/reference/domain-model.md`
- `docs/reference/architecture.md`
- `docs/reference/data-api-contract.md`
- `docs/reference/frontend-design.md`
- `docs/reference/backend-design.md`
- `docs/reference/dev-environment.md`

Required:
- Each non-UI reference catalog must define its own responsibility layer.
- A reference entry must be understandable within its own document's ownership boundary.
- Cross-document IDs may appear only as traceability hints.
- Related IDs must not replace the current entry's owned content.

Forbidden:
- Making one reference catalog depend on another reference catalog for meaning.
- Writing entries whose main content is "see another file."
- Copying another reference catalog's source-of-truth definitions.
- Redefining another reference catalog's owned content.
- Scattering final flow composition across reference catalogs.

Check:
- A non-UI reference entry can be understood without opening another non-UI reference document.

---

### DR-004: Non-UI Reference Ownership Rule

Requirement:
- Each non-UI reference catalog owns only its designated ID family or responsibility area.

Ownership:
- `product-spec.md` owns `REQ-*`.
- `domain-model.md` owns `ENT-*`, `REL-*`, `BR-*`, `STATE-*`.
- `architecture.md` owns `ARCH-*`.
- `data-api-contract.md` owns `DB-*`, `API-*`, `ERR-*`, `TYPE-*`.
- `frontend-design.md` owns `FE-*`.
- `backend-design.md` owns `BE-*`.
- `dev-environment.md` owns `ENV-*`.

Forbidden:
- Creating `TASK-*` or `VAL-*` in reference catalogs.
- Creating final executable `FLOW-*` in reference catalogs.
- Leaving unresolved Open Questions in reference catalogs.
- Performing full flow assembly in reference catalogs.

Check:
- The document's headings and IDs match its owned family.

---

### DR-005: Non-UI Entry Self-Containment Rule

Requirement:
- Every non-UI reference entry must be self-contained at the entry level.

Required:
- A `REQ-*` entry states product-facing behavior and acceptance intent.
- An `ENT-*` entry states domain meaning.
- A `BR-*` entry states the rule and when it applies.
- A `STATE-*` entry states state meaning and transition constraints.
- An `ARCH-*` entry states the architecture boundary or rule.
- An `API-*` entry states route, request, response, and error behavior.
- A `TYPE-*` entry states the shared contract meaning.
- A `FE-*` entry states frontend responsibility.
- A `BE-*` entry states backend responsibility.
- An `ENV-*` entry states environment or command policy.

Forbidden:
- Replacing owned explanation with a related ID.
- Using another reference file as required reading for understanding the current entry.
- Writing incomplete entries that only point elsewhere.

Check:
- Each entry can be read alone by ChatGPT or Codex when referenced from a task.

---

### DR-006: Traceability Without Dependency Rule

Requirement:
- Cross-document references in non-UI reference catalogs must support traceability, not dependency.

Allowed:

```text
Related Requirements:
- REQ-001
```

Forbidden:

```text
This implements REQ-001. See REQ-001 for details.
```

Required:
- State the current entry's owned meaning first.
- Add related IDs only after the current entry is understandable.
- Use related IDs for review, consistency checking, flow composition, and task-scoped source mapping.

Check:
- Removing related IDs should not destroy the entry's owned meaning.

---

## 7. UI Reference Document Responsibilities

### DR-007: UI Reference System Rule

Requirement:
- UI reference files define the user-visible shape of the Web App.

UI reference files:

```text
docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

Core principle:

```text
UI is the flow-facing shape of the Web App.
```

UI references must describe:
- what the user can see
- what the user can do
- how actions trigger effects
- how system status becomes visible
- how failure and blocked states are shown
- how recovery is offered
- where artifacts appear
- where completion becomes visible

UI references must not be reduced to:
- page names only
- component lists only
- visual tokens only
- style preferences only

Check:
- The UI reference set makes user flows operable, observable, recoverable, and complete.

---

### DR-008: UI File Ownership Rule

Requirement:
- Each UI reference file owns a distinct UI responsibility layer.

Ownership:

```text
UI_PAGE.yaml
= flow-facing semantic UI surface

UI_TOKENS.yaml
= technology-agnostic design token reference

UI_VISUAL_SPEC.yaml
= visual and interaction presentation rules
```

`UI_PAGE.yaml` owns:
- app shell
- navigation hierarchy
- routes
- pages
- sections
- actions
- route-backed state
- local UI state
- global UI states
- semantic component roles
- flow surface mapping
- action effect mapping
- feedback state mapping
- recovery path mapping
- artifact surface mapping
- completion signal mapping
- traceability to `REQ-*`, `API-*`, `ERR-*`, `TYPE-*`, and `FE-*` where useful

`UI_TOKENS.yaml` owns:
- theme intent
- semantic color roles
- typography tokens
- spacing tokens
- radius tokens
- border tokens
- shadow tokens
- layout dimension tokens
- breakpoint tokens
- motion tokens
- z-index tokens
- status token roles
- accessibility token roles

`UI_VISUAL_SPEC.yaml` owns:
- visual direction
- token usage intent
- layout presentation rules
- surface hierarchy
- navigation presentation rules
- component visual roles
- interaction state presentation
- feedback state presentation
- recovery presentation
- artifact presentation
- completion signal presentation
- responsive behavior
- accessibility presentation rules
- density intent
- status-to-visual-role mapping

Check:
- A reader can determine whether a UI concern belongs in `UI_PAGE.yaml`, `UI_TOKENS.yaml`, or `UI_VISUAL_SPEC.yaml`.

---

### DR-009: UI Technology-Agnostic Rule

Requirement:
- UI reference files must not assume a concrete frontend styling stack.

Forbidden unless explicitly introduced later by a dedicated implementation standard:
- Tailwind-specific rules
- shadcn/ui-specific rules
- CSS variable mappings
- MUI theme mappings
- Chakra theme mappings
- CSS Modules mappings
- Styled Components implementation rules
- Vanilla Extract implementation rules
- framework-specific component requirements
- full `className` strings
- React or JSX code

Required:
- UI references describe semantic UI, token intent, and visual/interaction presentation intent.
- Codex maps that intent to the actual project stack and code conventions during implementation.

Check:
- The UI reference files remain usable whether the project uses Tailwind, CSS Modules, MUI, Chakra, plain CSS, or another styling system.

---

### DR-010: UI Codex Consumption Rule

Requirement:
- Every generated UI YAML file must include a compact runtime dictionary section:

```yaml
codex_consumption:
  file_role: ...
  source_of_truth:
    - ...
  traceability_only:
    - ...
  codex_should:
    - ...
  codex_must_not:
    - ...
  read_with:
    - ...
```

Required:
- `UI_PAGE.yaml` must explain how Codex consumes semantic UI surfaces.
- `UI_TOKENS.yaml` must explain how Codex consumes technology-agnostic token intent.
- `UI_VISUAL_SPEC.yaml` must explain how Codex consumes visual and interaction presentation rules.

Forbidden:
- Requiring Codex to infer UI field meaning from field names alone.
- Using `codex_consumption` to define project-specific implementation tasks.
- Using `codex_consumption` to define a concrete styling stack.

Check:
- Codex can understand the role and limits of each UI YAML file without reading prompt-kit standards.

---

### DR-011: UI Traceability Without Redefinition Rule

Requirement:
- UI reference files may reference non-UI source IDs only for traceability.

Allowed:
- `UI_PAGE.yaml` may reference `REQ-*`, `API-*`, `ERR-*`, `TYPE-*`, and `FE-*`.
- `UI_VISUAL_SPEC.yaml` may reference UI_PAGE sections, actions, states, and UI_TOKENS token roles.
- `UI_TOKENS.yaml` should usually avoid non-UI source IDs unless a traceability note is truly useful.

Forbidden:
- Defining API request/response fields in `UI_PAGE.yaml`.
- Defining frontend implementation responsibilities in `UI_PAGE.yaml`.
- Defining token raw values inside `UI_VISUAL_SPEC.yaml`.
- Defining routes or page sections in `UI_VISUAL_SPEC.yaml`.
- Defining workflow logic, page structure, or component implementation in `UI_TOKENS.yaml`.

Check:
- UI files can point to source content, but do not redefine that content.

---

### DR-012: UI Flow Support Rule

Requirement:
- UI reference files must support flow-first execution.

Required:
- Primary user flows have visible UI surfaces.
- Important actions have user affordances.
- Important action effects have visible feedback.
- Critical states have visible presentation.
- Recovery paths are visible or intentionally out of scope.
- Artifacts have visible surfaces when in scope.
- Completion signals are visible to the user.

Forbidden:
- Treating backend success alone as UI completion.
- Hiding failed or blocked states in invisible state only.
- Relying on color alone for critical states.
- Omitting artifact surfaces when artifact interaction is in scope.
- Omitting UI recovery paths when recovery behavior is in scope.

Check:
- `flow-composition-review.md` can use UI references to determine whether each flow is operable, observable, recoverable, and complete.

---

## 8. Execution Document Responsibilities

### DR-013: Execution Document Responsibility Rule

Requirement:
- Execution documents define Codex runtime policy, executable tasks, validation, and runtime worklog behavior.

Required:
- `execution-validation.md` owns final `FLOW-*` where used, `TASK-*`, `VAL-*`, flow-first task sequencing, required validation, release validation, and task-scoped source references.
- `AGENTS.md` owns Codex runtime behavior, task reading policy, validation policy, blocker policy, UI consumption policy, and runtime worklog policy.
- `codex-execution-report.md` is a runtime worklog created and maintained by Codex according to `AGENTS.md`.

Forbidden:
- Redefining product requirements, API contracts, domain rules, frontend design, backend design, UI reference content, or environment source content inside execution docs.
- Treating the runtime worklog as a planning document or source-of-truth catalog.
- Adding unresolved Open Questions to execution documents.
- Asking Codex to infer tasks from reference catalogs or UI YAML files.

Check:
- Codex can execute from `AGENTS.md` and `execution-validation.md` without inventing missing product, UI, architecture, API, FE, BE, or ENV decisions.

---

### DR-014: Runtime Worklog Responsibility Rule

Requirement:
- `docs/execution/codex-execution-report.md` is a Codex runtime worklog, not a generated source catalog.

Required:
- `AGENTS.md` must instruct Codex to create the worklog when missing.
- The worklog records task status, sources read, files changed, validation commands, validation results, blockers, and notes.
- The worklog is updated after task attempts.

Forbidden:
- Generating `codex-execution-report.md` as a normal ChatGPT prompt output.
- Defining new requirements, UI references, decisions, APIs, tasks, or validation criteria inside the worklog.
- Treating the worklog as a reference document.

Check:
- Cross-document review checks whether `AGENTS.md` defines worklog behavior, not whether the worklog pre-exists.

---

## 9. Flow Responsibility Placement

### DR-015: Flow Responsibility Placement Rule

Requirement:
- Flow concepts must be represented at the correct document layer.

Required:
- Product-facing flow meaning belongs in `product-spec.md`.
- Domain states and rules that flows depend on belong in `domain-model.md`.
- API/data behavior required by flows belongs in `data-api-contract.md`.
- Frontend responsibilities required by flows belong in `frontend-design.md`.
- Backend responsibilities required by flows belong in `backend-design.md`.
- Environment or command requirements required by flows belong in `dev-environment.md`.
- UI surfaces, actions, states, feedback, recovery paths, artifacts, and completion signals belong in the UI reference files.
- Flow composition belongs in `docs/review/flow-composition-review.md`.
- Final executable flow assembly belongs in `docs/execution/execution-validation.md`.

Forbidden:
- Creating a separate non-UI reference flow model.
- Turning non-UI reference catalogs into a full flow assembly document.
- Generating final `FLOW-*`, `TASK-*`, or `VAL-*` inside reference catalogs or UI YAML files.
- Treating UI files as execution plans.

Check:
- Flow assembly happens after reference catalogs and UI references are generated, not inside them.

---

## 10. Open Questions Placement

### DR-016: Open Questions Placement Rule

Requirement:
- Open Questions are review-stage artifacts only.

Allowed:
- `docs/review/open-questions-review.md`
- `docs/review/question-resolution.md`

Forbidden:
- Unresolved Open Questions in `docs/reference/*.md`
- Unresolved Open Questions in `docs/reference/ui/*.yaml`
- Unresolved Open Questions in `docs/execution/*.md`
- `OQ-*` references in final reference or execution documents unless describing policy itself.

Check:
- Final reference, UI, and execution docs contain resolved content, not questions.

---

## 11. Duplicate Ownership

### DR-017: Duplicate Ownership Rule

Requirement:
- A generated document must not redefine content owned by another generated document.

Examples:
- `frontend-design.md` must not define API response shapes.
- `backend-design.md` must not define database schema as source of truth.
- `architecture.md` must not define API payload fields.
- `product-spec.md` must not define backend service implementation.
- `domain-model.md` must not define ORM columns.
- `execution-validation.md` must not redefine UI reference content.
- `AGENTS.md` must not define product, domain, API, frontend, backend, UI, environment, task, or validation source-of-truth entries.
- `UI_PAGE.yaml` must not define API request/response shapes.
- `UI_TOKENS.yaml` must not define CSS variable or Tailwind mappings in this revision.
- `UI_VISUAL_SPEC.yaml` must not define routes, sections, class names, JSX, or token raw values.

Check:
- Each source-of-truth fact has one owner.

---

## 12. Document Responsibility Matrix

| Generated File | Owns | Must Not Own |
|---|---|---|
| `docs/review/project-design-brief.md` | Discovery summary, project context, early flow source material, constraints, risks. | Final requirements, API contracts, tasks, validation commands. |
| `docs/review/open-questions-review.md` | Temporary `OQ-*`, blocking classification, affected docs, flow impact. | Final decisions, final requirements, tasks. |
| `docs/review/question-resolution.md` | User-confirmed answers, resolution mapping, conversion targets. | New unresolved questions, implementation tasks. |
| `docs/review/project-decisions.md` | `DEC-*`, project-level choices, rejected alternatives. | Product requirements, API contracts, FE/BE implementation, tasks. |
| `docs/review/flow-composition-review.md` | Candidate flows, UI flow surface checks, Side Effect Flow mapping, foundation readiness, flow-to-task seeds, validation seeds. | Final `FLOW-*`, `TASK-*`, `VAL-*`. |
| `docs/review/cross-document-review-report.md` | Consistency findings, readiness verdict, issue register. | Source-of-truth rewrites unless explicitly requested. |
| `docs/reference/product-spec.md` | `REQ-*`, product-facing flows, scope, feedback, recovery, completion signals. | Domain source definitions, API contracts, UI structure, FE/BE implementation, tasks. |
| `docs/reference/domain-model.md` | `ENT-*`, `REL-*`, `BR-*`, `STATE-*`. | DB schema, API payloads, UI structure, frontend/backend implementation. |
| `docs/reference/architecture.md` | `ARCH-*`, boundaries, dependency direction, runtime/storage/security rules. | API payloads, DB schema, UI structure, component details, tasks. |
| `docs/reference/data-api-contract.md` | `DB-*`, `API-*`, `ERR-*`, `TYPE-*`. | Frontend client implementation, backend service implementation, UI action structure, tasks. |
| `docs/reference/frontend-design.md` | `FE-*`, frontend implementation responsibilities and UI reference consumption responsibilities. | UI_PAGE structure, UI token source, UI visual presentation source, API contracts, DB schema, backend service definitions, tasks. |
| `docs/reference/backend-design.md` | `BE-*`, backend implementation responsibilities. | API contracts as source definitions, DB schema source definitions, frontend/UI behavior, tasks. |
| `docs/reference/dev-environment.md` | `ENV-*`, command policies, runtime/package/service rules. | Task-specific validation selection, product requirements, UI implementation standards. |
| `docs/reference/ui/UI_PAGE.yaml` | Flow-facing semantic UI surface, routes, pages, sections, actions, states, feedback/recovery/artifact/completion mappings. | Visual token values, styling stack, React/JSX, API payloads, backend logic. |
| `docs/reference/ui/UI_TOKENS.yaml` | Technology-agnostic design token intent. | CSS variables, Tailwind mappings, shadcn compatibility, page structure, component implementation. |
| `docs/reference/ui/UI_VISUAL_SPEC.yaml` | Visual and interaction presentation rules. | Routes, page section source definitions, raw token values, class names, JSX, API/backend logic. |
| `docs/execution/execution-validation.md` | Final executable `FLOW-*`, `TASK-*`, `VAL-*`, validation mapping, release validation. | Source definitions owned by reference catalogs or UI files. |
| `docs/execution/AGENTS.md` | Codex runtime policy, task reading policy, UI consumption policy, worklog policy. | Product/API/UI/task/validation source definitions. |
| `docs/execution/codex-execution-report.md` | Runtime worklog entries created by Codex. | New requirements, UI references, decisions, tasks, validation criteria. |

---

## 13. Prompt Integration Requirements

Prompts that generate non-UI reference documents must include or apply:

```text
Apply standards/document-responsibilities.md to enforce ownership boundaries.

For non-UI reference catalogs:
- entries must be ownership-decoupled
- entries must be self-contained
- related IDs are traceability hints only
- do not generate content owned by another document
- do not create unresolved Open Questions
```

Prompts that generate UI reference files must include or apply:

```text
Apply standards/document-responsibilities.md and standards/ui-reference-system.md.

For UI reference files:
- include codex_consumption
- remain technology-agnostic
- do not assume a concrete styling stack
- do not redefine non-UI reference content
- support flow-facing UI surfaces, actions, feedback, recovery, artifacts, and completion signals
- do not create unresolved Open Questions
```

Prompts that generate execution documents must include or apply:

```text
Execution docs may reference UI files task-by-task, but must not redefine UI content.
Codex must read codex_consumption before implementing UI tasks that reference UI YAML files.
```

---

## 14. Cross-Document Review Checklist

Cross-document review should check:

| Check | Failure Example |
|---|---|
| Review/reference/execution role separation | `question-resolution.md` becomes a final API catalog. |
| Non-UI reference ownership boundary | `frontend-design.md` defines API response shapes. |
| UI ownership boundary | `UI_VISUAL_SPEC.yaml` defines routes or page sections. |
| UI technology-agnostic rule | `UI_TOKENS.yaml` contains Tailwind mappings or shadcn compatibility. |
| UI Codex consumption rule | A UI YAML file lacks `codex_consumption`. |
| UI flow support | A Core User Flow has no visible UI surface or completion signal. |
| Traceability without dependency | A related ID replaces the current entry's content. |
| Duplicate source of truth | Backend design repeats DB schema as source definition. |
| Flow placement | Reference docs attempt final `FLOW-*` task assembly. |
| OQ leakage | `OQ-*` appears in final reference, UI, or execution docs. |
| Runtime worklog clarity | `AGENTS.md` does not define creation/update policy for the worklog. |

---

## 15. Forbidden Patterns

Do not write in non-UI reference catalogs:

```text
See REQ-001 for behavior.
```

when the current document owns behavior details for its own layer.

Do not write in `UI_PAGE.yaml`:

```yaml
calls_api:
  path: /api/run
  request:
    file: File
```

because API contracts belong in `data-api-contract.md`.

Do not write in `UI_TOKENS.yaml` in this revision:

```yaml
implementation:
  css_variables:
    primary: --primary
  tailwind_mapping:
    primary: primary
  shadcn_compatibility: true
```

because implementation mappings are intentionally removed from the current UI reference system.

Do not write in `UI_VISUAL_SPEC.yaml`:

```yaml
button:
  className: "rounded-md bg-primary px-4 py-2"
```

because UI visual specs must remain technology-agnostic.

Do not write in final reference, UI, or execution documents:

```text
Open Questions
TBD
unclear
ask user later
```

---

## 16. Final Rule

Review documents may connect.

Non-UI reference documents must define ownership-decoupled source entries.

UI reference documents must define the technology-agnostic user-visible shape, token intent, and visual/interaction presentation of the application.

Execution documents must direct Codex through task-scoped reading, validation, blocker handling, and runtime worklog behavior.

Codex must not infer missing decisions from reference catalogs or UI YAML files.
