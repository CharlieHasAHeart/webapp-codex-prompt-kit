# Final Codex Handoff Prompt

You are Codex working on this repository.

Before making any changes, read these files in order:

1. `AGENTS.md`
2. `docs/project-decisions.md`
3. `docs/prd.md`
4. `docs/domain-model.md`
5. `docs/tech-stack.md`
6. `docs/architecture.md`
7. `docs/db-schemas.md`
8. `docs/api-design.md`
9. `docs/dev-environment.md`
10. `docs/acceptance-and-validation.md`
11. `docs/execution-plan.md`
12. `docs/traceability-matrix.md`

Rules:

- Follow `AGENTS.md` as the highest-level project instruction.
- Use `project-decisions.md` for shared canonical decisions.
- Use `traceability-matrix.md` to connect requirements, domain, DB, API, validation, and tasks.
- Follow `dev-environment.md` for all commands.
- Follow `execution-plan.md` for implementation order.
- Validate against `acceptance-and-validation.md`.
- Do not change the technology stack unless the relevant document is updated first.
- Do not change the package manager.
- Do not invent new requirements.
- Do not skip validation.
- Maintain `codex-execution-report.md`.
- Maintain `codex-metrics.json`.
- If documents conflict, stop and report the conflict before continuing.
- If implementation requires a decision not covered by the documents, make the smallest reasonable assumption and record it.

Start with the first incomplete milestone in `docs/execution-plan.md`.

After each milestone, report:

1. What changed
2. Files modified
3. Validation commands run
4. Metrics updated
5. Remaining work
