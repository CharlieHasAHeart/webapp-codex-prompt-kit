# UI_PAGE.yaml Authoring Specification

## 1. Purpose

This document defines the authoring rules for `UI_PAGE.yaml`.

`UI_PAGE.yaml` is a semantic page-structure DSL used to describe product UI information architecture, navigation hierarchy, routes, page sections, actions, page states, and local UI states.

It is intended to be read by:

- product engineers
- frontend developers
- designers
- AI coding agents
- UI generation tools
- documentation and review agents

The goal of this specification is to make `UI_PAGE.yaml` consistent, predictable, and implementation-ready without turning it into HTML, React code, Tailwind code, or a visual design file.

---

## 2. Scope

`UI_PAGE.yaml` describes **what the UI is structurally**, not exactly how it is implemented.

It covers:

- product-level UI structure
- app shell structure
- sidebar navigation
- route mapping
- page definitions
- page goals
- primary user tasks
- sections
- actions
- state modeling
- local UI state modeling
- component responsibilities
- authoring constraints
- validation rules

It does not cover:

- color values
- typography values
- spacing token values
- Tailwind utility classes
- shadcn/ui component source code
- React hooks
- API request implementation
- database schema
- business logic implementation

Those concerns should be handled by separate documents such as:

- `UI_TOKENS.yaml`
- `UI_VISUAL_SPEC.yaml`
- API documentation
- frontend implementation documentation

---

## 3. Normative Keywords

This document uses the following normative keywords:

| Keyword | Meaning |
|---|---|
| `MUST` | Required. The rule is mandatory. |
| `MUST NOT` | Prohibited. The rule must not be violated. |
| `SHOULD` | Recommended. The rule should be followed unless there is a clear reason not to. |
| `SHOULD NOT` | Discouraged. Avoid unless there is a clear reason. |
| `MAY` | Optional. Allowed but not required. |

---

## 4. Core Positioning

`UI_PAGE.yaml` is a semantic UI structure document.

It is not:

- HTML
- JSX
- React component code
- Tailwind class composition
- CSS
- a visual token file
- a final screenshot description
- a component implementation file

The intended pipeline is:

```text
Product requirement
  -> UI_PAGE.yaml
  -> UI visual specification
  -> prototype / implementation
  -> production UI
````

`UI_PAGE.yaml` should answer:

* What pages exist?
* What is each page for?
* What is the primary task on each page?
* What routes map to each page?
* What navigation hierarchy exists?
* What sections exist on each page?
* What actions can the user perform?
* What states can the page enter?
* Which states belong in the URL?
* Which states are local UI-only state?

---

## 5. File-level Structure

A complete `UI_PAGE.yaml` file SHOULD use the following top-level structure:

```yaml
meta:
  name: UI_PAGE
  project: string
  version: 1
  purpose: string

product:
  id: string
  name: string
  description: string

app_shell:
  layout: object
  regions: array

sidebar:
  primary_navigation: array

routes:
  - id: string
    path: string
    page: string
    query: object

pages:
  - page: string
    goal: string
    primary_task: string
    route_ref: string
    sections: array
    actions: object
    states: array
    local_state: array

components:
  component_id:
    purpose: string

global_states:
  state_id:
    purpose: string

global_actions:
  action_id:
    type: action | navigation
```

---

## 6. Required Top-level Fields

A valid `UI_PAGE.yaml` document MUST include:

```yaml
meta:
product:
sidebar:
routes:
pages:
```

A valid `UI_PAGE.yaml` document SHOULD include:

```yaml
app_shell:
components:
global_states:
global_actions:
```

Optional fields MAY be omitted only if they are not needed for the product or current scope.

---

## 7. Top-level Field Definitions

## 7.1 `meta`

### Purpose

`meta` describes the DSL document itself.

### Required

Yes.

### Example

```yaml
meta:
  name: UI_PAGE
  project: license-forge
  version: 1
  purpose: >
    Define semantic page structure, navigation hierarchy, routes, sections,
    actions, and page states for the license-forge platform.
```

### Field Rules

| Field     | Type   | Required | Description                             |
| --------- | ------ | -------: | --------------------------------------- |
| `name`    | string |      yes | Must be `UI_PAGE`.                      |
| `project` | string |      yes | Project identifier.                     |
| `version` | number |      yes | DSL version.                            |
| `purpose` | string |      yes | Short description of this UI_PAGE file. |

---

## 7.2 `product`

### Purpose

`product` describes the product or platform being modeled.

### Required

Yes.

### Example

```yaml
product:
  id: license-forge
  name: License Forge
  description: Offline license issuing and management platform.
```

### Field Rules

| Field         | Type   | Required | Description                  |
| ------------- | ------ | -------: | ---------------------------- |
| `id`          | string |      yes | Stable product ID.           |
| `name`        | string |      yes | Human-readable product name. |
| `description` | string |      yes | Product description.         |

---

## 7.3 `app_shell`

### Purpose

`app_shell` describes the persistent application frame.

It may include:

* sidebar
* top bar
* main content area
* persistent regions
* desktop/mobile shell behavior

### Required

Recommended.

### Example

```yaml
app_shell:
  layout:
    type: sidebar_main
    sidebar:
      width: fixed
      behavior:
        desktop: persistent
        mobile: collapsible
    main:
      width: fill
  regions:
    - sidebar
    - top_bar
    - main_content
```

### Rules

* `app_shell` MUST describe persistent layout regions only.
* `app_shell` MUST NOT contain page-specific sections.
* `app_shell` MUST NOT include Tailwind classes or HTML tags.

---

## 7.4 `sidebar`

### Purpose

`sidebar` describes product navigation hierarchy.

### Required

Yes, if the product has sidebar navigation.

### Example

```yaml
sidebar:
  primary_navigation:
    - id: dashboard
      label: Dashboard
      route: /dashboard

    - id: applications
      label: Applications
      route: /applications
      children:
        - list
        - create
        - detail
```

### Rules

* `primary_navigation` represents top-level product navigation.
* `children` represents secondary navigation under a primary item.
* `children` expresses hierarchy, not a specific UI widget.
* Navigation MUST be used for moving through product structure.
* Actions MUST NOT be modeled as sidebar navigation items.

---

## 7.5 `routes`

### Purpose

`routes` defines URL mappings.

Routes describe addressable page state, not the entire page structure.

### Required

Yes.

### Example

```yaml
routes:
  - id: licenses_list
    path: /licenses
    page: licenses-list
    query:
      q: string
      status:
        - issued
        - replaced
        - expired
        - revoked
      page: number
      page_size: number
```

### Field Rules

| Field   | Type   | Required | Description              |
| ------- | ------ | -------: | ------------------------ |
| `id`    | string |      yes | Stable route ID.         |
| `path`  | string |      yes | URL path.                |
| `page`  | string |      yes | Referenced page ID.      |
| `query` | object |       no | Addressable query state. |

### Rules

* Every `routes[].page` MUST reference an existing `pages[].page`.
* Every `pages[].route_ref` MUST reference an existing `routes[].id`.
* Route query fields MUST represent addressable state.
* Temporary UI state MUST NOT be placed in `route.query`.

---

## 7.6 `pages`

### Purpose

`pages` defines the semantic page structure.

### Required

Yes.

### Example

```yaml
pages:
  - page: licenses-list
    goal: browse and manage issued licenses
    primary_task: find licenses by application, customer, status, or expiration
    route_ref: licenses_list
    sections:
      - id: header
        type: page_header
      - id: filters
        type: filter_bar
      - id: licenses_table
        type: data_table
    states:
      - loading
      - ready
      - empty
      - error
```

### Required Fields

Each page MUST include:

| Field          | Type   | Required | Description                              |
| -------------- | ------ | -------: | ---------------------------------------- |
| `page`         | string |      yes | Stable page ID.                          |
| `goal`         | string |      yes | The page’s user-facing purpose.          |
| `primary_task` | string |      yes | The main task users perform on the page. |
| `route_ref`    | string |      yes | Reference to `routes[].id`.              |
| `sections`     | array  |      yes | Page section structure.                  |
| `states`       | array  |      yes | Supported page states.                   |

Each page MAY include:

| Field         | Type   | Description                           |
| ------------- | ------ | ------------------------------------- |
| `actions`     | object | Page-level actions.                   |
| `local_state` | array  | UI-only state not represented in URL. |
| `layout`      | object | Semantic layout hints.                |

---

## 7.7 `components`

### Purpose

`components` defines reusable semantic component responsibilities.

It does not define implementation code.

### Recommended

Yes.

### Example

```yaml
components:
  page_header:
    purpose: provide page title, description, breadcrumb, and page-level actions
    recommended_slots:
      - title
      - description
      - metadata
      - actions

  data_table:
    purpose: show dense operational records
    interaction:
      row_click: optional
      row_actions: optional
      pagination: route_backed
```

### Rules

* Component entries SHOULD describe responsibility and usage.
* Component entries MUST NOT include JSX.
* Component entries MUST NOT include Tailwind classes.
* Component entries MUST NOT encode implementation-specific DOM nesting.

---

## 7.8 `global_states`

### Purpose

`global_states` defines reusable UI state meanings.

### Recommended

Yes.

### Example

```yaml
global_states:
  loading:
    purpose: data or action is in progress
    presentation: skeleton_or_spinner

  empty:
    purpose: no records match the current page or filters
    presentation: empty_state_with_relevant_action
```

### Rules

* State names SHOULD be stable semantic names.
* State definitions SHOULD describe meaning, not implementation details.
* State presentation MAY be expressed semantically.

---

## 7.9 `global_actions`

### Purpose

`global_actions` defines reusable action semantics.

### Recommended

Yes.

### Example

```yaml
global_actions:
  create_license:
    type: navigation
    target: license-create

  issue_license:
    type: action
    risk_level: high
    confirmation: required
    audit_log: required
```

### Rules

* Actions that move to another page SHOULD use `type: navigation`.
* Actions that mutate data or trigger work SHOULD use `type: action`.
* Risky actions SHOULD declare `confirmation: required`.
* Audited actions SHOULD declare `audit_log: required`.

---

## 8. Naming Conventions

## 8.1 General Rules

All IDs MUST be stable and semantic.

Good:

```yaml
page: license-detail
route_ref: license_detail
id: download_license
```

Bad:

```yaml
page: page1
route_ref: routeA
id: button2
```

---

## 8.2 Recommended Case Styles

| Entity       | Recommended Style | Example                |
| ------------ | ----------------- | ---------------------- |
| Product ID   | kebab-case        | `license-forge`        |
| Page ID      | kebab-case        | `license-detail`       |
| Route ID     | snake_case        | `license_detail`       |
| Action ID    | snake_case        | `download_license`     |
| State ID     | snake_case        | `download_in_progress` |
| Section ID   | snake_case        | `license_summary`      |
| Component ID | snake_case        | `page_header`          |

---

## 8.3 Page IDs

Page IDs SHOULD describe the user-visible page.

Examples:

```yaml
page: dashboard
page: applications-list
page: application-detail
page: license-create
page: license-detail
page: audit-logs-list
```

Do not use:

```yaml
page: screen1
page: main
page: viewA
```

---

## 8.4 Route IDs

Route IDs SHOULD use snake_case and correspond to a route’s role.

Examples:

```yaml
id: licenses_list
id: license_detail
id: application_create
```

---

## 8.5 Action IDs

Action IDs SHOULD use verb-object naming.

Examples:

```yaml
id: create_license
id: download_license
id: renew_license
id: generate_key
id: update_status
```

Avoid vague actions:

```yaml
id: submit
id: click
id: handle_ok
```

---

## 8.6 Section IDs

Section IDs SHOULD describe the semantic section role.

Examples:

```yaml
id: header
id: filters
id: licenses_table
id: license_summary
id: payload_editor
```

Avoid:

```yaml
id: div1
id: boxA
id: topPart
```

---

## 9. Navigation Modeling

## 9.1 Navigation Definition

A navigation item represents a movement to a product module, page, or major view.

Navigation SHOULD be used for:

* entering a module
* entering a page
* entering a major view
* locating the user in product structure

Navigation SHOULD NOT be used for:

* saving
* deleting
* signing
* downloading
* generating a key
* opening a modal
* toggling a drawer

---

## 9.2 Primary Navigation

Primary navigation represents top-level product areas.

Example:

```yaml
sidebar:
  primary_navigation:
    - id: dashboard
      label: Dashboard
      route: /dashboard

    - id: applications
      label: Applications
      route: /applications

    - id: licenses
      label: Licenses
      route: /licenses

    - id: audit_logs
      label: Audit Logs
      route: /audit-logs
```

---

## 9.3 Secondary Navigation

Secondary navigation is represented using `children`.

Example:

```yaml
sidebar:
  primary_navigation:
    - id: applications
      label: Applications
      route: /applications
      children:
        - list
        - create
        - detail
        - keys
```

`children` expresses hierarchy only. It does not require that the UI render as:

* accordion
* tree
* submenu
* tab
* dropdown

The visual implementation is determined elsewhere.

---

## 9.4 Navigation vs Tabs vs Actions

| Type       | Use When                                  | Model As                      |
| ---------- | ----------------------------------------- | ----------------------------- |
| Navigation | Moves through product structure           | `sidebar.primary_navigation`  |
| Tab        | Switches content within same page context | `sections[].type: tabs`       |
| Action     | Triggers an operation                     | `actions`                     |
| Menu item  | Appears inside dropdown/context menu      | `menu_items` or `row_actions` |

---

## 9.5 Anti-pattern: Action as Navigation

Bad:

```yaml
sidebar:
  primary_navigation:
    - id: generate_key
      label: Generate Key
```

Good:

```yaml
actions:
  secondary:
    - id: generate_key
      label: Generate Key
      type: action
      confirmation: required
```

---

## 10. Route Modeling

## 10.1 Route Purpose

Routes define addressable state.

A state belongs in `route.query` if it should:

* survive page refresh
* be shareable by URL
* restore view state from URL
* represent search/filter/pagination/sort state
* represent addressable tab state

Example:

```yaml
routes:
  - id: licenses_list
    path: /licenses
    page: licenses-list
    query:
      q: string
      status:
        - issued
        - replaced
        - expired
        - revoked
      page: number
      page_size: number
```

---

## 10.2 Route Query Rules

Route query fields SHOULD be used for:

* search query
* filters
* pagination
* sorting
* addressable tab selection

Route query fields SHOULD NOT be used for:

* dialog open state
* drawer open state
* copied-to-clipboard state
* temporary loading flags
* form dirty state
* confirmation modal state

---

## 10.3 Dynamic Path Segments

Dynamic route segments SHOULD use colon syntax.

Example:

```yaml
path: /licenses/:license_id
```

Dynamic segment names SHOULD match domain identifiers.

Good:

```yaml
path: /applications/:app_id
path: /licenses/:license_id
```

Bad:

```yaml
path: /applications/:id
path: /licenses/:item
```

---

## 11. Page Modeling

## 11.1 Page Purpose

Each page MUST define:

```yaml
goal:
primary_task:
```

`goal` describes why the page exists.

`primary_task` describes what the user is primarily trying to accomplish.

Example:

```yaml
page: license-detail
goal: inspect one signed license, download it, renew it, or update its platform status
primary_task: verify license metadata and retrieve the original license file
```

---

## 11.2 Page Sections

A page MUST define `sections`.

Sections describe information architecture and UI responsibilities.

Example:

```yaml
sections:
  - id: header
    type: page_header
    purpose: show page title and page-level actions

  - id: license_summary
    type: summary_panel
    purpose: show license metadata

  - id: detail_tabs
    type: tabs
    purpose: switch between overview, license JSON, and audit data
```

Sections MUST NOT be raw HTML containers.

Bad:

```yaml
sections:
  - type: div
    className: flex flex-col gap-4
```

Good:

```yaml
sections:
  - id: license_summary
    type: summary_panel
    purpose: show license metadata
```

---

## 11.3 Page Layout

A page MAY define semantic layout hints.

Example:

```yaml
layout:
  content_width: wide
  columns:
    - form
    - preview
```

Allowed layout values SHOULD remain semantic.

Good:

```yaml
layout:
  content_width: medium
```

Bad:

```yaml
layout:
  className: mx-auto max-w-3xl px-6
```

---

## 12. Section Modeling

## 12.1 Required Section Fields

Each section SHOULD include:

| Field     | Type   |    Required | Description             |
| --------- | ------ | ----------: | ----------------------- |
| `id`      | string |         yes | Stable section ID.      |
| `type`    | string |         yes | Section type.           |
| `purpose` | string | recommended | Section responsibility. |

Example:

```yaml
- id: filters
  type: filter_bar
  purpose: filter licenses by application, status, customer, or expiration
```

---

## 12.2 Recommended Section Types

The following section types are recommended:

| Section Type         | Purpose                                                  |
| -------------------- | -------------------------------------------------------- |
| `page_header`        | Page title, description, breadcrumb, page-level actions. |
| `metric_grid`        | Summary metrics.                                         |
| `summary_panel`      | Compact metadata or status information.                  |
| `collection_toolbar` | Search, filter, and collection-level actions.            |
| `filter_bar`         | Route-backed filters.                                    |
| `data_table`         | Dense operational records.                               |
| `data_list`          | List of records with lighter structure.                  |
| `activity_list`      | Recent activity or audit events.                         |
| `tabs`               | Same-page content panel switching.                       |
| `form`               | Data creation or editing.                                |
| `json_editor`        | Editable JSON object.                                    |
| `json_preview`       | Read-only generated JSON preview.                        |
| `code_viewer`        | Read-only code, PEM, canonical payload, or JSON.         |
| `action_bar`         | Grouped form/page actions.                               |
| `empty_state`        | No data state.                                           |
| `detail_panel`       | Secondary detail display.                                |

Custom section types MAY be added if they are semantic and reusable.

---

## 12.3 Data Table Sections

Data tables SHOULD specify:

* purpose
* columns
* row navigation if applicable
* row actions if applicable

Example:

```yaml
- id: licenses_table
  type: data_table
  purpose: show license records
  columns:
    - license_id
    - app_id
    - key_id
    - customer_name
    - issued_at
    - expires_at
    - status
    - row_actions
  row_navigation:
    target: license-detail
    params:
      license_id: license_id
  row_actions:
    - view_detail
    - download_license
    - renew_license
```

---

## 12.4 Form Sections

Forms SHOULD specify:

* fields
* required fields
* field type
* helper text where needed
* actions

Example:

```yaml
- id: license_form
  type: form
  purpose: collect license outer fields and custom payload
  fields:
    - id: app_id
      type: application_select
      required: true
    - id: expires_at
      type: datetime
      required: true
    - id: payload
      type: json_editor
      required: true
      helper: Application-defined license payload. Must be a JSON object.
```

---

## 13. Action Modeling

## 13.1 Action Types

Actions SHOULD use one of the following types:

| Type         | Meaning                         |
| ------------ | ------------------------------- |
| `navigation` | Moves to another page or route. |
| `action`     | Performs an operation.          |

Example:

```yaml
actions:
  primary:
    - id: create_license
      label: Create License
      type: navigation
      target: license-create

  secondary:
    - id: download_license
      label: Download License
      type: action
      audit_log: required
```

---

## 13.2 Action Groups

Actions MAY be grouped as:

```yaml
actions:
  primary: []
  secondary: []
  destructive: []
  row_actions: []
```

Use:

* `primary` for the main page action
* `secondary` for supporting actions
* `destructive` for dangerous actions
* `row_actions` for table/list row-specific actions

---

## 13.3 Risky Actions

Risky actions SHOULD declare risk and confirmation behavior.

Example:

```yaml
- id: generate_key
  label: Generate Key
  type: action
  risk_level: high
  confirmation: required
  audit_log: required
```

Actions that SHOULD require confirmation:

* generate key
* issue license
* renew license
* revoke license
* update license status
* retire key
* mark license as replaced

---

## 14. State Modeling

## 14.1 Page States

Every page MUST define `states`.

Common page states:

```yaml
states:
  - loading
  - ready
  - empty
  - error
```

Form pages MAY include:

```yaml
states:
  - editing
  - validating
  - submitting
  - success
  - error
```

Signing pages MAY include:

```yaml
states:
  - editing
  - validating
  - signing
  - success
  - error
```

---

## 14.2 State Definitions

Global state definitions SHOULD appear in `global_states`.

Example:

```yaml
global_states:
  loading:
    purpose: data or action is in progress
    presentation: skeleton_or_spinner

  signing:
    purpose: private-key signing operation is running
    restrictions:
      - disable_primary_actions
      - prevent_duplicate_submission
```

---

## 15. Local UI State Modeling

## 15.1 Local State Definition

`local_state` describes UI state that should not be encoded in the URL.

Examples:

```yaml
local_state:
  - selected_tab
  - update_status_dialog_open
  - payload_editor_dirty
  - license_json_copied
```

---

## 15.2 When to Use Local State

Use `local_state` for:

* dialog open state
* drawer open state
* selected row in local panel
* copy success state
* form dirty state
* unsaved editor state
* temporary preview expansion state

Do not use `local_state` for:

* search query
* filters
* pagination
* sort
* addressable tab state
* selected resource ID that should be shareable

---

## 15.3 Route vs Local State

| State           | Should Use    |
| --------------- | ------------- |
| Search query    | `route.query` |
| Status filter   | `route.query` |
| Pagination      | `route.query` |
| Sort order      | `route.query` |
| Addressable tab | `route.query` |
| Dialog open     | `local_state` |
| Drawer open     | `local_state` |
| Copy success    | `local_state` |
| Form dirty      | `local_state` |

---

## 16. Component Role Modeling

`components` describes reusable semantic component responsibilities.

Example:

```yaml
components:
  page_header:
    purpose: provide page title, description, breadcrumb, and page-level actions
    recommended_slots:
      - title
      - description
      - metadata
      - actions

  json_editor:
    purpose: edit application-defined payload JSON
    validation:
      required_object: true
      disallow_invalid_json: true

  code_viewer:
    purpose: display PEM keys, canonical payload, or license JSON
    read_only: true
```

Rules:

* Component entries SHOULD be semantic.
* Component entries MUST NOT include React implementation.
* Component entries MUST NOT include Tailwind classes.
* Component entries SHOULD describe purpose, slots, behavior, and validation responsibilities.

---

## 17. Boundaries with UI_TOKENS.yaml and UI_VISUAL_SPEC.yaml

## 17.1 UI_PAGE.yaml Responsibilities

`UI_PAGE.yaml` owns:

* page structure
* navigation hierarchy
* route mapping
* route-backed state
* page sections
* page goals
* primary tasks
* page actions
* page states
* local UI states
* semantic component responsibilities

## 17.2 UI_TOKENS.yaml Responsibilities

`UI_TOKENS.yaml` should own:

* color tokens
* typography tokens
* spacing tokens
* radius tokens
* shadow tokens
* border tokens
* semantic theme tokens
* light/dark mode token values

## 17.3 UI_VISUAL_SPEC.yaml Responsibilities

`UI_VISUAL_SPEC.yaml` should own:

* visual layout rules
* component visual behavior
* shadcn/ui component mapping
* Tailwind usage rules
* responsive visual behavior
* hover/focus/disabled/selected visual rules
* density rules
* card/panel/list visual guidance

## 17.4 Boundary Rule

`UI_PAGE.yaml` MUST NOT include:

* Tailwind utility classes
* CSS variables
* raw color values
* raw spacing values
* shadcn/ui component import paths
* JSX structure

---

## 18. Validation Rules

A `UI_PAGE.yaml` file SHOULD be validated against the following rules.

## 18.1 Document-level Validation

* `meta` MUST exist.
* `product` MUST exist.
* `routes` MUST exist.
* `pages` MUST exist.
* `sidebar` SHOULD exist for sidebar-based products.

## 18.2 Route Validation

* Every route `id` MUST be unique.
* Every route `path` MUST be unique.
* Every route `page` MUST reference an existing `pages[].page`.
* Every page `route_ref` MUST reference an existing `routes[].id`.

## 18.3 Page Validation

* Every page `page` MUST be unique.
* Every page MUST include `goal`.
* Every page MUST include `primary_task`.
* Every page MUST include `route_ref`.
* Every page MUST include `sections`.
* Every page MUST include `states`.

## 18.4 Section Validation

* Section IDs MUST be unique within a page.
* Section IDs SHOULD be semantic.
* Section types SHOULD use known section types unless a new semantic type is justified.
* Sections MUST NOT include HTML tags.
* Sections MUST NOT include Tailwind classes.

## 18.5 Navigation Validation

* Primary navigation IDs MUST be unique.
* Navigation items MUST represent pages, modules, or product views.
* Actions MUST NOT appear as primary navigation items.
* Children MUST represent secondary navigation structure, not random UI actions.

## 18.6 Action Validation

* Action IDs SHOULD use verb-object naming.
* Risky actions SHOULD define `confirmation`.
* Audited actions SHOULD define `audit_log`.
* Navigation actions SHOULD define `target`.
* Mutation actions SHOULD use `type: action`.

## 18.7 State Validation

* Route-backed state MUST be placed under `route.query`.
* Local UI-only state MUST be placed under `local_state`.
* The same state SHOULD NOT appear in both `route.query` and `local_state`.

---

## 19. Authoring Checklist

Before finalizing a `UI_PAGE.yaml`, verify:

```text
[ ] Does the document include meta, product, routes, and pages?
[ ] Does every page have a stable page ID?
[ ] Does every page define goal and primary_task?
[ ] Does every page route_ref match a route id?
[ ] Does every route.page match an existing page?
[ ] Are navigation items only used for product/page structure?
[ ] Are actions separated from navigation?
[ ] Are tabs only used for same-page panel switching?
[ ] Are route query fields addressable/shareable states?
[ ] Are local_state entries truly local UI-only states?
[ ] Are section IDs semantic?
[ ] Are section types semantic?
[ ] Does the file avoid HTML tags?
[ ] Does the file avoid Tailwind classes?
[ ] Does the file avoid React hooks or event handler names?
[ ] Does the file avoid implementation-specific DOM structure?
[ ] Are risky actions marked with confirmation when appropriate?
[ ] Are audited actions marked with audit_log when appropriate?
```

---

## 20. Good Examples

## 20.1 List Page

```yaml
- page: licenses-list
  goal: browse, search, filter, and manage issued licenses
  primary_task: find licenses by application, customer, status, or expiration
  route_ref: licenses_list
  sections:
    - id: header
      type: page_header
      purpose: show page title and create-license action

    - id: filters
      type: filter_bar
      purpose: filter licenses by operational fields
      controls:
        - search_license
        - filter_application
        - filter_status
        - filter_expiry_range

    - id: licenses_table
      type: data_table
      purpose: show license records
      columns:
        - license_id
        - app_id
        - key_id
        - customer_name
        - issued_at
        - expires_at
        - status
        - row_actions
      row_navigation:
        target: license-detail
        params:
          license_id: license_id
      row_actions:
        - view_detail
        - download_license
        - renew_license

  states:
    - loading
    - ready
    - empty
    - error
```

---

## 20.2 Detail Page with Tabs

```yaml
- page: license-detail
  goal: inspect one signed license, download it, renew it, or update its platform status
  primary_task: verify license metadata and retrieve the original license file
  route_ref: license_detail
  sections:
    - id: header
      type: page_header
      purpose: show license identity and page actions

    - id: license_summary
      type: summary_panel
      purpose: show license metadata

    - id: detail_tabs
      type: tabs
      purpose: switch between license overview, license JSON, and audit data
      tabs:
        - id: overview
          label: Overview
          sections:
            - license_metadata
            - payload_summary
            - signature_summary
        - id: license_json
          label: License JSON
          sections:
            - full_license_json_viewer
        - id: audit
          label: Audit
          sections:
            - related_audit_logs

  local_state:
    - selected_tab
    - update_status_dialog_open
    - license_json_copied

  states:
    - loading
    - ready
    - error
```

---

## 20.3 Create Page with Preview

```yaml
- page: license-create
  goal: issue a new offline license for an application
  primary_task: define license metadata, custom payload, expiration, and sign the license
  route_ref: license_create
  layout:
    content_width: wide
    columns:
      - form
      - preview
  sections:
    - id: header
      type: page_header
      purpose: introduce the license creation workflow

    - id: license_form
      type: form
      purpose: collect license outer fields and custom payload
      fields:
        - id: app_id
          type: application_select
          required: true
        - id: expires_at
          type: datetime
          required: true
        - id: customer_id
          type: text
          required: false
        - id: customer_name
          type: text
          required: false
        - id: payload
          type: json_editor
          required: true
          helper: Application-defined license payload. Must be a JSON object.

    - id: license_preview
      type: json_preview
      purpose: preview the license object before signature is generated

    - id: form_actions
      type: action_bar
      actions:
        primary:
          - id: issue_license
            label: Issue License
            type: action
            confirmation: required
            audit_log: required
        secondary:
          - id: cancel
            label: Cancel
            type: navigation
            target: licenses-list

  local_state:
    - payload_editor_dirty
    - preview_expanded
    - signing_confirmation_open

  states:
    - editing
    - validating
    - signing
    - success
    - error
```

---

## 21. Anti-patterns

## 21.1 HTML in DSL

Bad:

```yaml
sections:
  - component: div
    className: flex flex-col gap-4
```

Good:

```yaml
sections:
  - id: license_form
    type: form
    purpose: collect license metadata and payload
```

---

## 21.2 Tailwind Classes in DSL

Bad:

```yaml
layout:
  className: mx-auto max-w-7xl px-6 py-8
```

Good:

```yaml
layout:
  content_width: wide
```

---

## 21.3 Actions in Navigation

Bad:

```yaml
sidebar:
  primary_navigation:
    - id: issue_license
      label: Issue License
```

Good:

```yaml
actions:
  primary:
    - id: issue_license
      label: Issue License
      type: action
      confirmation: required
```

---

## 21.4 Local State in Route Query

Bad:

```yaml
route:
  query:
    drawer_open: boolean
```

Good:

```yaml
local_state:
  - drawer_open
```

---

## 21.5 Non-semantic IDs

Bad:

```yaml
sections:
  - id: box1
  - id: area2
  - id: rightStuff
```

Good:

```yaml
sections:
  - id: header
  - id: filters
  - id: licenses_table
```

---

## 22. Complete Reference Example

```yaml
meta:
  name: UI_PAGE
  project: license-forge
  version: 1
  purpose: >
    Define semantic page structure, navigation hierarchy, routes, sections,
    actions, and page states for license-forge.

product:
  id: license-forge
  name: License Forge
  description: Offline license issuing and management platform.

app_shell:
  layout:
    type: sidebar_main
    sidebar:
      width: fixed
      behavior:
        desktop: persistent
        mobile: collapsible
    main:
      width: fill
  regions:
    - sidebar
    - top_bar
    - main_content

sidebar:
  primary_navigation:
    - id: dashboard
      label: Dashboard
      route: /dashboard

    - id: applications
      label: Applications
      route: /applications
      children:
        - list
        - create
        - detail
        - keys

    - id: licenses
      label: Licenses
      route: /licenses
      children:
        - list
        - create
        - detail
        - renew

    - id: audit_logs
      label: Audit Logs
      route: /audit-logs

routes:
  - id: dashboard
    path: /dashboard
    page: dashboard

  - id: applications_list
    path: /applications
    page: applications-list
    query:
      q: string
      status:
        - active
        - inactive
        - archived
      page: number
      page_size: number

  - id: application_detail
    path: /applications/:app_id
    page: application-detail
    query:
      tab:
        - overview
        - keys
        - licenses
        - audit

  - id: licenses_list
    path: /licenses
    page: licenses-list
    query:
      q: string
      app_id: string
      status:
        - issued
        - replaced
        - expired
        - revoked
      page: number
      page_size: number

  - id: license_detail
    path: /licenses/:license_id
    page: license-detail
    query:
      tab:
        - overview
        - license_json
        - audit

pages:
  - page: dashboard
    goal: provide an operational overview of applications, licenses, expiry risks, and recent activity
    primary_task: inspect platform health and navigate to high-priority records
    route_ref: dashboard
    sections:
      - id: header
        type: page_header
        purpose: show dashboard title and high-level context
      - id: summary_metrics
        type: metric_grid
        purpose: summarize major platform counts
      - id: recent_licenses
        type: data_list
        purpose: show recently issued licenses
      - id: recent_audit_logs
        type: activity_list
        purpose: show recent platform operations
    states:
      - loading
      - ready
      - empty
      - error

  - page: licenses-list
    goal: browse, search, filter, and manage issued licenses
    primary_task: find licenses by application, customer, status, or expiration
    route_ref: licenses_list
    sections:
      - id: header
        type: page_header
        purpose: show page title and create-license action
      - id: filters
        type: filter_bar
        purpose: filter licenses by operational fields
      - id: licenses_table
        type: data_table
        purpose: show license records
        columns:
          - license_id
          - app_id
          - customer_name
          - issued_at
          - expires_at
          - status
          - row_actions
    states:
      - loading
      - ready
      - empty
      - error

components:
  page_header:
    purpose: provide page title, description, breadcrumb, and page-level actions

  filter_bar:
    purpose: expose route-backed filters for list pages

  data_table:
    purpose: show dense operational records

  summary_panel:
    purpose: show important metadata in compact field groups

global_states:
  loading:
    purpose: data or action is in progress

  ready:
    purpose: page data is available and can be used

  empty:
    purpose: no records match the current page or filters

  error:
    purpose: request or business operation failed

global_actions:
  create_license:
    type: navigation
    target: license-create

  download_license:
    type: action
    audit_log: required

  issue_license:
    type: action
    risk_level: high
    confirmation: required
    audit_log: required
```

---

## 23. Final Rule Summary

A good `UI_PAGE.yaml` file should be:

```text
semantic
structured
stable
implementation-neutral
route-aware
state-aware
navigation-safe
action-aware
easy for humans and AI to extend
```

It MUST NOT become:

```text
HTML
React code
Tailwind code
CSS
a screenshot script
a component implementation file
```

The document should remain a clean semantic bridge between product requirements and UI implementation.
