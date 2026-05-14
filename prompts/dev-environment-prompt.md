# Dev Environment Prompt

## Target File

```text
docs/dev-environment.md
```

## Purpose

Generate a compact development environment and command reference catalog for a Codex-ready Web App project.

`dev-environment.md` owns:

```text
ENV-* environment and command entries
container-first command policy
service names
runtime and package manager assumptions
setup/start/stop command patterns
dependency command patterns
database command patterns
test command patterns
milestone/release command patterns
forbidden host commands
open environment questions
```

It exists so `execution-validation.md` can reference precise command and environment entries from `TASK-*`.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Recommended upstream context:

```text
Project Design Brief
docs/product-spec.md
docs/project-decisions.md
docs/architecture.md
docs/data-api-contract.md
docs/frontend-design.md
docs/backend-design.md
current project discussion
uploaded project notes
```

Use `project-decisions.md` for:

- `DEC-*`
- container-first development
- package manager
- runtime choices
- frontend framework
- backend framework
- database choice
- validation policy
- deployment/runtime direction

Use `architecture.md` for:

- `ARCH-*`
- repository layout
- runtime units
- frontend/backend/data boundaries
- service boundaries

Use `data-api-contract.md` for:

- `DB-*`
- database and migration needs
- seed data needs
- API runtime needs

Use `frontend-design.md` for:

- `FE-*`
- frontend app path
- frontend package scripts
- frontend test assumptions

Use `backend-design.md` for:

- `BE-*`
- backend app path
- backend package scripts
- backend test assumptions
- worker/job runtime needs

If upstream documents are unavailable, use available context and state assumptions.

If a command or environment decision is unclear and affects execution tasks, list it under `Open Environment Questions`.

---

## Relevant Standards

Apply only the standards relevant to this document:

```text
standards/document-responsibilities.md
standards/document-length-budgets.md
standards/codex-ready-writing-rules.md
standards/frontend-backend-boundary.md
standards/validation-strategy.md
```

Do not restate these standards in the generated document.

---

## Output Rules

Generate only:

```text
docs/dev-environment.md
```

Do not generate other project documents.

Create only:

```text
ENV-*
```

Do not create:

```text
REQ-*
DEC-*
ENT-*
REL-*
BR-*
STATE-*
ARCH-*
DB-*
API-*
ERR-*
TYPE-*
FE-*
BE-*
TASK-*
VAL-*
```

You may reference existing:

```text
DEC-*
ARCH-*
DB-*
API-*
FE-*
BE-*
```

Every `ENV-*` must be heading-addressable.

Use this heading format:

```markdown
### ENV-001: Environment Item Name
```

Do not write a long environment guide.

Do not define product requirements, frontend/backend implementation, DB/API contracts, task lists, or validation criteria here.

---

## Required Document Structure

Use this structure:

```markdown
# Dev Environment

## Environment Catalog

## Forbidden Host Commands

## Open Environment Questions
```

Do not add extra sections unless they are necessary for the project.

---

## Environment Catalog

Generate compact `ENV-*` entries.

Each entry should be independently readable because `TASK-*` items in `execution-validation.md` will reference individual command/environment entries directly.

Recommended format:

```markdown
### ENV-001: Container-First Command Policy

Kind: command-policy

Rule:
- Install, test, build, migration, seed, and validation commands must run inside containers by default.

Applies To:
- all implementation tasks
- all validation tasks
- Codex execution

Canonical Pattern:
```bash
docker compose exec <service> <command>
```

Allowed:
- Use `docker compose exec <service> ...` when the service is running.
- Use `docker compose run --rm <service> ...` for setup-specific commands when the service is not running.

Forbidden:
- Do not run host-level package install, test, build, migration, or validation commands by default.

Related:
- DEC-002
```

Rules:

- Use `ENV-*` for environment and command entries that later tasks may reference.
- Keep entries compact.
- Include `Kind`.
- Include exact commands or command patterns.
- Include `Allowed` and `Forbidden` where useful.
- Include `Related` IDs where useful.
- Do not include task-specific validation selection.
- Do not create `TASK-*` or `VAL-*`.

Recommended `Kind` values:

```text
command-policy
service
runtime
package-manager
environment-variable
setup-command
start-stop-command
dependency-command
database-command
test-command
milestone-command
release-command
forbidden-command
troubleshooting
```

---

## Recommended Environment Entries

Generate entries only when they apply to the project.

Common entries include:

```text
ENV-001 Container-First Command Policy
ENV-002 Docker Compose Services
ENV-003 Runtime Versions
ENV-004 Package Manager Policy
ENV-005 Environment Variables
ENV-006 First-Time Setup Commands
ENV-007 Start and Stop Commands
ENV-008 Dependency Commands
ENV-009 Database Commands
ENV-010 Frontend Test Command Pattern
ENV-011 Backend Test Command Pattern
ENV-012 Milestone Validation Command Pattern
ENV-013 Release Validation Command Pattern
ENV-014 Optional Heavy Checks
ENV-015 Troubleshooting Notes
```

Do not force all entries if they are not useful.

---

## Entry Guidance

### Container-First Command Policy Entry

Should define:

- commands run inside containers by default
- when to use `docker compose exec`
- when to use `docker compose run --rm`
- host command restrictions

Should reference:

```text
DEC-* container-first decision
ARCH-* runtime/service boundary
```

---

### Docker Compose Services Entry

Should define service names.

Recommended format:

```markdown
### ENV-002: Docker Compose Services

Kind: service

Services:
| Service | Purpose | Related Area |
|---|---|---|
| web | Frontend Web app | apps/web |
| api | Backend API app | apps/api |
| db | Database service | persistence |
```

Rules:

- Use actual service names when known.
- If service names are assumptions, mark them clearly.
- Do not define Docker Compose YAML here unless requested.

---

### Runtime Versions Entry

Should define known runtime versions.

Recommended format:

```markdown
### ENV-003: Runtime Versions

Kind: runtime

Runtimes:
| Runtime | Version | Applies To |
|---|---|---|
| Node.js | 20.x | web, api |
| PostgreSQL | 16.x | db |
```

Rules:

- Use `TBD` if exact versions are unknown.
- Do not invent exact versions unless the project has a standard.

---

### Package Manager Entry

Should define exactly one package manager per app or workspace.

Recommended format:

```markdown
### ENV-004: Package Manager Policy

Kind: package-manager

Policy:
- Use npm for frontend and backend Node.js packages.

Canonical Prefixes:
| Area | Prefix |
|---|---|
| web | `docker compose exec web npm` |
| api | `docker compose exec api npm` |

Forbidden:
- Do not switch to pnpm, yarn, or bun unless DEC-* changes.
```

Rules:

- Do not list multiple package managers as alternatives.
- If backend uses Python, choose one tool such as `uv`, `poetry`, or `pip`.

---

### Environment Variables Entry

Should list local environment variables without secret values.

Recommended format:

```markdown
### ENV-005: Environment Variables

Kind: environment-variable

Variables:
| Variable | Used By | Required? | Notes |
|---|---|---:|---|
| DATABASE_URL | api | yes | Backend database connection string. |
| NEXT_PUBLIC_API_BASE_URL | web | yes | Public API base URL. |
```

Rules:

- Do not include secret values.
- Mark public frontend variables clearly.
- Server-only secrets must not be exposed to frontend.
- Mention `.env.example` if it should exist.

---

### Setup Commands Entry

Should define first-time setup commands.

Recommended format:

```markdown
### ENV-006: First-Time Setup Commands

Kind: setup-command

Commands:
```bash
docker compose up -d
docker compose exec web npm install
docker compose exec api npm install
```
```

Rules:

- Use canonical commands.
- Do not include host install commands.

---

### Start and Stop Commands Entry

Should define start, stop, and logs commands.

Recommended format:

```markdown
### ENV-007: Start and Stop Commands

Kind: start-stop-command

Commands:
```bash
docker compose up -d
docker compose logs -f web
docker compose logs -f api
docker compose down
```
```

---

### Dependency Commands Entry

Should define package install/add commands.

Recommended format:

```markdown
### ENV-008: Dependency Commands

Kind: dependency-command

Commands:
```bash
docker compose exec web npm install
docker compose exec api npm install
docker compose exec web npm install <package>
docker compose exec api npm install <package>
```
```

Rules:

- Use the selected package manager only.

---

### Database Commands Entry

Should define migration, seed, reset, and database test command patterns when persistence exists.

Recommended format:

```markdown
### ENV-009: Database Commands

Kind: database-command

Commands:
```bash
docker compose exec api npm run db:migrate
docker compose exec api npm run db:seed
```

Related:
- DB-001
```

Rules:

- Database commands should run through the backend/API service unless project decisions say otherwise.
- Frontend must not run database commands.
- Do not define schema details here.

---

### Test Command Entries

Create separate entries for frontend and backend when useful.

Recommended format:

```markdown
### ENV-010: Frontend Test Command Pattern

Kind: test-command

Command Pattern:
```bash
docker compose exec web npm run test -- <test-file-or-pattern>
```

Use For:
- frontend components
- frontend pages
- frontend hooks
- frontend API client behavior
```

```markdown
### ENV-011: Backend Test Command Pattern

Kind: test-command

Command Pattern:
```bash
docker compose exec api npm run test -- <test-file-or-pattern>
```

Use For:
- API handlers
- backend services
- repositories
- business rule enforcement
```

Rules:

- Prefer targeted test commands.
- Avoid full test suite as default task validation.
- Full test suite belongs to milestone or release validation unless task-specific.

---

### Milestone Validation Entry

Should define broader but still focused commands.

Recommended format:

```markdown
### ENV-012: Milestone Validation Command Pattern

Kind: milestone-command

Commands:
```bash
docker compose exec api npm run test -- --run
docker compose exec web npm run test -- --run
```
```

Rules:

- Milestone validation is broader than task validation.
- Avoid unrelated heavy checks unless relevant.

---

### Release Validation Entry

Should define final release validation commands.

Recommended format:

```markdown
### ENV-013: Release Validation Commands

Kind: release-command

Commands:
```bash
docker compose exec api npm run test -- --run
docker compose exec web npm run test -- --run
docker compose exec web npm run build
```
```

Rules:

- Include build/typecheck/lint only if project decisions require them for release.
- Do not use release validation as a substitute for task validation.

---

### Optional Heavy Checks Entry

Should list heavy checks that are not task-default.

Examples:

```bash
docker compose exec web npm run lint
docker compose exec web npm run typecheck
docker compose exec web npm run build
docker compose exec api npm run lint
docker compose exec api npm run typecheck
```

Rules:

- Heavy checks must not be required for every task by default.
- Heavy checks may be used for milestone/release validation or when a task specifically needs them.

---

### Troubleshooting Entry

Include only short practical troubleshooting notes.

Recommended format:

```markdown
### ENV-015: Troubleshooting Notes

Kind: troubleshooting

Notes:
| Problem | Likely Cause | Action |
|---|---|---|
| API cannot connect to database. | DB service not ready or DATABASE_URL missing. | Check `docker compose ps` and env vars. |
```

Do not include long debugging guides.

---

## Forbidden Host Commands

List commands Codex must not run on the host by default.

Recommended format:

```markdown
## Forbidden Host Commands

```bash
npm install
npm test
npm run build
pnpm install
yarn install
bun install
pytest
mypy
ruff check .
```
```

Adjust based on the chosen stack.

Rules:

- Include only commands that are actually risky for the project.
- Host commands may be allowed only if an explicit `ENV-*` entry or `DEC-*` decision permits them.

---

## Open Environment Questions

List unresolved environment questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Should backend use npm or uv? | yes | install and test commands |
| What is the exact database version? | no | Docker Compose, local development |
```

Rules:

- Include only questions that affect commands, runtime, services, package manager, env vars, testing, or execution tasks.
- Mark blocking questions clearly.
- Do not hide uncertainty inside `ENV-*` entries.

---

## Catalog Design Rules

The generated file should behave like a task-scoped reference catalog.

This means:

- each `ENV-*` entry must be short enough to read independently
- each `ENV-*` entry must have a stable Markdown heading
- each `ENV-*` entry should include related upstream IDs when useful
- task authors should be able to reference entries like:

```text
docs/dev-environment.md#ENV-001
docs/dev-environment.md#ENV-010
```

Avoid broad environment narrative that Codex would need to read globally.

---

## Writing Rules

- Write a reference catalog, not a long environment guide.
- Use stable heading-addressable `ENV-*` IDs.
- Keep every entry compact and independently readable.
- Use exact commands or exact command patterns.
- Prefer container-first commands.
- Use one package manager per app/workspace.
- Reference existing `DEC-*`, `ARCH-*`, `DB-*`, `API-*`, `FE-*`, and `BE-*` where useful.
- Do not create non-ENV IDs.
- Do not define task-specific validation selection here.
- Do not define product requirements.
- Do not define frontend/backend implementation details.
- Do not define DB/API schemas.
- Do not include secret values.
- Use `Open Environment Questions` for unresolved environment decisions.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] The file is a compact environment and command reference catalog.
[ ] Important environment entries have ENV-* headings.
[ ] Every ENV-* is independently readable.
[ ] IDs are heading-addressable.
[ ] Container-first policy is explicit.
[ ] Docker service names are defined or clearly assumed.
[ ] Package manager choices are explicit.
[ ] Setup commands are exact.
[ ] Start/stop commands are exact.
[ ] Dependency commands are exact.
[ ] Database command patterns are defined when DB exists.
[ ] Test command patterns are targeted.
[ ] Milestone/release command patterns are defined.
[ ] Heavy checks are not task-default.
[ ] Forbidden host commands are listed.
[ ] No TASK/VAL IDs are created.
[ ] No product or implementation design content is included.
[ ] No secrets are included.
```
