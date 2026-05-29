# Web App Execution Spine Standard

## 1. Purpose

This standard defines how `docs/execution/execution-validation.md` should be structured for a Codex-ready Web App project.

The execution spine is the primary implementation plan that Codex follows after review and reference documents are generated.

This standard establishes a **flow-first execution model**.

It does not define product requirements, domain meaning, API contracts, frontend design, backend design, UI design, or environment command details.

## 2. Scope

This standard applies to:

- `prompts/execution-validation-prompt.md`
- `docs/execution/execution-validation.md`
- `prompts/AGENTS-prompt.md`
- `docs/execution/AGENTS.md`
- `prompts/cross-document-review-prompt.md`
- `docs/review/flow-composition-review.md` when present

This standard should be used together with:

- `standards/flow-concepts-and-composition.md`
- `standards/document-responsibilities.md`
- `standards/codex-ready-writing-rules.md`
- `standards/validation-strategy.md`

## 3. Core Principle

`docs/execution/execution-validation.md` must be a **flow-first execution spine**.

It should organize implementation around executable and verifiable flows, not around technical layers.

The execution spine must answer:

- what Codex should implement
- in what order
- from which task-scoped sources
- within what scope
- with what validation
- under what completion rule

It must not require Codex to infer implementation tasks from reference catalogs.

## 4. Execution Spine Ownership

`docs/execution/execution-validation.md` owns:

- execution reading policy
- implementation strategy
- foundation tasks
- internal `FLOW-*` execution model entries when useful
- flow slice tasks
- cross-flow hardening tasks
- `TASK-*` entries
- `VAL-*` entries
- flow-level validation
- task-to-validation mapping
- release validation
- execution worklog update rules

It must not own:

- `REQ-*` product requirements
- `ENT-*`, `BR-*`, or `STATE-*` domain definitions
- `ARCH-*` architecture boundaries
- `API-*`, `DB-*`, `ERR-*`, or `TYPE-*` contracts
- `FE-*` frontend design entries
- `BE-*` backend design entries
- `ENV-*` command definitions
- UI page, token, or visual source definitions
- unresolved Open Questions

## 5. Flow-First Rule Catalog

### EXEC-RULE-001: Use Flow-First Structure

Requirement:

- `execution-validation.md` must use a flow-first structure.

Required:

- Foundation tasks before flow slices only when necessary.
- Flow model entries for executable user-facing or system-supporting flows when useful.
- Flow slice tasks that implement one behavior at a time.
- Flow-level validation for important flows.

Forbidden:

- Organizing the document as all data first, all backend second, all frontend third.
- Exposing a P0-P10 phase catalog as the top-level structure.
- Creating competing task catalogs.

Check:

- A reviewer can identify the behavior each task helps make executable.

### EXEC-RULE-002: Flow-First Does Not Mean Foundation-Free

Requirement:

- Every flow slice must have the minimum reusable foundation needed before Codex starts implementing it.

Required:

- Identify prerequisites before generating flow slice tasks.
- Create narrow foundation tasks for unavoidable reusable prerequisites.
- List dependencies from flow slice tasks to foundation tasks.

Forbidden:

- Starting a flow slice that requires Codex to invent project structure, command policy, contract base, runtime wiring, or validation setup.
- Hiding broad bootstrap work inside a flow slice.

Check:

- Each flow slice can begin without Codex making unplanned foundational decisions.

### EXEC-RULE-003: Foundation Tasks Must Be Minimal and Enabling

Requirement:

- Foundation tasks must create only the base required to unlock one or more flow slices.

Required:

- Each foundation task must state which flow or flows it unlocks.
- Foundation work must be reusable and necessary before the flow begins.

Forbidden:

- Implementing complete backend layers as foundation.
- Implementing complete frontend layers as foundation.
- Implementing all data models, all APIs, or all UI states before any flow.

Check:

- Removing the foundation task would block a named flow slice.

### EXEC-RULE-004: Absorb Engineering Coverage Into Flow Planning

Requirement:

- ChatGPT must consider full Web App implementation coverage while composing the flow-first plan.

Coverage considerations include:

- project bootstrap
- development environment
- shared contracts and types
- data or storage foundation
- backend API foundation
- backend workflow support
- frontend app shell
- frontend workflow implementation
- UI states and accessibility
- cross-cutting hardening
- release validation

Required:

- These concerns should appear as foundation, flow-slice, hardening, or release-validation work when needed.

Forbidden:

- Turning the coverage considerations into a mandatory P0-P10 phase table.
- Treating coverage areas as permission to implement all of one layer before flows.

Check:

- The execution plan is complete enough without becoming layer-first.

### EXEC-RULE-005: Use One Executable Task Catalog

Requirement:

- `TASK-*` entries are the only executable task source for Codex.

Required:

- Flow model entries may describe behavior.
- Task entries must define executable implementation work.
- Validation entries must prove task or flow claims.

Forbidden:

- Creating separate task catalogs that compete with the main `TASK-*` catalog.
- Letting `FLOW-*` entries function as executable tasks.

Check:

- Codex can determine the next implementation step only from `TASK-*` entries.

### EXEC-RULE-006: Keep Flow Model Internal to Execution

Requirement:

- `FLOW-*` entries, when used, belong only inside `execution-validation.md`.

Required:

- Use `FLOW-*` as execution assembly units.
- Do not create `docs/reference/flow-model.md`.
- Use `docs/review/flow-composition-review.md` only as a review-stage intermediate artifact when needed.

Forbidden:

- Treating flow model entries as final reference catalogs.
- Leaving flow candidates from review artifacts as final executable tasks.

Check:

- Final executable flow definitions appear in `execution-validation.md`, not in reference docs.

### EXEC-RULE-007: Validate Flows, Not Only Layers

Requirement:

- Important flows must have validation that proves behavior, not just isolated layer correctness.

Required:

- Each major flow should map to task validation and, when useful, flow-level validation.
- Validation claims must explain the user-visible or system-behavior proof.

Forbidden:

- Using only vague commands such as “run tests.”
- Marking a flow complete because backend tests pass while frontend feedback or recovery remains unproven.

Check:

- A reviewer can see how validation proves the flow is executable, observable, recoverable, or complete.

## 6. Recommended Output Structure

`docs/execution/execution-validation.md` should use this top-level structure:

```markdown
# Execution Validation

## 1. Execution Scope
## 2. Execution Reading Policy
## 3. Implementation Strategy
## 4. Foundation Task Catalog
## 5. Flow Model
## 6. Flow Slice Task Catalog
## 7. Cross-Flow Hardening Task Catalog
## 8. Validation Catalog
## 9. Flow-Level Validation
## 10. Task-to-Validation Mapping
## 11. Release Validation
## 12. Execution Worklog Rules
## 13. Execution Readiness
```

The final document may omit `Flow Model` only when the project is trivial and the task catalog itself clearly preserves flow-first sequencing.

## 7. Foundation Task Guidance

Foundation tasks are allowed only when they are required before a flow slice can be implemented safely.

Common foundation task areas:

- repository or app skeleton
- minimal frontend/backend runtime wiring
- service entry points
- shared contract or type base
- API error envelope base
- storage or workspace base
- minimal app shell or routing base
- validation command wiring

A foundation task should include:

- task type
- priority
- dependencies
- goal
- flows unlocked
- read-before sources
- implementation scope
- out-of-scope boundaries
- required validation
- completion rule

Foundation tasks must not implement full product behavior.

## 8. Flow Model Guidance

A `FLOW-*` entry describes an executable behavior to be assembled.

A flow may derive from:

- Core User Flow
- Side Effect Flow
- Supporting Interaction Flow
- Feedback Flow
- Recovery Flow
- Artifact Flow
- Blocked Flow
- Status Feedback Flow

Each important flow should include:

- flow type
- goal
- trigger or start condition
- required foundation
- inputs
- end-to-end slice responsibilities
- state transitions
- feedback
- failure and recovery behavior
- completion signal
- validation strategy

Do not turn every small interaction into a flow.

## 9. Flow Slice Task Guidance

A flow slice task implements the minimum useful vertical path for one behavior.

A flow slice may include the frontend, API, backend, data/storage, artifact, UI state, feedback, recovery, and validation work needed for that behavior.

A flow slice task should include:

- task type
- related flow
- dependencies
- goal
- prerequisites already satisfied
- implementation scope
- flow-local setup allowed
- out-of-scope boundaries
- expected code impact
- required validation
- completion rule

A flow slice must not silently broaden into unrelated flows.

## 10. Cross-Flow Hardening Guidance

Cross-flow hardening tasks should appear only after relevant flow slices exist.

Use hardening tasks for:

- shared error consistency
- security boundaries
- path traversal prevention
- artifact safety
- validation visibility
- accessibility across implemented states
- frontend/backend contract consistency
- release-level regression checks

Do not use hardening tasks to implement missing core behavior.

## 11. Validation Guidance

Validation must be task-scoped and claim-proven.

Validation entries should include:

- purpose
- command
- claim proven
- used by
- failure meaning

Flow-level validation should prove that an end-to-end behavior works or that a validation combination proves the flow in the absence of full E2E automation.

## 12. Execution Worklog Guidance

`docs/execution/codex-execution-report.md` is a Codex runtime worklog.

It is not generated by `execution-validation.md`.

`AGENTS.md` defines how Codex creates and updates it.

`execution-validation.md` may require tasks to update the worklog as part of completion.

## 13. Prompt Integration

Prompts that use this standard should:

- preserve flow-first structure
- use `standards/flow-concepts-and-composition.md` for flow terminology
- use `standards/validation-strategy.md` for validation rules
- avoid P0-P10 top-level phase output
- require foundation tasks only when they unlock flows
- ensure the final task catalog is the single executable source for Codex

## 14. Review Checklist

A reviewer should check:

- Does `execution-validation.md` use a flow-first structure?
- Are foundation tasks minimal and tied to named flows?
- Does each flow slice have required prerequisites?
- Are tasks sequenced by executable behavior rather than technical layer?
- Is there one `TASK-*` catalog and one `VAL-*` catalog?
- Are important flows mapped to validation?
- Are Open Questions absent from execution docs?
- Is the execution worklog treated as a runtime worklog governed by `AGENTS.md`?
