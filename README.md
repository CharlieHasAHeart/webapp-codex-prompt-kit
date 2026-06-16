# WebApp Codex Prompt Kit

A minimal prompt kit for turning Product & UX QA and Technical QA into Codex-ready Web App working documents.

This repository intentionally stays small:

```text
webapp-codex-prompt-kit/
├── .gitignore
├── CHANGELOG.md
├── README.md
└── prompts/
```

The kit does not ship templates, standards, examples, snippets, or migrations. The prompts define the workflow. The generated project contains the working documents.

## Core Direction

The workflow is **QA-driven, UX-first, and Codex-facing**.

- QA files preserve source memory and help fight ChatGPT forgetting.
- Working documents are compact record catalogs for Codex.
- Process checks do not become permanent files.
- Prompt files stay short and do not hide project rules.
- Codex reads only the task-relevant records.

Flow-based questioning may be used during Product & UX QA, but Flow-first is no longer the top-level document architecture.

## Generated Working Directory

A target Web App should use this structure:

```text
target-webapp/
├── AGENTS.md
├── codex-execution-report.md
│
├── docs/
│   ├── notes/
│   │   ├── product-ux-qa/
│   │   │   ├── 00_system.md
│   │   │   ├── 01_auth.md
│   │   │   ├── 02_home_navigation.md
│   │   │   ├── 03_core_workflows.md
│   │   │   ├── 04_feature_modules.md
│   │   │   ├── 05_ai_permissions.md
│   │   │   └── 06_settings_account.md
│   │   │
│   │   └── technical-qa/
│   │       ├── 00_technical_overview.md
│   │       ├── 01_frontend.md
│   │       ├── 02_backend_data.md
│   │       ├── 03_auth_permissions.md
│   │       ├── 04_ai_files.md
│   │       └── 05_deployment_validation.md
│   │
│   ├── product.md
│   ├── ux.md
│   ├── technical.md
│   ├── implementation.md
│   └── execution.md
│
└── src/
```

## File Roles

### `docs/notes/product-ux-qa/`

Source QA for product behavior, UX rules, modules, states, permissions, AI behavior, and user control.

Keep it split by module or topic. Do not store all questions in one file.

### `docs/notes/technical-qa/`

Source QA for stack, architecture, data, auth, AI, files, deployment, and validation.

Keep it split by technical area.

### `docs/product.md`

Codex-facing product records:

```text
PROD-*   product summary or boundary
USER-*   user or role
SCOPE-*  scope item
REQ-*    requirement
ENT-*    domain entity
BR-*     business rule
DEC-*    decision
```

### `docs/ux.md`

Codex-facing UX records:

```text
UXR-*      UX rule
PATTERN-*  interaction pattern
SCREEN-*   screen or route
STATE-*    state behavior
VIS-*      visual rule
A11Y-*     accessibility rule
```

### `docs/technical.md`

Codex-facing technical records:

```text
STACK-*  stack decision
ARCH-*   architecture record
DB-*     data/storage record
API-*    API contract
ERR-*    error contract
AUTH-*   auth/permission implementation
BE-*     backend responsibility
ENV-*    environment command or setup
```

### `docs/implementation.md`

Codex-facing implementation records:

```text
FE-*        frontend implementation
COMP-*      component record
ROUTE-*     route implementation
FORM-*      form behavior implementation
STATEIMPL-* state implementation
AIIMPL-*    AI implementation
FILEIMPL-*  file implementation
```

### `docs/execution.md`

Codex-facing execution records:

```text
MILESTONE-* milestone
TASK-*      implementation task
VAL-*       validation item
BLOCKER-*   blocker rule
```

### `AGENTS.md`

Runtime policy for Codex. It should tell Codex to start from `AGENTS.md` and `docs/execution.md`, then read only records referenced by the current `TASK-*`.

### `codex-execution-report.md`

Execution log for completed tasks, validations, blockers, and handoff notes.

## Workflow

```text
1. Product & UX QA
2. Product & UX Consolidation
3. UX Consistency Check
4. Technical QA
5. Technical Consolidation
6. Reference Alignment
7. Execution Plan
8. AGENTS Runtime Policy
9. Final Readiness Check
```

## Prompt Set

```text
prompts/
├── 01-product-ux-qa.md
├── 02-product-ux-consolidation.md
├── 03-ux-consistency-check.md
├── 04-technical-qa.md
├── 05-technical-consolidation.md
├── 06-reference-alignment.md
├── 07-execution-plan.md
├── 08-agents.md
└── 09-final-readiness-check.md
```

## Record Style

Working docs are not narrative documents. They are compact record catalogs.

Each record should be short and stable:

```markdown
## UXR-001: Unsaved Change Exit Rule

**Type:** UX Rule  
**Status:** Active

**Rule:**  
Explicit cancel discards changes directly. Accidental exits require confirmation when unsaved edits exist.

**Related:**  
- FE-010
- TASK-020
```

Keep files few, records short, IDs stable, and references explicit.

## Codex Reading Rule

Codex should default to:

```text
AGENTS.md
docs/execution.md
```

Then each task decides which records to read:

```text
docs/product.md#REQ-...
docs/ux.md#UXR-...
docs/technical.md#API-...
docs/implementation.md#FE-...
```

QA notes are source memory, not default execution context. Codex should only read `docs/notes/...` when a task explicitly permits source lookup or when a blocker requires clarification.
