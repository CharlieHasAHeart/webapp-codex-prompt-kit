# Web App Execution Spine

## Purpose

This standard defines the default engineering route for generating a complete `docs/execution-validation.md` for a Codex-ready Web App project.

It prevents `TASK-*` generation from covering only visible product features while missing required engineering foundation work.

Use this standard when generating:

```text
docs/execution-validation.md
```

---

## Core Principle

`execution-validation.md` is the primary Codex execution spine.

Codex should execute from:

```text
AGENTS.md
docs/execution-validation.md
```

Other project documents are task-scoped reference catalogs.

Codex should read other documents only when the current `TASK-*` explicitly references a specific heading, ID, or YAML key.

---

## Execution Spine Model

Every Web App project must be evaluated against these phases:

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

Every phase must be marked as one of:

```text
required
conditional
not_applicable
deferred
```

If a phase is not applicable, the generated `execution-validation.md` must explain why.

---

## Phase Rules

### P0 Project Bootstrap

Purpose:

```text
Create the repository and application skeleton required for later tasks.
```

Generate tasks when the repository or app structure is not already complete.

Typical task categories:

```text
repository layout
apps/web skeleton
apps/api skeleton
packages/* skeleton
base configuration files
codex-execution-report.md initialization
root README or project entry files when needed
```

Common references:

```text
docs/project-decisions.md#DEC-*
docs/architecture.md#ARCH-*
docs/dev-environment.md#ENV-*
```

Common validation:

```text
directory existence checks
package script checks
basic command execution checks
review validation for docs-only bootstrap
```

Common missing tasks:

```text
Codex starts implementing features before app skeleton exists.
Shared package directory is omitted.
codex-execution-report.md is not initialized.
```

---

### P1 Development Environment

Purpose:

```text
Make install, start, test, database, and validation commands predictable.
```

Generate tasks when local development or container execution is not fully defined.

Typical task categories:

```text
Docker Compose services
container-first command setup
package manager setup
runtime setup
environment example files
test script skeletons
database service wiring
logs/start/stop commands
```

Common references:

```text
docs/project-decisions.md#DEC-*
docs/architecture.md#ARCH-*
docs/dev-environment.md#ENV-*
```

Common validation:

```text
docker compose config
docker compose up -d
service health command
targeted install/test command dry run
```

Common missing tasks:

```text
Host npm install is used despite container-first policy.
Database service exists but API cannot connect.
Test scripts are referenced before they exist.
```

---

### P2 Shared Contracts and Types

Purpose:

```text
Create shared contract foundations that reduce frontend/backend drift.
```

Generate tasks when the project uses shared types, schemas, API contracts, error envelopes, or pagination models.

Typical task categories:

```text
shared API contract package
shared error envelope types
shared pagination types
shared enum/value-set types
shared schemas
API client base type helpers
shared test utilities when app-agnostic
```

Common references:

```text
docs/data-api-contract.md#TYPE-*
docs/data-api-contract.md#ERR-*
docs/project-decisions.md#DEC-*
docs/architecture.md#ARCH-*
docs/dev-environment.md#ENV-*
```

Common validation:

```text
targeted type/schema tests
package import boundary checks
frontend/backend compile or targeted test when available
```

Common missing tasks:

```text
Frontend and backend each define their own error shape.
Pagination is reimplemented differently per endpoint.
Shared package imports app-specific code.
```

---

### P3 Data Layer

Purpose:

```text
Implement persistence foundations required by product workflows and API contracts.
```

Generate tasks when persistence exists.

Typical task categories:

```text
database client or ORM setup
initial migrations
DB-* implementation
seed data
repository/data access modules
repository tests
data constraints and indexes
```

Common references:

```text
docs/data-api-contract.md#DB-*
docs/domain-model.md#ENT-*
docs/domain-model.md#REL-*
docs/domain-model.md#BR-*
docs/architecture.md#ARCH-*
docs/backend-design.md#BE-*
docs/dev-environment.md#ENV-*
```

Common validation:

```text
migration command
seed command
repository tests
targeted data access tests
```

Common missing tasks:

```text
API tasks assume tables exist before migrations are created.
Seed data is missing for UI/API validation.
Business constraints are not supported by persistence where needed.
```

---

### P4 Backend API Foundation

Purpose:

```text
Create backend runtime foundations before feature endpoints are implemented.
```

Generate tasks when the project includes `apps/api` or equivalent backend runtime.

Typical task categories:

```text
API app bootstrap
route registration
request validation
structured error handling
auth/session placeholder or real auth
permission policy helpers
health endpoint
backend test support
configuration loading
```

Common references:

```text
docs/backend-design.md#BE-*
docs/data-api-contract.md#ERR-*
docs/data-api-contract.md#TYPE-*
docs/architecture.md#ARCH-*
docs/project-decisions.md#DEC-*
docs/dev-environment.md#ENV-*
```

Common validation:

```text
health endpoint test
error handler test
request validation test
backend app bootstrap test
```

Common missing tasks:

```text
Feature handlers are implemented before structured errors exist.
Auth is unclear, so handlers silently assume no auth.
Validation errors do not match documented ERR-* contracts.
```

---

### P5 Backend Feature Workflows

Purpose:

```text
Implement backend services, repositories, handlers, transactions, jobs, and integrations for product workflows.
```

Generate tasks from:

```text
REQ-*
ENT-*
BR-*
STATE-*
DB-*
API-*
ERR-*
BE-*
```

Typical task categories:

```text
service implementation
API handler implementation
repository workflow support
transaction boundary
background job
integration adapter
service tests
API tests
business rule tests
```

Common references:

```text
docs/product-spec.md#REQ-*
docs/domain-model.md#ENT-*
docs/domain-model.md#BR-*
docs/domain-model.md#STATE-*
docs/data-api-contract.md#DB-*
docs/data-api-contract.md#API-*
docs/data-api-contract.md#ERR-*
docs/backend-design.md#BE-*
docs/dev-environment.md#ENV-*
```

Common validation:

```text
targeted service tests
targeted API tests
transaction/conflict tests
integration adapter tests with mocks
```

Common missing tasks:

```text
API handler exists but service rule enforcement is missing.
Business rules are enforced only in frontend.
Transaction-critical workflow is split across non-atomic operations.
```

---

### P6 Frontend App Shell

Purpose:

```text
Create the frontend runtime shell, routing foundation, navigation, and shared UI infrastructure.
```

Generate tasks when the project includes a frontend app.

Typical task categories:

```text
app layout
sidebar/top navigation
page header
route skeletons
frontend API client base
shared loading/empty/error components
auth/permission UI shell
global providers
frontend test support
```

Common references:

```text
docs/frontend-design.md#FE-*
docs/ui/UI_PAGE.yaml#*
docs/project-decisions.md#DEC-*
docs/architecture.md#ARCH-*
docs/data-api-contract.md#API-*
docs/data-api-contract.md#ERR-*
docs/dev-environment.md#ENV-*
```

Common validation:

```text
frontend route test
component test
API client test
render smoke test
```

Common missing tasks:

```text
Feature pages are built without app shell.
Navigation is invented instead of using UI_PAGE.yaml.
API calls are duplicated instead of using API client base.
```

---

### P7 Frontend Feature Workflows

Purpose:

```text
Implement user-facing pages and interactions from UI_PAGE.yaml, FE-* entries, and API contracts.
```

Generate tasks from:

```text
UI pages
UI actions
UI states
FE-*
API-*
ERR-*
REQ-*
```

Typical task categories:

```text
page implementation
forms
data tables/lists
detail pages
create/edit/delete flows
API client calls
frontend state handling
frontend tests
```

Common references:

```text
docs/frontend-design.md#FE-*
docs/ui/UI_PAGE.yaml#*
docs/data-api-contract.md#API-*
docs/data-api-contract.md#ERR-*
docs/product-spec.md#REQ-*
docs/domain-model.md#ENT-*
docs/dev-environment.md#ENV-*
```

Common validation:

```text
page tests
component tests
form behavior tests
API client mock tests
route state tests
```

Common missing tasks:

```text
Page renders happy path only.
Route-backed filters are missing.
Frontend ignores documented error behavior.
```

---

### P8 UI System and Interaction States

Purpose:

```text
Apply UI tokens, visual rules, responsive behavior, accessibility, and complete UI states.
```

Generate tasks when UI exists.

Typical task categories:

```text
apply UI tokens
apply visual spec
responsive behavior
loading states
empty states
error states
permission states
disabled/submitting/success/conflict states
accessibility pass
shadcn/ui and Tailwind boundary checks
```

Common references:

```text
docs/ui/UI_TOKENS.yaml#*
docs/ui/UI_VISUAL_SPEC.yaml#*
docs/ui/UI_PAGE.yaml#*
docs/frontend-design.md#FE-*
docs/data-api-contract.md#ERR-*
docs/dev-environment.md#ENV-*
```

Common validation:

```text
component visual behavior tests
state rendering tests
accessibility checks when available
targeted frontend tests
```

Common missing tasks:

```text
UI only handles ready state.
Empty/error/permission states are not implemented.
Raw colors or ad hoc styles bypass tokens.
Responsive behavior is undefined.
```

---

### P9 Cross-Cutting Hardening

Purpose:

```text
Close gaps that often remain after feature implementation.
```

Generate tasks based on project risk.

Typical task categories:

```text
structured errors across app
permission consistency
duplicate submission prevention
data freshness/stale state
input validation consistency
sensitive data exposure check
logging basics
persistence after refresh
frontend/backend contract drift check
seed/demo data consistency
```

Common references:

```text
docs/domain-model.md#BR-*
docs/architecture.md#ARCH-*
docs/data-api-contract.md#ERR-*
docs/frontend-design.md#FE-*
docs/backend-design.md#BE-*
docs/dev-environment.md#ENV-*
```

Common validation:

```text
targeted regression tests
contract tests
permission tests
error handling tests
manual review validation when appropriate
```

Common missing tasks:

```text
Feature tests pass but permission consistency is not checked.
Duplicate submissions create duplicate records.
Sensitive fields leak through API response.
```

---

### P10 Final Validation and Handoff

Purpose:

```text
Prove MVP readiness and prepare the repository for handoff.
```

Generate tasks for:

```text
milestone validation
release validation
codex-execution-report completion
deferred/skipped task documentation
open question review
handoff readiness
```

Common references:

```text
docs/execution-validation.md#VAL-*
docs/dev-environment.md#ENV-*
AGENTS.md
```

Common validation:

```text
milestone validation commands
release validation commands
final report review
```

Common missing tasks:

```text
No release validation is run.
Failed or skipped tasks are not recorded.
Open blocking questions remain hidden.
```

---

## Task Generation Rules

When generating `execution-validation.md`, create tasks from two sources:

```text
Engineering Spine Tasks
Product Flow Tasks
```

### Engineering Spine Tasks

Generate from this standard's phases.

These tasks make the project buildable, runnable, testable, and maintainable.

Examples:

```text
bootstrap repository
configure container services
create API app bootstrap
create frontend app shell
create shared API client base
create database migrations
create test support
```

### Product Flow Tasks

Generate from the project reference catalogs.

Examples:

```text
implement case list API
implement case detail page
implement create case flow
enforce no concurrent active run rule
render run result state
```

A complete `execution-validation.md` must include both.

---

## Task-Scoped Reference Rules

Every implementation task must include:

```text
Read scope
Read before this task
Implementation Scope
Expected Code Impact
Out of Scope
Required Validation
Completion Rule
```

`Read before this task` should reference specific headings, IDs, or YAML keys.

Good examples:

```text
docs/product-spec.md#REQ-001
docs/domain-model.md#BR-001
docs/data-api-contract.md#API-001
docs/frontend-design.md#FE-003
docs/backend-design.md#BE-004
docs/dev-environment.md#ENV-010
docs/ui/UI_PAGE.yaml#cases_list
```

Avoid:

```text
docs/product-spec.md
docs/domain-model.md
docs/frontend-design.md
```

Full-document reads should be avoided unless the task explicitly requires them.

---

## Validation Rules

Each implementation task must have targeted validation.

Validation must be:

```text
container-first
task-scoped
evidence-driven
minimal but meaningful
```

Task validation should prove the task's claim.

Milestone and release validation may be broader.

Do not require full lint, full typecheck, full build, mypy, or full E2E for every task by default.

---

## Phase Applicability Rules

Every phase must be evaluated.

Use:

```text
required
conditional
not_applicable
deferred
```

Examples:

```text
P3 Data Layer: required if DB-* entries exist.
P5 Backend Feature Workflows: required if API-* or BE-* entries exist.
P6 Frontend App Shell: required if UI_PAGE.yaml exists or frontend UI is in scope.
P8 UI System and Interaction States: required if UI exists.
P10 Final Validation and Handoff: always required.
```

If a phase is skipped, explain why.

---

## Common Incomplete Execution Plans

Watch for these patterns:

```text
feature pages exist but no app shell task
API endpoints exist but no API app bootstrap task
DB contracts exist but no migration task
business rules exist but no backend service validation
UI pages exist but no loading/empty/error state tasks
container-first decision exists but host commands are used
release validation exists but task-level validation is missing
Codex must infer missing tasks from reference catalogs
```

These are review failures.

---

## Quality Checklist for execution-validation.md

Before finalizing `execution-validation.md`, verify:

```text
[ ] P0-P10 phases are evaluated.
[ ] Engineering foundation tasks are included.
[ ] Product workflow tasks are included.
[ ] Not-applicable phases have reasons.
[ ] Every implementation task has task-scoped source references.
[ ] Source references point to specific headings, IDs, or YAML keys.
[ ] Every implementation task has Implementation Scope.
[ ] Every implementation task has Out of Scope.
[ ] Every implementation task has Required Validation.
[ ] Every validation command has a claim proven.
[ ] Validation commands are container-first.
[ ] Broad checks are limited to milestone/release validation unless task-specific.
[ ] Codex does not need to infer tasks from other documents.
[ ] Final validation and handoff tasks exist.
```
