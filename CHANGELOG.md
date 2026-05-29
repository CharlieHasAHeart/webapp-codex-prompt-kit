# Changelog

## v0.5.0

### Summary

v0.5.0 is a major Flow-first document-system revision.

This release changes the prompt kit from the v0.4.x execution-spine model into a full Flow-first generation system:

```text
Discovery
→ Question Resolution
→ Project Decisions
→ Non-UI References
→ UI References
→ Flow Composition
→ Execution Validation
→ AGENTS Runtime Policy
→ Cross-Document Review
```

The main change is that UI references are no longer treated as late visual/reference artifacts. They now sit after non-UI references and before flow composition, because user-visible flows must be checked for:

```text
operability
observability
feedback
recovery
artifact visibility
completion signal visibility
```

This release also removes concrete UI styling-stack assumptions from the active system. The UI reference system is now technology-agnostic.

---

## Added

### Flow-first document system

- Added a Flow-first explanation to `README.md`.
- Defined Flow-first as the reason for organizing generation around complete user/system behavior instead of broad technical layers.
- Clarified that Flow-first does not mean foundation-free.
- Clarified that foundation tasks should exist only when they unlock named flows.
- Added stronger language that Codex should implement one validated slice at a time instead of wiring disconnected layers together at the end.

### Open Questions and decision flow

- Added the Open Questions flow as a first-class generation path:
  - `prompts/open-questions-extraction-prompt.md`
  - `prompts/question-resolution-prompt.md`
- Added these outputs to the active generated document system:
  - `docs/review/open-questions-review.md`
  - `docs/review/question-resolution.md`
- Added Open Questions handling to the generation matrix.
- Clarified that unresolved Open Questions must not leak into final reference, UI, or execution documents.

### UI reference system

- Added `standards/ui-reference-system.md`.
- Added a technology-agnostic UI reference model:
  - `UI_PAGE.yaml` = flow-facing semantic UI surface
  - `UI_TOKENS.yaml` = technology-agnostic design token reference
  - `UI_VISUAL_SPEC.yaml` = visual and interaction presentation rules
- Added the rule that every generated UI YAML file must include:
  ```yaml
  codex_consumption:
  ```
- Added Codex runtime consumption rules for UI YAML files.
- Added UI field dictionary behavior through `codex_consumption` rather than a separate generated project dictionary.

### UI authoring specifications

- Added or renamed active UI authoring specs:
  - `standards/ui-authoring-specs/UI_PAGE.yaml-Authoring-Specification.md`
  - `standards/ui-authoring-specs/UI_TOKENS.yaml-Authoring-Specification.md`
  - `standards/ui-authoring-specs/UI_VISUAL_SPEC.yaml-Authoring-Specification.md`
- `UI_PAGE.yaml` authoring now supports:
  - flow surface mapping
  - action effect mapping
  - feedback state mapping
  - recovery path mapping
  - artifact surface mapping
  - completion signal mapping
  - `codex_consumption`
- `UI_TOKENS.yaml` authoring now supports technology-agnostic token intent only.
- `UI_VISUAL_SPEC.yaml` authoring now supports technology-agnostic state, feedback, recovery, artifact, completion, responsive, and accessibility presentation rules.

### README generation matrix

- Added a simplified Generation Matrix to `README.md`.
- The matrix now focuses on:
  - step number
  - prompt file
  - standards to read
  - target output
- Removed duplicated role explanations from the matrix because the README now explains file responsibilities separately.

### Release process

- Added a simplified release command order to `README.md`.
- Added GitHub CLI release creation guidance.
- Standardized on `CHANGELOG.md` as the release note source.
- Added guidance to use pinned tags or commits when stability matters.

---

## Changed

### Overall generation order

The active generation order now becomes:

```text
1. discovery-workshop-prompt.md
2. open-questions-extraction-prompt.md
3. question-resolution-prompt.md
4. project-decisions-prompt.md

5. product-spec-prompt.md
6. domain-model-prompt.md
7. architecture-prompt.md
8. data-api-contract-prompt.md
9. frontend-design-prompt.md
10. backend-design-prompt.md
11. dev-environment-prompt.md

12. ui-page-prompt.md
13. ui-tokens-prompt.md
14. ui-visual-spec-prompt.md

15. flow-composition-review-prompt.md
16. execution-validation-prompt.md
17. AGENTS-prompt.md
18. cross-document-review-prompt.md
```

This replaces the v0.4.x order where UI files existed inside reference generation but did not yet function as the formal flow-facing surface before flow composition.

### Generated document structure

The generated project structure now uses three explicit layers:

```text
docs/review/
docs/reference/
docs/execution/
```

Reference UI files now live under:

```text
docs/reference/ui/
```

Execution files now live under:

```text
docs/execution/
```

The runtime worklog remains:

```text
docs/execution/codex-execution-report.md
```

but it is created and maintained by Codex, not normally generated by ChatGPT.

### `README.md`

- Rewritten into four sections:
  1. Flow-first introduction
  2. Document systems
  3. Generation Matrix
  4. Release Command Order
- Removed the separate “Manual File Upload” and “GitHub Repository Usage” framing.
- Replaced that framing with a normal usage rule:
  - read README
  - identify the current step
  - read the current prompt
  - read only the standards listed by that prompt
  - read only required upstream documents
- Added a repository-based usage pattern without making it a separate mode.

### `standards/document-system.md`

- Updated to include `docs/review/`, `docs/reference/`, and `docs/execution/`.
- Added the UI reference layer under `docs/reference/ui/`.
- Removed the old flat generated-doc structure as the active model.
- Marked `codex-execution-report-format.md` as inactive.
- Marked `shadcn-tailwind-implementation-standard.md` as inactive in the current active system.

### `standards/document-generation-order.md`

- Updated the canonical generation order to include:
  - Open Questions extraction
  - Question resolution
  - UI reference generation before flow composition
  - Flow composition before execution validation
- Clarified regeneration rules for UI changes.
- Clarified that UI must not move after execution.

### `standards/document-responsibilities.md`

- Added UI reference responsibilities.
- Added ownership boundaries for:
  - `UI_PAGE.yaml`
  - `UI_TOKENS.yaml`
  - `UI_VISUAL_SPEC.yaml`
- Added UI technology-agnostic rules.
- Added UI `codex_consumption` requirements.
- Added UI flow support checks.
- Strengthened the distinction between review, non-UI reference, UI reference, and execution documents.

### `standards/webapp-execution-spine.md`

- Reworked from the v0.4.x P0-P10 execution-spine model toward a Flow-first execution spine.
- Execution planning now follows:
  ```text
  minimal foundation tasks
  → executable flow slices
  → cross-flow hardening
  → release validation
  ```
- Added UI work as part of user-visible flow slices.
- Added UI validation claims.
- Added the rule that UI work should not be postponed into a later full-UI phase when required by the current flow.
- Added the rule that broad UI/system/styling tasks are forbidden unless narrowly required by a flow.

### `standards/validation-strategy.md`

- Added UI validation strategy.
- Added validation claims for:
  - action affordance
  - visible pending/running/submitting states
  - failed / blocked / validation_failed states
  - recovery actions
  - artifact surfaces
  - completion signals
  - accessibility and focus behavior
- Clarified that backend success alone is not enough to validate a user-visible flow.
- Added UI evidence types such as frontend interaction tests, component tests, E2E tests, manual smoke evidence, screenshot evidence, accessibility checks, and DOM/state inspection evidence.

### `prompts/frontend-design-prompt.md`

- Updated to consume UI references without redefining them.
- Added `standards/ui-reference-system.md` as a required standard.
- Added explicit boundary rules:
  - frontend design may describe how implementation consumes UI references
  - frontend design must not redefine UI_PAGE, UI_TOKENS, or UI_VISUAL_SPEC
- Removed default styling-stack assumptions.

### `prompts/flow-composition-review-prompt.md`

- Updated to include UI flow-surface review.
- Flow composition now checks:
  - visible UI surface
  - primary action affordance
  - feedback states
  - recovery paths
  - artifact surfaces
  - completion signals
  - UI `codex_consumption`
- Added UI gaps as blockers or warnings.
- Added UI prerequisites to foundation readiness analysis.

### `prompts/execution-validation-prompt.md`

- Updated to generate flow-first execution instead of layer-first execution.
- Added UI task-scoped source behavior.
- Added the requirement that UI tasks read UI YAML `codex_consumption` before modifying UI code.
- Added UI-level validation table expectations.
- Added explicit warning not to assume Tailwind, shadcn/ui, CSS variables, MUI, Chakra, CSS Modules, Styled Components, or any concrete styling stack.

### `prompts/AGENTS-prompt.md`

- Added UI runtime policy.
- Added UI ownership safety policy.
- Added the rule that Codex must read `codex_consumption` for referenced UI YAML files before UI implementation.
- Added the rule that Codex must use existing project stack and code conventions for UI implementation.
- Added worklog expectations for UI source consumption.

### `prompts/cross-document-review-prompt.md`

- Added UI reference review.
- Added UI flow-surface review.
- Added checks for:
  - missing `codex_consumption`
  - UI technology-stack assumptions
  - CSS variable / Tailwind / shadcn leakage
  - UI flow surface gaps
  - UI validation gaps
  - AGENTS UI runtime policy gaps
- Added checks that `codex-execution-report.md` is treated as a runtime worklog, not a prompt-generated source file.

### UI prompts

- `ui-page-prompt.md` now generates a flow-facing semantic UI surface.
- `ui-tokens-prompt.md` now generates technology-agnostic design token intent only.
- `ui-visual-spec-prompt.md` now generates technology-agnostic visual and interaction presentation rules.
- All three UI prompts now require `codex_consumption` in generated YAML.

---

## Removed / Inactive

The following files are no longer part of the active v0.5.0 system:

```text
standards/codex-execution-report-format.md
standards/ui-authoring-strategy.md
standards/ui-authoring-specs/shadcn-tailwind-implementation-standard.md
```

Removal / inactive reasons:

### `codex-execution-report-format.md`

- Runtime worklog behavior is now owned by `AGENTS.md`.
- `codex-execution-report.md` remains active as a Codex-created runtime worklog, but its format is not maintained as a separate active standard.

### `ui-authoring-strategy.md`

- Replaced by the more explicit `standards/ui-reference-system.md`.

### `shadcn-tailwind-implementation-standard.md`

- Removed from the active UI system because UI references are now technology-agnostic.
- A future implementation standard may be introduced separately when a concrete styling stack is selected.

---

## Migration Notes from v0.4.1

### Main migration

v0.4.1 was organized around:

```text
Discovery Workshop
Reference Catalogs
Execution Spine
AGENTS Runtime Policy
Cross-Document Review
```

v0.5.0 changes the active system to:

```text
Discovery
Question Resolution
Project Decisions
Non-UI References
UI References
Flow Composition
Execution Validation
AGENTS Runtime Policy
Cross-Document Review
```

### Prompt order migration

Replace the v0.4.1 prompt order with the v0.5.0 18-step generation matrix in `README.md`.

Important changes:

- Add `open-questions-extraction-prompt.md`.
- Add `question-resolution-prompt.md`.
- Move UI references after all non-UI references and before flow composition.
- Add `flow-composition-review-prompt.md` before `execution-validation-prompt.md`.
- Keep `AGENTS-prompt.md` after `execution-validation-prompt.md`.
- Keep `cross-document-review-prompt.md` as the final review step.

### Path migration

Update generated project paths:

```text
docs/product-spec.md
```

to:

```text
docs/reference/product-spec.md
```

and similarly for non-UI reference files.

Update UI paths:

```text
docs/ui/UI_PAGE.yaml
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
```

to:

```text
docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

Update execution paths:

```text
docs/execution-validation.md
AGENTS.md
codex-execution-report.md
```

to:

```text
docs/execution/execution-validation.md
docs/execution/AGENTS.md
docs/execution/codex-execution-report.md
```

### Execution migration

Replace P0-P10 as the top-level execution structure with Flow-first execution:

```text
minimal foundation tasks
→ executable flow slices
→ cross-flow hardening
→ release validation
```

P0-P10-style thinking may still inform engineering completeness, but it should not be the top-level task structure.

### UI migration

Replace the v0.4.1 UI strategy with the v0.5.0 technology-agnostic UI reference system.

Required changes:

- Add `standards/ui-reference-system.md`.
- Replace older UI authoring spec filenames with:
  - `UI_PAGE.yaml-Authoring-Specification.md`
  - `UI_TOKENS.yaml-Authoring-Specification.md`
  - `UI_VISUAL_SPEC.yaml-Authoring-Specification.md`
- Remove active dependency on `shadcn-tailwind-implementation-standard.md`.
- Ensure every generated UI YAML file includes `codex_consumption`.

### Runtime worklog migration

Do not require `codex-execution-report-format.md`.

Instead:

```text
AGENTS.md defines when and how Codex creates or updates docs/execution/codex-execution-report.md.
```

### README migration

Replace the v0.4.1 README structure with four sections:

```text
1. Why Flow-first
2. Document Systems
3. Generation Matrix
4. Release Command Order
```

---

## Recommended Commit Message

```bash
git commit -m "Release v0.5.0 flow-first UI reference system"
```

## Release Commands

```bash
git status
git diff -- README.md CHANGELOG.md prompts standards
git add README.md CHANGELOG.md prompts standards
git commit -m "Release v0.5.0 flow-first UI reference system"
git tag v0.5.0
git push origin main
git push origin v0.5.0
gh release create v0.5.0 --title "v0.5.0" --notes-file CHANGELOG.md
```
