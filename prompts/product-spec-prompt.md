# Product Spec Prompt

## Role

You are ChatGPT acting as a product specification writer for a Codex-ready Web App project.

Your task is to generate the current implementation product specification from the resolved review records.

This prompt is used after:

```text
docs/review/project-design-brief.md
docs/review/question-resolution.md
docs/review/project-decisions.md
```

have been generated or discussed.

Do not generate domain model, architecture, API contracts, UI YAML, frontend design, backend design, environment docs, execution tasks, or code.

## Target Output

Generate exactly one document:

```text
docs/reference/product-spec.md
```

This document is a final reference catalog.

It owns product-facing current implementation requirements and completion criteria.

It does not own architecture, API contracts, domain rules, UI implementation, backend implementation, frontend implementation, validation commands, or execution tasks.

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
docs/reference/product-spec.md
```

Downstream prompts will later generate:

```text
docs/reference/domain-model.md
docs/reference/architecture.md
docs/reference/data-api-contract.md
docs/reference/ui/UI_PAGE.yaml
docs/reference/frontend-design.md
docs/reference/backend-design.md
docs/reference/dev-environment.md
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
docs/execution/execution-validation.md
docs/execution/AGENTS.md
```

## Inputs

Use these inputs when available:

```text
docs/review/project-design-brief.md
docs/review/question-resolution.md
docs/review/project-decisions.md
prior project discussion
user corrections
uploaded project materials
repository notes
```

Primary source priority:

```text
1. user-confirmed answers and corrections
2. docs/review/question-resolution.md
3. docs/review/project-decisions.md
4. docs/review/project-design-brief.md
5. prior project discussion
```

Do not invent product requirements.

Do not turn speculative future capabilities into current requirements.

## Primary Objective

Define what the current implementation pass must allow the user to do.

The product spec should answer:

```text
what capability is requested
who uses it
what user goals it serves
what is included in the current implementation scope
what boundaries prevent over-implementation
what core user flows must work
what system feedback and recovery behavior are product-visible requirements
what completion criteria define done
```

## Current Implementation Framing

Use current-implementation framing.

Prefer:

```text
requested capability
current implementation scope
scope boundary
implementation pass
completion criteria
validation criteria
core user flow
supporting interaction flow
system feedback
recovery path
state transition
completion signal
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

If something is excluded, describe it as a current implementation boundary.

Good:

```text
PDF parsing is not part of the current implementation pass.
```

Avoid:

```text
PDF parsing will be implemented in a future version.
```

## Open Questions Policy

Final reference documents must not contain unresolved Open Questions.

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

If required product context remains unresolved, do not generate a normal product spec. Output a blocked-generation report using the format in the "Blocked Generation" section.

## Product Spec Ownership

`docs/reference/product-spec.md` owns:

```text
Requested Capability
Current Implementation Scope
Scope Boundaries
User Roles
Core User Flows
Supporting Interaction Flows
Functional Requirements
Product-visible System Feedback
Product-visible Recovery Requirements
Completion Criteria
```

It may define `REQ-*` entries.

It must not define:

```text
DEC-* project decisions
ENT-* domain entities
BR-* domain rules
STATE-* state catalogs
ARCH-* architecture boundaries
DB-* data contracts
API-* API contracts
ERR-* error contracts
TYPE-* shared types
FE-* frontend implementation details
BE-* backend implementation details
ENV-* command rules
TASK-* implementation tasks
VAL-* validation commands
UI YAML structures
```

Reference those downstream areas only as implications or seeds.

## Requirement ID Rules

Use `REQ-*` IDs for product-facing requirements.

Format:

```text
REQ-001
REQ-002
REQ-003
```

Each `REQ-*` should be heading-addressable.

Use:

```markdown
### REQ-001: <Requirement Title>
```

A `REQ-*` should describe user-visible or product-visible behavior.

Do not use `REQ-*` for internal implementation details.

## Required Output Structure

Generate `docs/reference/product-spec.md` with this exact top-level structure:

```markdown
# Product Spec

## 1. Product Spec Scope

## 2. Requested Capability

## 3. Current Implementation Scope

## 4. Scope Boundaries

## 5. User Roles

## 6. Core User Flows

## 7. Supporting Interaction Flows

## 8. Functional Requirement Catalog

## 9. Product-Visible Feedback and Recovery

## 10. Completion Criteria

## 11. Downstream Reference Seeds

## 12. Source Traceability

## 13. Readiness for Downstream Documents
```

## Section 1: Product Spec Scope

Summarize the source inputs and ownership.

Use:

```markdown
## 1. Product Spec Scope

Sources Used:
- `docs/review/project-design-brief.md`
- `docs/review/question-resolution.md`
- `docs/review/project-decisions.md`

This document owns:
- requested capability
- current implementation scope
- product-facing requirements
- user flows
- completion criteria

This document does not own:
- API contracts
- domain model
- architecture
- frontend/backend implementation
- execution tasks
- validation commands
```

## Section 2: Requested Capability

State the requested capability clearly.

Use concise language.

Example:

```markdown
## 2. Requested Capability

The requested capability is to provide a web application workflow that lets the user submit source material, start a proposal generation run, monitor progress, inspect generated artifacts, and download the final proposal output.
```

Rules:

```text
Describe only the current requested capability.
Do not include future product direction.
Do not mention MVP.
```

## Section 3: Current Implementation Scope

List what is included in the current implementation pass.

Use:

```markdown
## 3. Current Implementation Scope

The current implementation pass includes:

- ...
```

Rules:

```text
Each item should be implementable.
Each item should map to requirements or flows.
Avoid architecture-level wording unless user-visible behavior depends on it.
```

## Section 4: Scope Boundaries

List explicit boundaries that prevent over-implementation.

Use:

```markdown
## 4. Scope Boundaries

The following are outside the current implementation pass:

- ...
```

Rules:

```text
Only include boundaries that matter.
Do not write a future roadmap.
Do not promise later implementation.
```

## Section 5: User Roles

Define current user roles.

Use:

```markdown
## 5. User Roles

| Role | Description | Current Capabilities |
|---|---|---|
```

If there is only one role, define it clearly.

Avoid adding roles not mentioned or required by the current implementation.

## Section 6: Core User Flows

Define the main user flows.

Use:

```markdown
## 6. Core User Flows

### Core User Flow: <Name>

Start Condition:
- ...

Completion Signal:
- ...

| Step | User Action | System Response | Product Requirement |
|---|---|---|---|
```

Rules:

```text
Core User Flows should map to current implementation scope.
Include system response and completion signal.
Do not reduce flows to UI layout.
```

## Section 7: Supporting Interaction Flows

Define auxiliary flows that help users complete the core flow.

Examples:

```text
replace uploaded file
retry failed run
view validation details
refresh status
download artifact
copy error message
```

Use:

```markdown
## 7. Supporting Interaction Flows

| Flow | Purpose | Trigger | Expected System Response |
|---|---|---|---|
```

## Section 8: Functional Requirement Catalog

Create `REQ-*` entries.

Use this format:

```markdown
## 8. Functional Requirement Catalog

### REQ-001: <Requirement Title>

Type: functional / feedback / recovery / boundary / content / workflow
Priority: required / conditional
User Role:
- ...

Requirement:
- ...

Acceptance Intent:
- ...

Scope Boundary:
- ...

Related Flow:
- ...

Downstream Implications:
- ...
```

### Requirement Type Guidance

Use:

```text
functional = user-visible capability
feedback = visible state/status/progress requirement
recovery = failure or blocked path requirement
boundary = explicit current scope boundary
content = generated or displayed content requirement
workflow = sequence or flow requirement
```

### Priority Guidance

Use:

```text
required
conditional
```

Do not use MoSCoW or roadmap-style priority labels unless the user requested them.

## Section 9: Product-Visible Feedback and Recovery

Summarize product-visible UX behavior.

Use:

```markdown
## 9. Product-Visible Feedback and Recovery

| Scenario | Required Feedback | Recovery Path | Related Requirements |
|---|---|---|---|
```

Include:

```text
pending/submitting feedback
running/progress feedback
validation feedback
success feedback
failure feedback
blocked feedback
artifact availability
retry behavior
```

Rules:

```text
Critical states must include visible text.
Do not rely on color alone.
Do not describe component implementation details.
```

## Section 10: Completion Criteria

Define product-level done conditions.

Use:

```markdown
## 10. Completion Criteria

The current implementation pass is product-complete when:

- ...
```

Completion criteria should be user-visible or behavior-visible.

Do not include final test commands here.

Validation commands belong in:

```text
docs/execution/execution-validation.md
```

## Section 11: Downstream Reference Seeds

List what downstream documents should absorb.

Use:

```markdown
## 11. Downstream Reference Seeds

| Downstream Document | Seed Content |
|---|---|
| `docs/reference/domain-model.md` | ... |
| `docs/reference/data-api-contract.md` | ... |
| `docs/reference/ui/UI_PAGE.yaml` | ... |
| `docs/reference/frontend-design.md` | ... |
| `docs/reference/backend-design.md` | ... |
| `docs/execution/execution-validation.md` | ... |
```

Rules:

```text
Seeds are not final downstream entries.
Do not create API-* or TASK-* IDs here.
```

## Section 12: Source Traceability

Map product requirements to review sources.

Use:

```markdown
## 12. Source Traceability

| Product Item | Source |
|---|---|
| REQ-001 | `docs/review/project-decisions.md#DEC-...` or description |
```

If final DEC IDs are not available, cite the decision title or review source section.

Do not cite `OQ-*` as final source. Use the resolved content from `question-resolution.md`.

## Section 13: Readiness for Downstream Documents

End with:

```markdown
## 13. Readiness for Downstream Documents

Status: ready / blocked

Summary:
- ...

Next Step:
- Continue to `prompts/domain-model-prompt.md`.
```

If blocked, list missing product decisions.

## Blocked Generation

If required product context is missing, output:

```markdown
# Product Spec

## Blocked Product Spec Generation

Status: blocked

Reason:
- ...

Missing Product Decisions:
| Missing Decision | Why Required | Affected Downstream Docs |
|---|---|---|

Next Step:
- Resolve the missing decision in `docs/review/question-resolution.md` or `docs/review/project-decisions.md`, then rerun this prompt.
```

## Path Rules

Use only new document paths:

```text
docs/review/project-design-brief.md
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
```

Do not generate final:

```text
ENT-* entries
BR-* entries
STATE-* entries
ARCH-* entries
API-* entries
DB-* entries
ERR-* entries
TYPE-* entries
FE-* entries
BE-* entries
ENV-* entries
TASK-* entries
VAL-* entries
code
implementation plan
Open Questions section
OQ-* IDs
```

## Final Self-Check

Before finalizing the output, verify:

```text
[ ] The requested capability is clear.
[ ] The document uses current implementation framing, not MVP framing.
[ ] Current implementation scope is explicit.
[ ] Scope boundaries prevent over-implementation without becoming a roadmap.
[ ] Core User Flows are defined.
[ ] Supporting Interaction Flows are defined when relevant.
[ ] Product-visible feedback and recovery behavior are captured.
[ ] REQ-* entries are product-facing and heading-addressable.
[ ] API, DB, frontend, backend, and task details are not defined here.
[ ] No Open Questions or OQ-* IDs appear.
[ ] Downstream seeds are present but not final downstream catalogs.
[ ] Readiness for downstream documents is explicit.
```
