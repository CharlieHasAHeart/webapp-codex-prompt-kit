# Prompt: Generate `dev-environment.md`

Generate `dev-environment.md`.

## Responsibility

`dev-environment.md` defines how Codex installs, runs, tests, builds, migrates, and validates the project.

## Include

- Purpose
- Source of Truth
- Codex Usage
- Non-Goals
- Operating system assumptions
- Runtime versions
- Package manager policy
- Canonical command table
- Allowed commands
- Forbidden commands
- Install commands
- Dev commands
- Build commands
- Lint commands
- Typecheck commands
- Test commands
- E2E commands
- Migration and seed commands
- Environment variables
- Command substitution policy

## Rules

- Put a short `Canonical Commands` table near the top.
- Do not let Codex choose between multiple package managers.
- Do not include product requirements or API contracts.
- Reference `project-decisions.md` for shared validation policy.
- Keep within the dev environment length budget.

Output complete Markdown suitable for `docs/dev-environment.md`.
