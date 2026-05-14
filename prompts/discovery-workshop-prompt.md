# Discovery Workshop Prompt

## Target Output

```text
Project Design Brief
```

This prompt does not generate a final Codex runtime document by default.

It helps ChatGPT and the user think through the project before generating reference catalogs and the execution spine.

---

## Purpose

Use this prompt before generating the formal project documents.

The goal is to turn an early Web App idea into structured project context that is mature enough to generate:

```text
reference catalogs
execution-validation.md
AGENTS.md
```

This prompt should help discover:

- product boundaries
- MVP workflows
- non-goals
- domain concepts
- data and API needs
- frontend page structure
- backend workflow needs
- UI direction
- engineering constraints
- execution risks
- missing decisions

The output is thinking support for ChatGPT and the user.

It should not be treated as a Codex execution document.

---

## How to Use

Use the available conversation context.

If the user has already described the project, analyze that content directly.

If key information is missing, ask a small number of high-impact questions.

Do not ask for every detail before helping.

Prefer this workflow:

```text
1. Summarize current understanding.
2. Identify missing or risky areas.
3. Ask focused questions if needed.
4. Propose assumptions where reasonable.
5. Produce a compact Project Design Brief.
6. State whether the project is ready for document generation.
```

---

## Output Rules

Generate a compact working brief.

Do not generate final project files such as:

```text
product-spec.md
project-decisions.md
domain-model.md
data-api-contract.md
execution-validation.md
AGENTS.md
```

Do not create formal IDs by default:

```text
REQ-*
DEC-*
ENT-*
BR-*
DB-*
API-*
FE-*
BE-*
TASK-*
VAL-*
```

Temporary labels are allowed when useful, but formal stable IDs should be created by the later catalog prompts.

---

## Required Output Structure

Use this structure:

```markdown
# Project Design Brief

## Current Understanding

## Product Boundary

## Target Users

## MVP Workflows

## Explicit Non-Goals

## Candidate Project Decisions

## Domain Notes

## Data and API Notes

## Frontend and UI Notes

## Backend Notes

## Engineering and Environment Notes

## Execution Risks

## Open Questions

## Readiness Checklist

## Recommended Next Step
```

---

## Section Guidance

### Current Understanding

Summarize the project in a short paragraph.

Answer:

```text
What is the product?
Who is it for?
What is the MVP trying to accomplish?
```

Avoid marketing language.

### Product Boundary

Clarify what belongs in the MVP.

Recommended format:

```markdown
| Area | In MVP? | Notes |
|---|---:|---|
| Case management | yes | Core workflow. |
| Export | no | Future scope unless requested. |
```

### Target Users

Identify real user types.

Recommended format:

```markdown
| User Type | Goal | Main Workflow |
|---|---|---|
| Analyst | Create and evaluate records. | Create, edit, run, review. |
```

Avoid vague user types unless their responsibilities are defined.

### MVP Workflows

List the main user-visible and system-critical workflows.

Recommended format:

```markdown
| Workflow | User/System | Outcome | Notes |
|---|---|---|---|
| Create case | user | A new case exists and can be opened. | Requires persistence. |
```

Include system workflows such as background jobs, imports, or calculations when relevant.

### Explicit Non-Goals

State what should not be built.

Recommended format:

```markdown
| Non-Goal | Reason | Future? |
|---|---|---|
| Enterprise SSO | Not needed for MVP. | yes |
```

This prevents overbuilding later.

### Candidate Project Decisions

Capture early technical and execution choices.

These are candidates only. They will later become formal decisions in `project-decisions.md`.

Recommended areas:

- repository layout
- package manager
- container-first development
- frontend framework
- backend framework
- database
- ORM/query layer
- API style
- UI stack
- auth direction
- testing tools
- deployment target

Recommended format:

```markdown
| Area | Candidate Decision | Confidence | Notes |
|---|---|---|---|
| Repository | `apps/web`, `apps/api`, `packages/*` | high | Default Web App monorepo layout. |
| Development | Container-first | high | Commands should run through Docker. |
```

### Domain Notes

Identify core domain concepts and rules.

Recommended prompts:

```text
What entities exist?
Which states matter?
Which business rules must not be violated?
Which terms are easy to confuse?
Who owns what?
```

Recommended format:

```markdown
| Concept | Meaning | Notes |
|---|---|---|
| Case | Main record being evaluated. | Likely persisted. |
```

### Data and API Notes

Identify what must be persisted and how frontend/backend may communicate.

Recommended prompts:

```text
What data must be stored?
What data is derived?
What list/detail/update/create APIs are likely needed?
What errors matter?
What permission checks matter?
What data must not be exposed?
```

Recommended format:

```markdown
| Area | Need | Notes |
|---|---|---|
| Persistence | Store cases and results. | Requires database schema. |
| API | List cases and get case detail. | Requires pagination if list grows. |
```

### Frontend and UI Notes

Identify pages, navigation, and UI states.

Recommended prompts:

```text
What pages exist?
What navigation model is expected?
What page states must be handled?
What modern Web App details matter?
What UI patterns should be used?
```

Consider:

- app shell
- collapsible sidebar
- lucide icons
- page header
- breadcrumbs
- filter toolbar
- data table
- detail page
- forms
- dialogs
- loading states
- empty states
- error states
- permission states
- responsive behavior

Recommended format:

```markdown
| Page / UI Area | Purpose | Notes |
|---|---|---|
| Case List | Browse and filter cases. | Needs loading, empty, error, ready states. |
```

### Backend Notes

Identify backend workflows and risks.

Recommended prompts:

```text
Which workflows need services?
Which operations need transactions?
Which operations need idempotency?
Which operations can fail?
Are background jobs needed?
Are integrations needed?
What errors should be structured?
```

Recommended format:

```markdown
| Backend Area | Need | Notes |
|---|---|---|
| Case service | Enforce case rules. | Should be tested directly. |
```

### Engineering and Environment Notes

Identify engineering constraints.

Recommended prompts:

```text
Is the project container-first?
Which package manager should be used?
Which runtime versions matter?
Which test tools are expected?
Which commands should Codex use?
What should Codex never run on the host?
```

Recommended format:

```markdown
| Area | Decision / Need | Notes |
|---|---|---|
| Commands | Container-first. | Use Docker Compose service commands. |
```

### Execution Risks

Identify risks that may cause Codex to miss tasks or overbuild.

Common risks:

- missing project bootstrap tasks
- missing Docker setup tasks
- missing API client base
- missing test setup
- missing seed data
- missing structured error handling
- missing loading/empty/error UI states
- frontend/backend contract drift
- DB/API mismatch
- auth ambiguity
- package manager drift
- task list covers features but not complete Web App foundation

Recommended format:

```markdown
| Risk | Impact | Mitigation |
|---|---|---|
| TASK list may only cover product flows. | Codex may miss app bootstrap/setup work. | Use Web App Execution Spine in execution-validation.md. |
```

### Open Questions

Ask only questions that affect document generation or execution.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Is authentication required in MVP? | yes | API, backend, frontend, validation |
```

### Readiness Checklist

Use this checklist before generating formal documents.

```markdown
| Check | Status | Notes |
|---|---|---|
| MVP workflows are named. | yes/no/partial |  |
| Non-goals are explicit. | yes/no/partial |  |
| Core domain concepts are identified. | yes/no/partial |  |
| Persistence needs are identified. | yes/no/partial |  |
| Core API interactions are identified. | yes/no/partial |  |
| Frontend pages are identified. | yes/no/partial |  |
| UI states are identified. | yes/no/partial |  |
| Backend workflows are identified. | yes/no/partial |  |
| Auth/permission stance is clear. | yes/no/partial |  |
| Container/package manager stance is clear. | yes/no/partial |  |
| Execution risks are listed. | yes/no/partial |  |
```

### Recommended Next Step

End with one of:

```text
Ready to generate product-spec.md.
Needs more discussion before generating documents.
Ready with assumptions.
```

If ready, say:

```text
Next prompt: product-spec-prompt.md
```

---

## Writing Rules

- Think broadly, then summarize compactly.
- Do not generate final reference catalogs.
- Do not create formal stable IDs by default.
- Prefer tables.
- Make assumptions explicit.
- Separate MVP from future scope.
- Identify risks that could cause incomplete TASK coverage.
- Keep the brief useful for generating the next prompt.
- Do not include long implementation details.
- Do not write code.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Project summary is clear.
[ ] MVP workflows are listed.
[ ] Non-goals are explicit.
[ ] Candidate technical decisions are captured.
[ ] Domain notes are sufficient for later domain catalog generation.
[ ] Data/API notes are sufficient for later contract catalog generation.
[ ] Frontend/UI notes are sufficient for UI_PAGE generation.
[ ] Backend notes identify services, rules, transactions, or jobs where relevant.
[ ] Engineering constraints include container/package/test assumptions.
[ ] Execution risks include missing-task risks.
[ ] Readiness status is clear.
```
