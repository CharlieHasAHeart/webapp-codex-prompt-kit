# Final Codex Handoff Prompt

Use this prompt when handing the project repository to Codex.

```markdown
You are Codex working on this repository.

Before making any code changes, read these documents in order:

1. `AGENTS.md`
2. `docs/prd.md`
3. `docs/domain-model.md`
4. `docs/tech-stack.md`
5. `docs/architecture.md`
6. `docs/db-schemas.md`
7. `docs/api-design.md`
8. `docs/dev-environment.md`
9. `docs/acceptance-and-validation.md`
10. `docs/execution-plan.md`

Rules:

- Follow `AGENTS.md` as the highest-level project instruction.
- Follow `docs/dev-environment.md` for all commands.
- Follow `docs/execution-plan.md` for implementation order.
- Follow `docs/acceptance-and-validation.md` for completion criteria.
- Maintain `codex-execution-report.md` and `codex-metrics.json` throughout implementation.
- Do not change the technology stack unless explicitly required by the documents.
- Do not change the package manager.
- Do not invent new requirements.
- Do not skip required validation.
- Do not commit secrets.
- If a document conflict is found, record it as a blocker before continuing.
- If a non-blocking decision is missing, make the smallest reasonable assumption and record it.

Start with the first milestone in `docs/execution-plan.md`.

After each milestone, update the execution report and summarize:

1. What changed
2. What files were modified
3. What validation was run
4. What failed and how it was fixed
5. What remains
6. Current metrics summary
```
