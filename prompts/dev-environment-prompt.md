# Dev Environment Prompt

## Target File

```text
docs/dev-environment.md
```

## Purpose

Generate the development environment and command source of truth for a Codex-ready Web App project.

`dev-environment.md` defines how Codex and developers should run setup, install, start, migration, seed, test, build, and validation commands.

It should make command execution predictable, container-first, and low-noise.

It should not define product requirements, frontend design, backend design, database/API contracts, task order, or validation criteria.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Required upstream documents:

```text
docs/product-spec.md
docs/project-decisions.md
docs/architecture.md
docs/data-api-contract.md
docs/frontend-design.md
docs/backend-design.md
```

Use `project-decisions.md` for:

- package manager
- runtime choices
- repository layout
- container-first development decision
- frontend framework
- backend framework
- database choice
- validation strategy decision

Use `architecture.md` for:

- repository layout
- runtime units
- frontend/backend/data boundaries
- service names when implied

Use `data-api-contract.md` for:

- database and migration needs
- seed data needs
- API runtime requirements

Use `frontend-design.md` for:

- frontend app path
- frontend package scripts
- frontend test framework assumptions

Use `backend-design.md` for:

- backend app path
- backend package scripts
- backend test framework assumptions
- job/worker runtime needs

If an upstream document is unavailable, use the available context and state assumptions.

If command choices are blocked by missing information, ask the minimum necessary blocking questions.

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

Do not create new IDs by default.

You may reference existing:

```text
DEC-*
DB-*
API-*
FE-*
BE-*
```

Do not create:

```text
REQ-*
ENT-*
REL-*
BR-*
DB-*
API-*
FE-*
BE-*
VAL-*
TASK-*
```

Do not define product requirements, frontend implementation details, backend implementation details, DB/API schemas, task order, or validation criteria here.

---

## Required Document Structure

Use this structure unless the project clearly needs a small adjustment:

```markdown
# Dev Environment

## Purpose

## Source of Truth

## Codex Usage

## Non-Goals of This Document

## Environment Summary

## Repository Layout Assumptions

## Container-First Policy

## Services

## Runtime Versions

## Package Managers

## Environment Variables

## Setup Commands

## Start and Stop Commands

## Dependency Commands

## Database Commands

## Test Command Patterns

## Task-Scoped Validation Command Patterns

## Milestone Validation Commands

## Release Validation Commands

## Optional Heavy Checks

## Forbidden Host Commands

## Troubleshooting Notes

## Assumptions

## Open Questions
```

---

## Section Rules

### Purpose

State that this document defines the canonical development environment and command rules.

Do not describe feature implementation details.

---

### Source of Truth

State that this document owns:

- container-first command policy
- Docker Compose service names
- runtime versions
- package managers
- setup commands
- start/stop commands
- dependency commands
- migration commands
- seed commands
- test command patterns
- milestone validation command patterns
- release validation command patterns
- optional heavy checks
- forbidden host commands
- environment variable inventory at command level

State that this document does not own:

- product requirements
- domain rules
- frontend design
- backend design
- database schema details
- API contracts
- task definitions
- validation criteria
- Codex execution protocol

---

### Codex Usage

Tell Codex to use this document to:

- select canonical commands
- avoid host-level commands when containers are required
- determine which service to exec into
- run setup, tests, migrations, seeds, builds, and validations consistently
- avoid package-manager drift

Tell Codex that task-specific validation selection belongs in `execution-validation.md`.

---

### Non-Goals of This Document

Explicitly state that this document does not define:

- which tasks to implement
- which validation proves each task
- business acceptance criteria
- API request/response shapes
- database table fields
- frontend component structure
- backend service structure

---

## Environment Summary

Provide a compact overview.

Recommended format:

```markdown
The project uses a container-first development workflow.

Canonical commands run through Docker Compose.

Default app layout:
- `apps/web`
- `apps/api`
- `packages/*`

Codex must use the service-specific commands in this document unless a later document explicitly overrides them.
```

Adjust based on project decisions.

---

## Repository Layout Assumptions

State command-relevant layout only.

Recommended format:

```markdown
| Path | Purpose |
|---|---|
| `apps/web` | Frontend Web app. |
| `apps/api` | Backend API app. |
| `packages/*` | Shared app-agnostic packages. |
```

Do not define full directory trees here.

---

## Container-First Policy

Define the command policy clearly.

Recommended rules:

```markdown
- All install, test, build, migration, seed, and validation commands must run inside containers by default.
- Host-level commands are forbidden unless explicitly listed as allowed.
- Codex must not switch package managers.
- Codex must not install dependencies on the host when the project is container-first.
- Codex must use `docker compose exec <service> ...` when the service is already running.
- Codex may use `docker compose run --rm <service> ...` only when the service is not running or the command is setup-specific.
```

---

## Services

Define Docker Compose services.

Recommended format:

```markdown
| Service | Purpose | Typical Commands |
|---|---|---|
| `web` | Frontend Web app | npm install, npm run dev, npm run test |
| `api` | Backend API app | npm install, npm run dev, npm run test, db commands |
| `db` | Database | database container |
```

Use actual service names from the project if known.

If service names are not known, choose reasonable names and mark them as assumptions.

---

## Runtime Versions

Define known runtime versions.

Recommended format:

```markdown
| Runtime | Version | Applies To |
|---|---|---|
| Node.js | 20.x | `apps/web`, `apps/api` |
| PostgreSQL | 16.x | `db` |
```

If exact versions are unknown, write:

```text
TBD
```

Do not invent exact versions unless the project has a standard.

---

## Package Managers

Define package manager choices.

Recommended format:

```markdown
| Area | Package Manager | Canonical Prefix |
|---|---|---|
| Frontend | npm | `docker compose exec web npm` |
| Backend | npm | `docker compose exec api npm` |
```

Rules:

- Do not list multiple package managers as alternatives.
- If `npm` is chosen, forbid `pnpm`, `yarn`, and `bun` unless explicitly allowed.
- If backend uses Python, define whether the project uses `uv`, `poetry`, or plain `pip`, but choose one.

---

## Environment Variables

List environment variables needed to run locally.

Recommended format:

```markdown
| Variable | Used By | Required? | Notes |
|---|---|---:|---|
| `DATABASE_URL` | `api` | yes | Backend database connection string. |
| `NEXT_PUBLIC_API_BASE_URL` | `web` | yes | Public API base URL for frontend. |
```

Rules:

- Do not include secret values.
- Mark public frontend variables clearly.
- Server-only secrets must not be exposed to frontend.
- If `.env.example` should exist, state that requirement.

---

## Setup Commands

Define first-time setup commands.

Recommended format:

```bash
docker compose up -d
docker compose exec web npm install
docker compose exec api npm install
```

Rules:

- Commands must be canonical.
- Do not include alternative commands unless clearly marked.
- Do not include host-level install commands by default.

---

## Start and Stop Commands

Define start and stop commands.

Recommended format:

```bash
docker compose up -d
docker compose logs -f web
docker compose logs -f api
docker compose down
```

If the app requires separate dev servers inside containers, include those commands.

---

## Dependency Commands

Define dependency installation and update commands.

Recommended format:

```bash
docker compose exec web npm install
docker compose exec api npm install
```

If adding packages:

```bash
docker compose exec web npm install <package>
docker compose exec api npm install <package>
```

Rules:

- Use the selected package manager only.
- Do not use host commands unless explicitly allowed.

---

## Database Commands

Define migration, seed, and reset command patterns.

Recommended format:

```bash
docker compose exec api npm run db:migrate
docker compose exec api npm run db:seed
```

If commands are not yet known, define the expected script names and mark them as assumptions.

Rules:

- Database commands should run through the backend/API service unless project decisions say otherwise.
- Frontend must not run database commands.
- Do not define schema details here.

---

## Test Command Patterns

Define general test command patterns.

Recommended format for Node:

```bash
docker compose exec web npm run test -- <test-file-or-pattern>
docker compose exec api npm run test -- <test-file-or-pattern>
```

Recommended format for Python backend:

```bash
docker compose exec api pytest <test-file-or-pattern>
```

Rules:

- Prefer targeted test commands.
- Avoid full test suite as default task validation.
- Full test suite belongs to milestone or release validation unless task-specific.

---

## Task-Scoped Validation Command Patterns

Define patterns, not task-specific validation selection.

Task-specific validation belongs in `execution-validation.md`.

Recommended format:

```markdown
| Task Type | Command Pattern | Notes |
|---|---|---|
| frontend component | `docker compose exec web npm run test -- <component-test>` | Use for component behavior. |
| frontend page | `docker compose exec web npm run test -- <page-test>` | Use for page states and route behavior. |
| backend service | `docker compose exec api npm run test -- <service-test>` | Use for business rule enforcement. |
| backend API | `docker compose exec api npm run test -- <api-test>` | Use for request/response behavior. |
| database migration | `docker compose exec api npm run db:migrate` | Pair with repository/API test when behavior changes. |
```

Rules:

- Do not assign these to `TASK-*` here.
- Do not define `VAL-*` here.
- Each actual required validation in `execution-validation.md` should include a claim proven.

---

## Milestone Validation Commands

Define broader but still focused commands.

Recommended examples:

```bash
docker compose exec api npm run test -- --run
docker compose exec web npm run test -- --run
```

or for Python backend:

```bash
docker compose exec api pytest tests/api tests/services
docker compose exec web npm run test -- --run
```

Rules:

- Milestone validation should be broader than task validation.
- It should still avoid unrelated heavy checks unless needed.

---

## Release Validation Commands

Define commands for final handoff or release.

Recommended examples:

```bash
docker compose exec api npm run test -- --run
docker compose exec web npm run test -- --run
docker compose exec web npm run build
```

Include typecheck/lint only if project decisions require them for release.

---

## Optional Heavy Checks

List heavy checks that are not task-default.

Examples:

```bash
docker compose exec web npm run lint
docker compose exec web npm run typecheck
docker compose exec web npm run build
docker compose exec api npm run lint
docker compose exec api npm run typecheck
```

For Python backend:

```bash
docker compose exec api ruff check .
docker compose exec api mypy .
```

Rules:

- Heavy checks must not be required for every task by default.
- Heavy checks may be used for milestone/release validation or when a task specifically needs them.

---

## Forbidden Host Commands

List commands Codex must not run on the host by default.

Recommended format:

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

Adjust based on chosen stack.

---

## Troubleshooting Notes

Include only short, practical troubleshooting notes.

Recommended format:

```markdown
| Problem | Likely Cause | Action |
|---|---|---|
| `api` cannot connect to database | DB service not ready or `DATABASE_URL` missing | Check `docker compose ps` and env vars. |
```

Do not include long debugging guides.

---

## Assumptions

List command-related assumptions.

Recommended format:

```markdown
| Assumption | Impact | Confirm Later? |
|---|---|---|
| Docker Compose service names are `web`, `api`, and `db`. | Affects all commands. | yes |
```

---

## Open Questions

List unresolved environment questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Should backend use npm or uv? | yes | install and test commands |
```

---

## Writing Rules

- Use exact commands.
- Prefer container-first commands.
- Use one package manager per app.
- Do not include multiple equivalent command choices.
- Mark unknown command details as assumptions or open questions.
- Do not define task-specific validation here.
- Do not define product requirements.
- Do not define frontend/backend implementation details.
- Do not define DB/API schemas.
- Do not include secrets.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Container-first policy is explicit.
[ ] Docker service names are defined or clearly assumed.
[ ] Package manager choices are explicit.
[ ] Setup commands are exact.
[ ] Start/stop commands are exact.
[ ] Dependency commands are exact.
[ ] Database command patterns are defined when DB exists.
[ ] Test command patterns are targeted.
[ ] Milestone validation commands are defined.
[ ] Release validation commands are defined.
[ ] Heavy checks are not task-default.
[ ] Forbidden host commands are listed.
[ ] No task-specific VAL/TASK content is included.
[ ] No product or implementation design content is included.
```
