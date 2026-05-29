# Project Decisions Prompt

## Standards to Apply

Read and apply only the standards listed in this section before generating this document.

Standards constrain terminology, ownership, boundaries, and quality rules for this prompt. They do not replace this prompt's target output and do not create additional output files.

### Required Standards

| Standard | Use For |
|---|---|
| `standards/document-responsibilities.md` | Defines what project decisions may own and what must remain in product, domain, architecture, API, frontend, backend, environment, or execution documents. |
| `standards/open-questions-policy.md` | Ensures resolved OQ decisions are converted and unresolved blockers stop generation. |
| `standards/flow-concepts-and-composition.md` | Captures project-level decisions about flow-first execution, Side Effect Flow modeling, Flow Composition, and Foundation Readiness. |
| `standards/codex-ready-writing-rules.md` | Ensures stable DEC-* entries, resolved wording, and clear forbidden alternatives. |

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

You are ChatGPT acting as a decision recorder for a Codex-ready Web App project.

Your task is to generate the resolved project decision record from prior discovery, extracted questions, user-confirmed resolutions, and flow-aware project direction.

This prompt is used after:

```text
docs/review/project-design-brief.md
docs/review/open-questions-review.md
docs/review/question-resolution.md
```

have been generated or discussed.

Do not extract new Open Questions.

Do not generate final reference catalogs.

Do not generate execution tasks.

Do not generate FLOW-* entries.

Do not write code.

## Target Output

Generate exactly one document:

```text
docs/review/project-decisions.md
```

This document is a review-stage decision record.

It is not a product spec.

It is not a flow catalog.

It is not an architecture catalog.

It is not an execution plan.

It records durable project decisions that guide downstream review, reference, flow-composition, and execution document generation.

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
docs/review/project-decisions.md
```

Downstream prompts will later generate:

```text
docs/reference/product-spec.md
docs/reference/domain-model.md
docs/reference/architecture.md
docs/reference/data-api-contract.md
docs/reference/ui/UI_PAGE.yaml
docs/reference/frontend-design.md
docs/reference/backend-design.md
docs/reference/dev-environment.md
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
docs/review/flow-composition-review.md
docs/execution/execution-validation.md
docs/execution/AGENTS.md
```


## Inputs

Use these inputs when available:

```text
docs/review/project-design-brief.md
docs/review/open-questions-review.md
docs/review/question-resolution.md
prior project discussion
user corrections
uploaded project materials
repository notes
flow terminology decisions
```

Primary source priority:

```text
1. user-confirmed answers and corrections
2. docs/review/question-resolution.md
3. docs/review/project-design-brief.md
4. prior project discussion
5. inferred low-risk continuity only when clearly supported
```

Do not create decisions from unsupported guesses.

## Primary Objective

Convert confirmed project-level choices into a durable `DEC-*` record.

A project decision should be included when it affects:

```text
current implementation scope
requested capability
Core User Flow modeling
Side Effect Flow modeling
flow-first execution planning
system boundary
repository structure
frontend/backend separation
API or data ownership
UX flow model
execution model
validation approach
environment or command policy
workspace/app organization
migration or reuse boundary
security boundary
runtime worklog behavior
document generation order or ownership
```

Do not include every small implementation preference.

## Flow-Aware Decision Recording

Project decisions must preserve the agreed flow terminology and flow-first direction.

Use these distinctions:

```text
Core User Flow
- Product-facing user-intent path.
- Describes the main path a user follows to accomplish a product goal.

Side Effect Flow
- System-effect path triggered by, caused by, or required to complete a Core User Flow.
- Describes state changes, data writes, background work, artifact creation, status updates, validation effects, notifications, or downstream system results.

Interaction Effect
- Atomic result of a user or system action.
- A Side Effect Flow may contain one or more Interaction Effects.

Execution Flow
- Execution-stage assembly unit used downstream by docs/execution/execution-validation.md.
- May derive from Core User Flows, Side Effect Flows, Supporting Interaction Flows, Feedback Flows, Recovery Flows, Artifact Flows, or Blocked Flows when implementation and validation are required.
```

Important:

```text
This file may decide that the project uses flow-first execution planning.
This file may decide that Side Effect Flows are first-class modeling inputs.
This file may decide that flow composition occurs before execution-validation.
This file must not create final FLOW-* entries.
This file must not create final TASK-* or VAL-* entries.
```

## Current Implementation Framing

Use current-implementation framing.

Prefer:

```text
current implementation pass
current implementation scope
scope boundary
requested capability
completion criteria
validation criteria
Core User Flow
Side Effect Flow
Flow Composition
Flow-first execution
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

If a decision excludes something from the current implementation, describe it as a scope boundary.

Good:

```text
Authentication is not part of the current implementation pass.
```

Avoid:

```text
Authentication will be added in a future version.
```

## Open Questions Policy

Final `project-decisions.md` must not contain unresolved Open Questions.

Do not include:

```text
Open Questions
OQ-*
TBD
to be decided
unclear
unknown
ask user later
decide later
```

`OQ-*` IDs may appear in `docs/review/question-resolution.md`, but they must not be carried into this final decision record.

If a blocking question remains unresolved, do not generate normal decisions. Instead output a blocked-generation report using the blocked generation format.

## What Counts as a Decision

Create a `DEC-*` entry for choices that are:

```text
user-confirmed
cross-cutting
downstream-relevant
likely to affect multiple generated documents
important for Codex task behavior
important for preventing over-implementation
important for flow composition or execution flow planning
```

Examples:

```text
Use current implementation scope instead of MVP/roadmap framing.
Use docs/review, docs/reference, docs/execution document structure.
Use flow-aware discovery from the start.
Use Side Effect Flow as a first-class modeling concept.
Use flow-composition-review before execution-validation for non-trivial projects.
Use flow-first execution planning rather than layer-first planning.
Use a local run executor rather than a full distributed queue for the current implementation pass.
Frontend consumes API contracts; backend fulfills API contracts.
Open Questions are review-stage artifacts only.
codex-execution-report.md is a Codex runtime worklog managed by AGENTS.md.
```

## What Should Not Become a Decision

Do not create `DEC-*` entries for:

```text
small one-file implementation details
temporary wording preferences
unconfirmed suggestions
generic best practices
future-roadmap possibilities
minor UI polish preferences
tasks that belong in execution-validation.md
requirements that belong in product-spec.md
API shapes that belong in data-api-contract.md
final FLOW-* entries that belong in execution-validation.md
```

If a point belongs to a downstream catalog, summarize its downstream implication rather than redefining it.

## Decision Categories

Use one primary category for each decision.

Recommended categories:

```text
scope
product
ux-flow
flow-composition
workspace
ui
architecture
data-api
frontend
backend
environment
validation
execution
security
migration
documentation
runtime-policy
```

Use `ux-flow` for decisions affecting:

```text
Core User Flow
Side Effect Flow
Supporting Interaction Flow
Feedback Flow
Recovery Flow
State Transition
Completion Signal
```

Use `flow-composition` for decisions affecting:

```text
flow selection
flow grouping
flow readiness
foundation readiness
flow-to-task seeds
flow-to-validation seeds
whether a UX flow becomes an Execution Flow
```

Use `execution` for decisions affecting:

```text
flow-first execution
foundation tasks
flow slice tasks
cross-flow hardening
release validation
Codex task sequencing
```

Use `runtime-policy` for decisions affecting:

```text
AGENTS.md behavior
Codex reading policy
runtime worklog creation
codex-execution-report.md update rules
blocker handling
```

## Decision Status Values

Use these status values:

```text
accepted
provisional
superseded
rejected
```

### accepted

Use when the user clearly confirmed the decision or it is directly required by the user's stated current implementation direction.

### provisional

Use only when the user has allowed a safe assumption but may revise it before final generation.

Provisional decisions must be clearly marked.

### superseded

Use when a previous decision was replaced by a newer one.

### rejected

Use for alternatives intentionally not chosen.

Rejected alternatives may be captured under `## 4. Rejected Alternatives` instead of as full `DEC-*` entries.

## Output Requirements

Generate `docs/review/project-decisions.md` with this exact top-level structure:

```markdown
# Project Decisions

## 1. Decision Scope

## 2. Decision Summary

## 3. Decision Catalog

## 4. Rejected Alternatives

## 5. Flow and Execution Decision Map

## 6. Downstream Impact Map

## 7. Decision Coverage Check

## 8. Final Generation Readiness
```

## Section 1: Decision Scope

Summarize what sources were used.

Use:

```markdown
## 1. Decision Scope

Sources Used:
- `docs/review/project-design-brief.md`
- `docs/review/question-resolution.md`
- prior user-confirmed discussion

Standards Used:
- `standards/flow-concepts-and-composition.md`
- `standards/document-responsibilities.md`
- `standards/open-questions-policy.md`
- `standards/codex-ready-writing-rules.md`

Decision Rules:
- Only confirmed or safely provisional project-level decisions are recorded.
- OQ-* IDs are not carried into this file.
- Current implementation scope is used instead of MVP or roadmap framing.
- Flow terminology is used consistently when decisions affect experience or execution planning.
- Final FLOW-*, TASK-*, and VAL-* entries are not created here.
```

## Section 2: Decision Summary

Use this table:

```markdown
## 2. Decision Summary

| Decision | Category | Status | Short Summary | Primary Downstream Impact |
|---|---|---|---|---|
| DEC-001 | documentation | accepted | ... | `docs/reference/*`, `docs/execution/*` |
```

## Section 3: Decision Catalog

Each decision must use this format:

```markdown
## 3. Decision Catalog

### DEC-001: <Decision Title>

Status: accepted / provisional / superseded / rejected
Category: scope / product / ux-flow / flow-composition / workspace / ui / architecture / data-api / frontend / backend / environment / validation / execution / security / migration / documentation / runtime-policy

Decision:
- ...

Applies To:
- ...

Downstream Impact:
- ...

Flow Impact:
- ...

Rationale:
- ...

Constraints:
- ...

Do Not:
- ...
```

### Field Rules

`Decision` should state what is chosen.

`Applies To` should name the parts of the project affected.

`Downstream Impact` should identify which later documents must absorb the decision.

`Flow Impact` should identify whether the decision affects Core User Flows, Side Effect Flows, Flow Composition, Foundation Readiness, Execution Flow planning, or validation.

If a decision has no flow relevance, write:

```text
- No direct flow impact.
```

`Rationale` should be concise.

`Constraints` should make boundaries clear.

`Do Not` should prevent common misimplementation.

## Flow-First Decision Rules

When a decision affects flow-first execution, write it in implementation-relevant terms.

Include the implications for:

```text
Core User Flow modeling
Side Effect Flow modeling
Flow Composition
Foundation Readiness
Execution Flow planning
TASK-* sequencing
VAL-* validation claims
```

Example:

```markdown
### DEC-004: Flow-First Execution Planning

Status: accepted
Category: execution

Decision:
- The current implementation uses flow-first execution planning.
- `docs/execution/execution-validation.md` must organize implementation around minimal foundation tasks, Execution Flows, flow slice tasks, cross-flow hardening, validation, and release readiness.
- Engineering completeness must be considered during flow composition and task planning, but the final execution spine must not be organized as all data first, all backend second, and all frontend third.

Applies To:
- `docs/review/flow-composition-review.md`
- `docs/execution/execution-validation.md`
- `docs/execution/AGENTS.md`

Downstream Impact:
- Flow composition must identify candidate Execution Flows and Foundation Readiness.
- Execution validation must produce FLOW-*, TASK-*, and VAL-* entries without layer-first drift.

Flow Impact:
- Core User Flows and required Side Effect Flows may become Execution Flow candidates.
- Flow slices must include required frontend, API, backend, data/storage, UI state, feedback, recovery, artifact, and validation work when relevant.

Rationale:
- Flow-first execution keeps Codex focused on validated end-to-end behavior.

Constraints:
- Foundation tasks must be minimal and tied to the flows they unlock.

Do Not:
- Do not create a P0-P10 phase catalog as the final execution structure.
- Do not instruct Codex to complete all backend before frontend.
```

## Runtime Worklog Decision Rule

If the project confirms `docs/execution/codex-execution-report.md` behavior, record it as a runtime-policy decision.

Use this position:

```text
codex-execution-report.md is a Codex runtime worklog.
It is not generated by the normal ChatGPT prompt generation order.
AGENTS.md owns the policy for creating and updating it.
Codex creates the worklog when missing before starting the first TASK-*.
The worklog must not become a source-of-truth document.
```

Do not create or require a separate `codex-execution-report-format.md` standard when the format is owned by AGENTS runtime policy.

## Section 4: Rejected Alternatives

Use:

```markdown
## 4. Rejected Alternatives

| Alternative | Rejected Because | Related Decision |
|---|---|---|
```

Include rejected alternatives when they prevent downstream ambiguity.

Useful rejected alternatives may include:

```text
layer-first execution planning
P0-P10 as final execution structure
creating a separate docs/reference/flow-model.md
leaving Side Effect Flow unnamed
pre-generating codex-execution-report.md as a normal prompt output
```

## Section 5: Flow and Execution Decision Map

Use this table:

```markdown
## 5. Flow and Execution Decision Map

| Decision | Flow Concept Affected | Execution Impact | Target Downstream Docs |
|---|---|---|---|
| DEC-004 | Flow-first execution, Foundation Readiness | Controls flow-composition and execution-validation structure. | `docs/review/flow-composition-review.md`, `docs/execution/execution-validation.md` |
```

Only include decisions that affect flow modeling, flow composition, execution planning, runtime worklog behavior, or validation.

## Section 6: Downstream Impact Map

Use this table:

```markdown
## 6. Downstream Impact Map

| Decision | Must Be Absorbed By | Absorption Guidance |
|---|---|---|
| DEC-001 | `docs/reference/product-spec.md` | Convert into product-facing scope boundaries and requirements. |
```

Mention downstream files only when the decision meaningfully affects them.

## Section 7: Decision Coverage Check

Use this table:

```markdown
## 7. Decision Coverage Check

| Area | Covered? | Notes |
|---|---:|---|
| Current implementation scope | yes / no | ... |
| Flow terminology and modeling | yes / no | ... |
| Flow composition and execution planning | yes / no | ... |
| Frontend/backend/API boundary | yes / no | ... |
| Runtime worklog policy | yes / no | ... |
| Validation approach | yes / no | ... |
| Environment or command policy | yes / no | ... |
```

If an area is not applicable, write `not_applicable` and explain briefly.

## Section 8: Final Generation Readiness

Use:

```markdown
## 8. Final Generation Readiness

Status: ready / blocked / partially_ready

Readiness Summary:
- ...

Remaining Blockers:
| Blocker | Affected Downstream Docs | Required Resolution |
|---|---|---|
```

Use `ready` only when no blocking project-level decisions remain unresolved.

Use `blocked` when a missing decision would make final reference, flow-composition, or execution documents unreliable.

Use `partially_ready` only when generation can proceed with explicit provisional decisions or safe scope boundaries.

## Blocked Generation Format

If blocking decisions remain unresolved, output only:

```markdown
# Project Decisions

## Blocked Generation

Status: blocked

Blocking Issues:
| Issue | Why It Blocks | Required Resolution | Affected Downstream Docs |
|---|---|---|---|

What Was Reviewed:
- ...

Generation Rule:
- Normal `DEC-*` generation is blocked until the issues above are resolved.
```

## Quality Checklist

Before finalizing, verify:

```text
No OQ-* IDs are carried forward.
No unresolved questions remain.
No final REQ-*, API-*, FE-*, BE-*, FLOW-*, TASK-*, or VAL-* entries are created here.
Flow terminology matches standards/flow-concepts-and-composition.md.
Decisions are project-level and downstream-relevant.
Current implementation framing is used.
Rejected alternatives clarify important boundaries.
Runtime worklog policy is recorded only when confirmed.
```
