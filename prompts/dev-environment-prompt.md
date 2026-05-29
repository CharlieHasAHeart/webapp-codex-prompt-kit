# Dev Environment Prompt

## Purpose

Use this prompt to generate the development environment and command policy catalog for the current implementation.

The dev environment document defines stable `ENV-*` entries for runtime versions, package manager policy, service names, container/local execution policy, install commands, development commands, build commands, test command patterns, lint/typecheck command patterns, environment variable policy, and forbidden host commands.

It is a non-UI reference catalog. It must be ownership-decoupled and entry-self-contained.

It is not a product requirements file, not an implementation design file, not an execution task file, and not a task-specific validation plan.

## Target Output

Generate exactly one document:

```text
docs/reference/dev-environment.md
```

## Standards to Apply

Read only the standards listed below.

| Standard | Required? | Use For |
|---|---:|---|
| `standards/document-responsibilities.md` | yes | Enforces non-UI reference ownership, entry self-containment, and traceability without dependency. |
| `standards/codex-ready-writing-rules.md` | yes | Ensures stable IDs, resolved wording, and Codex-safe command policy entries. |
| `standards/open-questions-policy.md` | yes | Prevents unresolved questions from entering final reference docs. |
| `standards/validation-strategy.md` | yes | Separates command patterns from task-specific validation selection. |
| `standards/document-length-budgets.md` | optional | Use to keep environment entries compact and addressable. |
| `standards/flow-concepts-and-composition.md` | optional | Use only when environment policy must support flow-first execution or foundation readiness. |

## Standard Application Rules

Standards constrain how this prompt generates its target document. Standards do not create additional output targets.

Rules:
1. Read only the standards listed in this prompt.
2. Do not load all standards by default.
3. The current prompt defines the target output and required output structure.
4. Standards define reusable terminology, ownership boundaries, quality rules, and review constraints.
5. Do not copy large sections from standards into the generated document.
6. Do not generate documents requested by a standard unless this prompt explicitly targets them.
7. If required context remains unresolved under the standards, output a blocked-generation report instead of inventing missing decisions.

## Priority Rule

When generating the target document, use this priority order:

1. User-confirmed answers and corrections.
2. This prompt's target output and required output structure.
3. Required standards listed in this prompt.
4. Upstream generated project documents.
5. Prior project discussion.

If a conflict involves unresolved blockers, Open Questions leakage, unsafe scope invention, missing required decisions, or reference ownership dependency, output a blocked-generation report instead of generating a normal final document.

## Required Inputs

Use these upstream documents when available:

```text
docs/review/project-decisions.md
docs/review/question-resolution.md
docs/reference/architecture.md
```

Optional upstream inputs when available:

```text
docs/reference/data-api-contract.md
docs/reference/frontend-design.md
docs/reference/backend-design.md
```

Do not require every reference document to understand this output. The generated environment entries must be self-contained in their own environment and command policy responsibility layer.

## Dev Environment Ownership

`docs/reference/dev-environment.md` owns:

```text
ENV-*
runtime version policy
package manager policy
dependency installation policy
service naming policy
container/local execution policy
development command patterns
build command patterns
test command patterns
lint command patterns
typecheck command patterns
database or migration command patterns when applicable
environment variable policy
secret handling policy
generated artifact and temporary file policy for local execution
forbidden host commands
command safety notes
```

It must not own:

```text
product requirements
domain source definitions
architecture source rules
API request/response source contracts
database schema source definitions
frontend implementation responsibilities
backend implementation responsibilities
task-specific validation selection
execution task sequencing
validation catalog entries
final executable FLOW-*
TASK-*
VAL-*
Open Questions
```

## Reference Decoupling Rules

Because this is a non-UI reference catalog:

1. Every `ENV-*` entry must be entry-self-contained.
2. Related IDs may be included only for traceability.
3. Do not write "see architecture.md for details" as a substitute for environment policy content.
4. Do not copy architecture, frontend, backend, API, or validation source definitions.
5. Do not redefine another document's owned content.
6. Environment entries may mention related flow or execution areas, but must not perform full flow composition or task-specific validation selection.

Allowed:

```markdown
Related Architecture:
- ARCH-001
```

Forbidden:

```markdown
ENV-001 follows ARCH-001. See ARCH-001 for details.
```

## Environment vs Validation Boundary

This document may define command patterns such as:

```text
npm run test
npm run lint
npm run typecheck
docker compose up
docker compose run app npm test
```

This document must not decide which exact command proves completion for a specific `TASK-*`.

Task-specific validation belongs to:

```text
docs/execution/execution-validation.md
```

The validation catalog may reference command patterns defined here, but `dev-environment.md` must not define `VAL-*`.

## Flow-First Support Rules

The environment policy may support flow-first execution by defining:

- minimal project setup commands
- service startup commands
- test command patterns
- build/typecheck/lint command patterns
- container-first or local-first execution boundaries
- safe file/artifact workspace rules

It must not define:

- final `FLOW-*`
- flow slice tasks
- execution order
- task-specific validation mapping

## Required Output Structure

```markdown
# Dev Environment

## 1. Environment Scope

State what this document owns and what it does not own.

## 2. Environment Summary

Summarize the runtime, package, service, and command policy in current-scope terms.

## 3. Environment Policy Catalog

### ENV-001: <Environment Policy Name>

Type:
- runtime / package_manager / install / service / container / local_execution / dev_command / build_command / test_command_pattern / lint_command_pattern / typecheck_command_pattern / env_var / secret / artifact_workspace / forbidden_command / safety

Policy:
- ...

Allowed:
- ...

Forbidden:
- ...

Commands:
```bash
...
```

Applies To:
- ...

Related IDs:
- ...

Out of Scope:
- ...

## 4. Runtime and Package Policy

Define runtime versions, package manager, dependency install behavior, and lockfile expectations.

## 5. Service and Execution Policy

Define service names, startup behavior, container/local execution rules, and process boundaries.

## 6. Command Pattern Catalog

Define command patterns for install, dev, build, lint, typecheck, test, migration, or other environment actions.

Do not select task-specific validation commands.

## 7. Environment Variable and Secret Policy

Define variable names, required/optional status, local defaults, and forbidden secret handling.

Do not include real secrets.

## 8. Artifact and Temporary File Policy

Define local workspace, generated artifacts, cache, temp file, or upload/download folder rules when relevant.

## 9. Forbidden Commands and Safety Rules

List commands Codex must not run and explain why.

## 10. Out-of-Scope Environment Behavior

List environment concerns intentionally excluded from the current implementation.

## 11. Downstream Seeds

List concise seeds for `execution-validation.md` and `AGENTS.md`.

## 12. Final Readiness

Status: ready / blocked

If blocked, list missing decisions and affected downstream documents.
```

## ENV Entry Requirements

Each `ENV-*` entry must include:

```text
ID
name
type
policy
allowed
forbidden
applies to
out-of-scope where useful
```

Optional but useful:

```text
commands
related IDs
flow-first support
security impact
downstream seeds
```

## Command Writing Rules

Use concrete command patterns where known.

Prefer:

```bash
npm install
npm run dev
npm run build
npm run typecheck
npm run lint
npm test
```

or container-first equivalents when the project requires container execution.

Avoid vague commands:

```text
run the tests
start the app
check everything
```

If the command is unknown, write a blocked-generation issue rather than inventing a package manager or runtime.

## Environment Variable Rules

Define variables as policy entries when relevant.

Allowed:

```text
ENV-010: Local database URL policy
Required variable: DATABASE_URL
Local default: provided by docker compose service
Secret? no for local dev placeholder
```

Forbidden:

```text
DATABASE_URL=<real secret>
```

## Blocked Generation Rules

Output a blocked-generation report instead of a normal dev environment document if:

- runtime or package manager is required but unknown
- container/local execution policy is required but unresolved
- service names are required but unresolved
- install/build/test command patterns cannot be safely determined
- secret handling policy is unclear
- environment variables required for current scope are unknown
- unresolved Open Questions would enter the final environment doc

Blocked-generation report structure:

```markdown
# Dev Environment Generation Blocked

## Blocking Issues

| Issue | Decision Needed | Affected Docs | Flow / Execution Impact |
|---|---|---|---|

## Partial Safe Content

## Required User Decisions
```

## Final Checks

Before finalizing, verify:

- No unresolved Open Questions remain.
- No product requirements are defined.
- No API contracts are defined.
- No frontend or backend implementation responsibilities are defined.
- No task-specific validation mapping is defined.
- No `TASK-*`, `VAL-*`, or final executable `FLOW-*` entries are created.
- Every `ENV-*` entry is self-contained.
- Related IDs are traceability hints only.
- The document defines command patterns, not task-specific proof obligations.
