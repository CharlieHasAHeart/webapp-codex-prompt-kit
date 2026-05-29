# UI Reference System Standard

## 1. Purpose

This standard defines the UI reference system for Web App development.

It explains how ChatGPT should generate UI reference files and how Codex should consume them during implementation.

The UI reference system is technology-agnostic in this revision. It does not prescribe Tailwind, shadcn/ui, CSS variables, MUI, Chakra, CSS Modules, or any other concrete styling stack.

## 2. Core Principle

UI is the flow-facing shape of the Web App.

This means the UI reference system must describe:

```text
what the user can see
what the user can do
how actions trigger effects
how system status becomes visible
how failure and blocked states are shown
how recovery is offered
where artifacts appear
where completion becomes visible
```

The UI reference system must not be reduced to:

```text
page names
component lists
visual tokens
style preferences
```

A useful UI reference must make user flows operable, observable, recoverable, and complete.

## 3. Generated UI Files

Generated projects should contain exactly three UI reference files:

```text
docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

Do not generate a separate project-level UI dictionary file in this revision.

Instead, each generated UI YAML file must contain its own compact runtime dictionary section:

```yaml
codex_consumption:
  file_role: ...
  source_of_truth:
    - ...
  traceability_only:
    - ...
  codex_should:
    - ...
  codex_must_not:
    - ...
  read_with:
    - ...
```

The full authoring dictionary lives in this standard.

## 4. File Responsibility Model

### 4.1 UI_PAGE.yaml

UI_PAGE.yaml owns the semantic UI surface.

It defines app shell, navigation, routes, pages, sections, actions, states, local UI state, global UI states, semantic component roles, flow surfaces, action effects, feedback surfaces, recovery paths, artifact surfaces, completion signals, and relationships to requirements, APIs, and frontend responsibilities.

It must not define visual token values, raw colors, spacing values, CSS variables, Tailwind classes, React or JSX code, API request/response shapes, backend logic, database schema, business rules owned elsewhere, or concrete styling technology.

### 4.2 UI_TOKENS.yaml

UI_TOKENS.yaml owns technology-agnostic reusable visual tokens.

It defines theme intent, semantic color roles, typography tokens, spacing tokens, radius tokens, border tokens, shadow tokens, layout dimension tokens, breakpoint tokens, motion tokens, z-index tokens, status token roles, and accessibility token roles.

It must not define CSS variable names, Tailwind theme keys, shadcn compatibility, MUI theme mappings, Chakra theme mappings, className strings, component implementation, React or JSX code, page structure, workflow logic, API contracts, or backend behavior.

In this revision, UI_TOKENS.yaml must stay technology-agnostic.

Codex may map token intent to the actual project styling system during implementation, based on the existing codebase and task instructions.

### 4.3 UI_VISUAL_SPEC.yaml

UI_VISUAL_SPEC.yaml owns visual and interaction presentation rules.

It defines visual direction, layout intent, surface hierarchy, navigation presentation, component visual roles, interaction state presentation, feedback state presentation, recovery presentation, artifact presentation, completion signal presentation, responsive behavior, accessibility presentation rules, and status-to-visual-role mapping.

It must not define routes, page section source definitions, raw token values, React or JSX code, API contracts, backend logic, database schema, business workflows, className strings, or concrete styling technology.

## 5. UI Reference Generation Order

UI references should be generated after non-UI reference catalogs and before flow composition.

Recommended position:

```text
product-spec.md
domain-model.md
architecture.md
data-api-contract.md
frontend-design.md
backend-design.md
dev-environment.md

UI_PAGE.yaml
UI_TOKENS.yaml
UI_VISUAL_SPEC.yaml

flow-composition-review.md
execution-validation.md
AGENTS.md
cross-document-review.md
```

Reason:

```text
Non-UI reference catalogs define what the system is.
UI references define how users see and operate the system.
Flow composition checks whether flows are visible, operable, recoverable, and complete.
Execution validation turns the combined reference system into Codex tasks.
```

## 6. Technology-Agnostic Rule

The UI reference system must not assume a concrete frontend styling stack.

Do not assume Tailwind, shadcn/ui, CSS variables, MUI, Chakra, CSS Modules, Styled Components, Vanilla Extract, or plain CSS.

Generated UI references should describe UI semantics and visual intent.

Codex should implement the UI according to existing project stack, existing code conventions, task-scoped instructions, frontend-design.md, and execution-validation.md, while preserving the UI reference intent.

## 7. Codex Consumption Rule

Codex must not infer UI field meaning from field names alone.

When a Codex task references any UI YAML file, Codex must first read that file's codex_consumption section.

Codex should treat:

```text
UI_PAGE.yaml as semantic UI surface
UI_TOKENS.yaml as technology-agnostic token intent
UI_VISUAL_SPEC.yaml as visual and interaction presentation rules
```

Codex must not use UI YAML files to invent API contracts, backend behavior, database schema, product requirements, domain rules, frontend tasks not listed in execution-validation.md, or styling stack decisions that conflict with existing code.

## 8. codex_consumption Required Shape

Every UI YAML file must include:

```yaml
codex_consumption:
  file_role: string
  source_of_truth:
    - string
  traceability_only:
    - string
  codex_should:
    - string
  codex_must_not:
    - string
  read_with:
    - string
```

Optional fields:

```yaml
  missing_field_behavior:
    - string
  implementation_freedom:
    - string
  validation_expectations:
    - string
```

## 9. UI_PAGE.yaml Runtime Dictionary

UI_PAGE.yaml should include:

```yaml
codex_consumption:
  file_role: flow_facing_semantic_ui_surface
  source_of_truth:
    - app shell
    - navigation hierarchy
    - routes
    - pages
    - sections
    - actions
    - UI states
    - local UI state
    - flow surface mapping
    - feedback surface mapping
    - recovery path mapping
    - artifact surface mapping
    - completion signal mapping
  traceability_only:
    - related_requirements
    - calls_api
    - related_frontend_responsibilities
  codex_should:
    - implement pages, sections, actions, and states according to their semantic purpose
    - read referenced API contracts before implementing actions that call APIs
    - preserve the distinction between route-backed state and local UI state
    - make completion signals visible to the user
    - make recovery paths actionable when defined
    - use UI_VISUAL_SPEC.yaml for presentation rules
    - use UI_TOKENS.yaml for token intent
  codex_must_not:
    - infer API request or response shapes from calls_api
    - treat sections as HTML tags or component implementation instructions
    - treat states as visual styling rules
    - hide critical states behind color-only feedback
    - invent routes, pages, or actions outside the current task scope
  read_with:
    - docs/reference/ui/UI_VISUAL_SPEC.yaml
    - docs/reference/ui/UI_TOKENS.yaml
    - docs/reference/frontend-design.md
    - docs/reference/data-api-contract.md when actions call APIs
```

## 10. UI_TOKENS.yaml Runtime Dictionary

UI_TOKENS.yaml should include:

```yaml
codex_consumption:
  file_role: technology_agnostic_design_token_reference
  source_of_truth:
    - semantic token names
    - reusable visual primitives
    - status token roles
    - accessibility token roles
    - spacing, typography, radius, border, shadow, layout, breakpoint, motion, and z-index intent
  traceability_only: []
  codex_should:
    - preserve semantic token intent when implementing UI styling
    - map token roles to the project's actual styling system based on existing code
    - use generic status roles rather than business-specific one-off color names
    - preserve accessibility-related token intent such as focus and contrast
  codex_must_not:
    - assume Tailwind, shadcn/ui, CSS variables, MUI, Chakra, or any specific styling stack
    - create business-specific one-off token names unless required by the UI reference
    - treat this file as component implementation guidance
    - treat tokens as page structure or workflow logic
  read_with:
    - docs/reference/ui/UI_PAGE.yaml
    - docs/reference/ui/UI_VISUAL_SPEC.yaml
    - docs/reference/frontend-design.md
```

## 11. UI_VISUAL_SPEC.yaml Runtime Dictionary

UI_VISUAL_SPEC.yaml should include:

```yaml
codex_consumption:
  file_role: visual_and_interaction_presentation_rules
  source_of_truth:
    - visual direction
    - layout presentation rules
    - surface hierarchy
    - component visual roles
    - interaction state presentation
    - feedback and recovery presentation
    - artifact and completion signal presentation
    - responsive behavior
    - accessibility presentation rules
    - status-to-visual-role mapping
  traceability_only:
    - references to UI_PAGE sections, actions, states, or statuses
    - references to token roles from UI_TOKENS.yaml
  codex_should:
    - apply visual rules to UI_PAGE semantic structures
    - make critical states text-visible, not color-only
    - make blocked states visibly distinct from success states
    - preserve recovery affordances where defined
    - preserve artifact availability and completion signal visibility
    - respect accessibility and responsive behavior requirements
  codex_must_not:
    - duplicate raw token values from UI_TOKENS.yaml
    - create routes or page sections
    - define API contracts or backend behavior
    - output React, JSX, or className strings as the specification
    - assume a concrete styling stack
  read_with:
    - docs/reference/ui/UI_PAGE.yaml
    - docs/reference/ui/UI_TOKENS.yaml
    - docs/reference/frontend-design.md
```

## 12. UI_PAGE Field Dictionary

### meta

Meaning:
- Describes the UI reference file.

Codex should:
- Use it to confirm file identity and project context.

Codex must not:
- Treat it as runtime app content.

### product

Meaning:
- Identifies the app or product represented by the UI file.

Codex should:
- Use it for naming context.

Codex must not:
- Treat it as a product requirements source.

### app_shell

Meaning:
- Persistent UI structure shared across pages.

Codex should:
- Implement or preserve the shell when tasks require page layout or navigation.

Codex must not:
- Add page-specific sections into app shell.

### navigation

Meaning:
- Product navigation hierarchy.

Codex should:
- Implement navigation as movement between surfaces.

Codex must not:
- Treat data-mutating actions as navigation.

### routes

Meaning:
- Addressable URL paths and route-backed state.

Codex should:
- Use route definitions for navigation and shareable state.

Codex must not:
- Place temporary UI state in route query.

### pages

Meaning:
- User-visible page surfaces.

Codex should:
- Implement page goal, primary task, sections, actions, and states.

Codex must not:
- Treat pages as React file paths unless the task and codebase imply that mapping.

### sections

Meaning:
- Semantic page regions.

Codex should:
- Implement visible regions that support the section purpose.

Codex must not:
- Treat section type as HTML tag, CSS class, or component library instruction.

### actions

Meaning:
- User-triggered operations or navigation.

Codex should:
- Implement action affordances and state changes.
- Read referenced API contracts before implementing API calls.

Codex must not:
- Infer API request or response shape from action fields.

### states

Meaning:
- UI states that must be represented or handled.

Codex should:
- Make relevant states visible according to UI_VISUAL_SPEC.yaml.

Codex must not:
- Use color alone for critical states.

### local_state

Meaning:
- UI-only state that should not be URL-addressable.

Codex should:
- Keep local state local unless reference docs say otherwise.

Codex must not:
- Persist or route-back temporary UI-only state.

### flow_surface_mapping

Meaning:
- Maps user flows to UI surfaces.

Codex should:
- Use it to know which UI areas support a task's flow.

Codex must not:
- Treat it as final execution task structure.

### action_effect_mapping

Meaning:
- Describes what user actions are expected to trigger.

Codex should:
- Use it to connect action, state transition, API call, feedback, and recovery behavior.

Codex must not:
- Redefine backend side effects or API contracts.

### feedback_state_mapping

Meaning:
- Maps flow or action states to UI-visible feedback.

Codex should:
- Implement visible feedback for required states.

Codex must not:
- Hide required feedback in logs or invisible state only.

### recovery_path_mapping

Meaning:
- Defines user-visible recovery options.

Codex should:
- Implement recovery affordances when tasks include the related flow.

Codex must not:
- Invent recovery behavior beyond documented scope.

### artifact_surface_mapping

Meaning:
- Defines where generated or uploaded artifacts appear.

Codex should:
- Implement artifact availability, preview, download, or error states as specified.

Codex must not:
- Invent artifact contracts or storage behavior.

### completion_signal_mapping

Meaning:
- Defines how the UI shows that a flow is complete.

Codex should:
- Make completion visible and testable.

Codex must not:
- Treat backend success alone as user-visible completion.

## 13. UI_TOKENS Field Dictionary

### theme

Meaning:
- Describes mode intent such as light-only, dark-only, light/dark, or system.

Codex should:
- Preserve the mode intent using the existing project styling system.

Codex must not:
- Invent a full theme switching system unless requested by tasks.

### color.semantic

Meaning:
- Defines semantic color roles, not implementation classes.

Codex should:
- Preserve roles like background, foreground, primary, muted, success, warning, destructive, and info.

Codex must not:
- Create page-specific or business-specific token names as substitutes.

### typography

Meaning:
- Defines reusable type roles and scale.

Codex should:
- Apply typography intent through the existing styling approach.

Codex must not:
- Treat typography tokens as page-specific layout instructions.

### spacing

Meaning:
- Defines reusable spacing scale and semantic spacing roles.

Codex should:
- Preserve spacing consistency.

Codex must not:
- Hardcode inconsistent one-off spacing where token intent is clear.

### radius, border, shadow

Meaning:
- Define reusable shape, border, and elevation intent.

Codex should:
- Use them to preserve surface and component consistency.

Codex must not:
- Treat them as component implementation source by themselves.

### layout

Meaning:
- Defines reusable dimensions or layout roles.

Codex should:
- Use it to guide shell, container, panel, or surface sizing.

Codex must not:
- Treat layout tokens as page structure.

### breakpoint

Meaning:
- Defines responsive scale intent.

Codex should:
- Use existing responsive system to satisfy responsive behavior.

Codex must not:
- Assume a specific breakpoint implementation library.

### motion

Meaning:
- Defines motion duration and easing intent.

Codex should:
- Use motion to support feedback and usability.

Codex must not:
- Add decorative animation that conflicts with visual spec.

### z_index

Meaning:
- Defines stacking roles.

Codex should:
- Preserve predictable overlay, dialog, drawer, and toast stacking.

Codex must not:
- Use arbitrary stacking values without reason.

### status_roles

Meaning:
- Defines generic visual roles for statuses.

Codex should:
- Map workflow statuses through UI_VISUAL_SPEC.yaml status mapping.

Codex must not:
- Create business-specific colors in the token file.

### accessibility

Meaning:
- Defines token intent for focus, contrast, and reduced motion.

Codex should:
- Preserve accessibility intent during implementation.

Codex must not:
- Ignore focus-visible or contrast intent.

## 14. UI_VISUAL_SPEC Field Dictionary

### visual_direction

Meaning:
- Defines overall visual temperament.

Codex should:
- Preserve the product's visual tone.

Codex must not:
- Treat it as a component implementation recipe.

### token_usage

Meaning:
- Describes how token roles should be applied.

Codex should:
- Use it to connect visual rules to token roles.

Codex must not:
- Duplicate raw token values.

### layout

Meaning:
- Defines layout presentation intent.

Codex should:
- Apply layout behavior to UI_PAGE surfaces.

Codex must not:
- Create routes or page structure here.

### surfaces

Meaning:
- Defines visual surface hierarchy.

Codex should:
- Use it to distinguish page, panel, card, dialog, popover, and other surfaces.

Codex must not:
- Treat every section as a card unless specified.

### navigation

Meaning:
- Defines navigation presentation rules.

Codex should:
- Make selected, hover, collapsed, and mobile states visually clear when applicable.

Codex must not:
- Redefine navigation hierarchy.

### components

Meaning:
- Defines component visual roles, not component source code.

Codex should:
- Use these rules when implementing reusable or feature components.

Codex must not:
- Treat the spec as JSX or className output.

### states

Meaning:
- Defines interaction and workflow state presentation.

Codex should:
- Make critical states text-visible and distinguishable.

Codex must not:
- Use color alone for critical state communication.

### responsive

Meaning:
- Defines responsive behavior intent.

Codex should:
- Implement responsive behavior using the project's existing styling approach.

Codex must not:
- Assume Tailwind or any specific breakpoint syntax.

### accessibility

Meaning:
- Defines accessibility presentation requirements.

Codex should:
- Preserve focus, contrast, keyboard, status text, and reduced motion expectations.

Codex must not:
- Implement inaccessible state presentation.

### status_mapping

Meaning:
- Maps workflow statuses to generic visual roles.

Codex should:
- Use it for badges, panels, labels, and other status presentations.

Codex must not:
- Create new business-specific token roles when generic roles suffice.

## 15. Missing Field Behavior

If a required UI reference field is missing:

```text
ChatGPT should block generation or record the missing decision.
Codex should record a blocker during implementation if the field is required for the task.
```

If an optional UI reference field is missing:

```text
ChatGPT may omit it when it is not relevant.
Codex may follow existing project conventions while preserving documented UI intent.
```

Codex must not invent missing product or contract behavior to fill UI gaps.

## 16. Flow-Facing UI Checks

Before finalizing UI references, verify:

```text
Every primary user flow has a visible UI surface.
Every important action has an affordance.
Every important action effect has feedback.
Every critical state has visible presentation.
Every recovery path has a user action or explanation.
Every generated artifact has a surface or explicit out-of-scope status.
Every completion signal is visible to the user.
```

## 17. Final Rule

The UI reference system defines what the UI must mean and express.

It does not define the concrete frontend styling stack.

Codex implements the UI using the existing project stack and code conventions while preserving:

```text
flow surface
action semantics
state feedback
recovery behavior
artifact visibility
completion signal visibility
visual intent
accessibility intent
technology-agnostic token intent
```
