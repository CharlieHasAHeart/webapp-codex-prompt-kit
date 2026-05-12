# shadcn/ui + Tailwind Implementation Standard

Version: v1

## 1. Purpose

This standard defines how **shadcn/ui**, **Tailwind CSS**, and **CSS variables** should work together in a Web App project.

It is intended to prevent common implementation problems:

- theme values scattered across components
- hardcoded colors, radius values, and shadows
- non-reusable component styling
- excessive card-based page structure
- unclear boundaries between Tailwind, CSS, and component responsibilities
- expensive visual refactoring when the product style changes

The core principle is:

```text
Theme in CSS variables.
Expression in Tailwind utilities.
Components in shadcn/ui.
```

---

## 2. Responsibility Layers

### 2.1 Global Theme Layer

The global theme layer defines the product's reusable visual language.

It owns:

- semantic colors
- light and dark mode values
- global radius
- border color
- input border color
- focus ring color

This layer should primarily live in global CSS, such as:

```text
src/styles/globals.css
```

or the framework's equivalent global stylesheet.

Use CSS variables for theme values.

### 2.2 Design System Extension Layer

The design system extension layer defines reusable system scales and extensions.

It owns:

- font families
- shadow levels
- breakpoints
- animations
- keyframes
- reusable background patterns
- Tailwind plugin configuration

This layer should primarily live in:

```text
tailwind.config.ts
```

### 2.3 Component and Layout Implementation Layer

The component and layout implementation layer defines how UI is actually rendered.

It owns:

- flex and grid layout
- spacing
- sizing
- typography utilities
- border utilities
- hover, focus, active, selected, and disabled states
- responsive behavior
- component variants
- layout components

This layer should primarily live in React components and layout components through Tailwind `className` usage.

---

## 3. Recommended Project Structure

```text
src/
  components/
    ui/
      button.tsx
      input.tsx
      card.tsx
      badge.tsx
      select.tsx
      dialog.tsx
    layout/
      app-shell.tsx
      sidebar.tsx
      page-header.tsx
      page-container.tsx
      section.tsx
  styles/
    globals.css
  lib/
    utils.ts
tailwind.config.ts
```

### Directory Responsibilities

| Path | Responsibility |
|---|---|
| `components/ui/*` | Reusable base UI components, usually shadcn/ui-based. |
| `components/layout/*` | Application shell, sidebar, page header, page container, and layout primitives. |
| `styles/globals.css` | Theme CSS variables and global base styles. |
| `tailwind.config.ts` | Tailwind design-system extensions and plugins. |
| `lib/utils.ts` | Utility helpers such as `cn()`. |

---

## 4. Theme Design Standard

### 4.1 Start from Semantic Roles

Do not start with "I need a blue button."

Start with semantic UI roles.

Recommended semantic roles include:

- `background`
- `foreground`
- `card`
- `card-foreground`
- `primary`
- `primary-foreground`
- `secondary`
- `secondary-foreground`
- `muted`
- `muted-foreground`
- `accent`
- `accent-foreground`
- `destructive`
- `destructive-foreground`
- `border`
- `input`
- `ring`
- `radius`

### 4.2 Define Theme Values in CSS Variables

Recommended example:

```css
:root {
  --background: 210 20% 98%;
  --foreground: 222 20% 14%;

  --card: 0 0% 100%;
  --card-foreground: 222 20% 14%;

  --primary: 200 85% 32%;
  --primary-foreground: 0 0% 100%;

  --secondary: 210 16% 95%;
  --secondary-foreground: 222 20% 14%;

  --muted: 210 16% 96%;
  --muted-foreground: 215 14% 42%;

  --accent: 210 16% 94%;
  --accent-foreground: 222 20% 14%;

  --destructive: 0 72% 52%;
  --destructive-foreground: 0 0% 100%;

  --border: 214 18% 86%;
  --input: 214 18% 86%;
  --ring: 200 85% 32%;

  --radius: 0.375rem;
}
```

### 4.3 Theme Rules

- Theme values should express semantic roles, not one-off business colors.
- Do not create variables such as `--blue-button` or `--gray-panel`.
- Theme variables should be reusable across the whole product.
- Component code should consume semantic utilities such as `bg-background`, `text-foreground`, `border-border`, and `bg-primary`.

---

## 5. Dark Mode Standard

Dark mode should override the same semantic variables.

Do not create a separate component system for dark mode.

Recommended example:

```css
.dark {
  --background: 222 20% 10%;
  --foreground: 210 20% 96%;

  --card: 222 18% 12%;
  --card-foreground: 210 20% 96%;

  --primary: 200 90% 60%;
  --primary-foreground: 222 20% 10%;

  --secondary: 222 16% 18%;
  --secondary-foreground: 210 20% 96%;

  --muted: 222 16% 16%;
  --muted-foreground: 214 12% 70%;

  --accent: 222 16% 18%;
  --accent-foreground: 210 20% 96%;

  --destructive: 0 72% 56%;
  --destructive-foreground: 0 0% 100%;

  --border: 222 14% 24%;
  --input: 222 14% 24%;
  --ring: 200 90% 60%;
}
```

Rules:

- Components must not hardcode light-mode colors.
- Light and dark mode should use the same token names.
- Component classes should remain semantic across modes.

---

## 6. Visual Tone Guidance

### 6.1 Tooling, Admin, Console, and Agent Workbench Products

Recommended style:

- small to medium radius
- clear borders
- restrained shadows
- lower pill-shaped visual density
- less floating-card feeling
- hierarchy through spacing, borders, and typography

Recommended defaults:

```text
radius: 0.25rem to 0.5rem
shadow: subtle or none for structural containers
hierarchy: border + spacing first
```

### 6.2 Brand, Marketing, or Presentation Products

These products may use:

- larger radius
- stronger shadows
- more saturated color
- more decorative surfaces

Do not apply this style to admin tools by default.

---

## 7. Tailwind Configuration Standard

### 7.1 What Belongs in `tailwind.config.ts`

Recommended:

- `fontFamily`
- `boxShadow`
- `backgroundImage`
- `backgroundSize`
- `screens`
- `animation`
- `keyframes`
- plugins
- small reusable scale extensions

Example:

```ts
import type { Config } from "tailwindcss"
import animate from "tailwindcss-animate"

export default {
  darkMode: ["class"],
  content: ["./index.html", "./src/**/*.{ts,tsx}"],
  theme: {
    extend: {
      fontFamily: {
        sans: ["IBM Plex Sans", "ui-sans-serif", "system-ui", "sans-serif"],
        mono: ["IBM Plex Mono", "ui-monospace", "monospace"],
      },
      boxShadow: {
        panel: "0 1px 2px rgba(15, 23, 42, 0.06)",
      },
      backgroundImage: {
        "mesh-grid":
          "linear-gradient(to right, rgba(15,23,42,0.06) 1px, transparent 1px), linear-gradient(to bottom, rgba(15,23,42,0.06) 1px, transparent 1px)",
      },
      backgroundSize: {
        grid: "32px 32px",
      },
    },
  },
  plugins: [animate],
} satisfies Config
```

### 7.2 What Should Not Live Primarily in `tailwind.config.ts`

Avoid using `tailwind.config.ts` as the main home for:

- brand color values
- dark-mode theme values
- the full theme token system
- one-off component visual differences

These should usually live in CSS variables or reusable component variants.

---

## 8. Tailwind Usage Standard

Tailwind should primarily express:

- layout
- spacing
- sizing
- typography
- borders
- interaction states
- responsive behavior

Recommended:

```tsx
<div className="flex items-center justify-between gap-4 border border-border bg-background px-4 py-3 text-foreground hover:bg-muted md:px-6" />
```

Rules:

- Use semantic color utilities whenever possible.
- Use responsive utilities for layout adaptation.
- Avoid hardcoded arbitrary colors.
- Avoid repeated long class strings; extract reusable components or variants.

---

## 9. Component Implementation Standard

### 9.1 Base Rules

Components should:

- reference semantic tokens
- use variants for semantic differences
- use sizes for dimension differences
- accept `className` extension when appropriate
- keep structure stable
- avoid page-specific forks

### 9.2 Structural vs Semantic Styling

Structural styling controls:

- height
- padding
- alignment
- radius
- font size
- transition
- disabled behavior

Semantic styling controls:

- default action
- secondary action
- outline action
- ghost action
- destructive action
- success/warning/status variants when needed

Example variant direction:

```tsx
"default": "bg-primary text-primary-foreground hover:bg-primary/90"
"outline": "border border-border bg-background hover:bg-muted"
"ghost": "hover:bg-muted"
"destructive": "bg-destructive text-destructive-foreground hover:bg-destructive/90"
```

---

## 10. Button Standard

Recommended sizes:

| Size | Suggested Shape |
|---|---|
| `sm` | `h-8 px-3 text-xs` |
| `md` | `h-9 px-4 text-sm` |
| `lg` | `h-10 px-5 text-sm` |

Recommended variants:

- `default`
- `secondary`
- `outline`
- `ghost`
- `destructive`

Rules:

- Do not hardcode one-off business colors in page buttons.
- Do not repeatedly copy new button class strings across pages.
- Do not recreate button styling inside business pages.
- Use `destructive` for dangerous actions.
- Use `default` for the page's primary action.

---

## 11. Input, Select, and Textarea Standard

Form controls should share:

- height
- border
- focus ring
- placeholder color
- disabled state

Recommended direction:

```tsx
className="h-9 border border-input bg-background px-3 text-sm text-foreground placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring disabled:cursor-not-allowed disabled:opacity-50"
```

Rules:

- Focus state must be visible.
- Disabled state must look non-interactive.
- Placeholder text should use muted foreground.
- Controls should remain visually consistent across the product.

---

## 12. Card and Panel Standard

A card is not the default container.

A card is an emphasized container.

Recommended card use:

- settings blocks
- information summaries
- visually bounded local regions
- sections that need background support

Avoid:

- wrapping every page section in a card
- turning every list row into a heavy card
- using large radius and large shadows for headers

Recommended default:

```tsx
className="rounded-md border bg-card text-card-foreground"
```

Optional emphasized version:

```tsx
className="rounded-md border bg-card text-card-foreground shadow-panel"
```

Rules:

- Use panels often.
- Use cards selectively.
- Prefer borders and spacing over heavy shadows.
- `shadow-panel` should be used sparingly.

---

## 13. Badge Standard

Badges can easily make tool interfaces feel overly soft or decorative.

Recommended default:

```tsx
className="inline-flex items-center rounded-sm border px-2 py-0.5 text-xs font-medium"
```

Avoid defaulting to:

```tsx
className="rounded-full px-3 py-1"
```

Rules:

- Tooling interfaces should prefer small radius or light-border badges.
- Use status variants for operational meaning.
- Avoid overly pill-shaped badges unless the brand direction requires it.

---

## 14. Layout Standard

Layout should provide structure, not decoration.

It owns:

- page width
- horizontal and vertical padding
- header height
- sidebar width
- content flex behavior
- section spacing
- responsive adaptation

Recommended app shell direction:

```tsx
<div className="min-h-screen bg-background text-foreground">
  <div className="flex min-h-screen">
    <aside className="w-64 shrink-0 border-r bg-muted/20" />
    <main className="flex-1 px-4 py-6 md:px-6 lg:px-8" />
  </div>
</div>
```

Rules:

- Sidebar should have a stable width.
- Main content should flex.
- Page padding should be consistent.
- Background and text should use semantic tokens.

---

## 15. Page Container Standard

Recommended default:

```tsx
<div className="mx-auto w-full max-w-7xl" />
```

Guidance:

- Forms and settings pages may use narrower widths such as `max-w-3xl` or `max-w-4xl`.
- Workbench, dashboard, and data-heavy pages may use wider widths such as `max-w-6xl` or `max-w-7xl`.
- Do not force every page into the same narrow content width.

---

## 16. Section Layering Standard

Prefer hierarchy through:

- `space-y-*`
- `gap-*`
- `border-b`
- `py-*`
- subtle background changes

Recommended:

```tsx
<section className="border-b py-4" />
```

or:

```tsx
<section className="space-y-4" />
```

Use this more carefully:

```tsx
<section className="rounded-xl border bg-card p-6 shadow-panel" />
```

---

## 17. List Page Standard

List pages should not default to heavy card-per-row layouts.

Prefer:

- row-based layout
- dividers
- hover highlight
- subtle local background

Example:

```tsx
<div className="divide-y rounded-md border bg-background">
  <div className="px-4 py-3 hover:bg-muted/40" />
  <div className="px-4 py-3 hover:bg-muted/40" />
</div>
```

Avoid:

- every item as a large rounded card
- strong shadow per row
- separate floating background per row

---

## 18. Interaction State Standard

### 18.1 Hover

Hover should be lightweight.

Recommended:

- subtle background change
- slight border change
- slight text color change

Avoid:

- large shadow jump
- layout shift
- high-saturation background jump

### 18.2 Focus

Interactive components must have visible `focus-visible` styling.

Recommended:

```tsx
focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2
```

Rules:

- Buttons, inputs, selects, textareas, links, menu items, and tabs must have visible focus.
- Do not remove focus outlines unless an equivalent focus ring is provided.

### 18.3 Disabled

Recommended:

```tsx
disabled:cursor-not-allowed disabled:opacity-50
```

Rules:

- Disabled state should not look clickable.
- Disabled state should not rely only on a very subtle color change.

### 18.4 Selected and Active

Selected state must be visually distinct from hover state.

Recommended:

- selected uses stronger background or border
- active uses subtle press feedback
- current navigation uses `bg-muted` or `bg-accent`

---

## 19. Responsive Standard

Responsive design should reorganize hierarchy and spacing, not only shrink content.

Check:

- whether sidebar collapses
- whether page padding reduces
- whether grids become single-column
- whether header actions wrap or move into overflow
- whether tables scroll horizontally or transform into stacked rows

Recommended spacing:

| Area | Mobile | Tablet | Desktop |
|---|---|---|---|
| Page container | `px-4` | `md:px-6` | `lg:px-8` |
| Content block | `p-4` | `md:p-5` | `lg:p-6` |

Rules:

- Prefer `md:` for common layout changes.
- Use `lg:` for large-screen optimization.
- Avoid many tiny breakpoint-specific differences.

---

## 20. CSS, Tailwind, and Style Terminology

CSS is styling.

Tailwind is a utility-first way to express styling.

shadcn/ui is a component source pattern built on top of this styling approach.

Styling includes:

- color
- typography
- size
- margin
- padding
- gap
- border
- radius
- shadow
- layout
- responsive behavior
- hover, focus, active, selected, and disabled states

---

## 21. Recommended Implementation Sequence

### Step 1: Establish Tokens

Define these first in global CSS:

- `background`
- `foreground`
- `card`
- `primary`
- `secondary`
- `muted`
- `border`
- `input`
- `ring`
- `radius`

### Step 2: Normalize Base Components

Normalize:

- Button
- Input
- Select
- Card
- Badge

These strongly shape the product's visual tone.

### Step 3: Normalize Layout Rules

Normalize:

- page padding
- content max width
- sidebar width
- section spacing
- list and form layering

### Step 4: Reduce Excessive Card Usage

Review whether the project overuses:

- cards
- large radius
- large shadows
- pill-shaped badges

Replace with:

- clearer borders
- consistent spacing
- fewer but more meaningful cards

### Step 5: Complete Interaction States

Complete:

- hover
- focus-visible
- active
- selected
- disabled

Ensure interaction behavior is consistent across components.

---

## 22. Common Mistakes

### Mistake 1: Putting the Whole Theme in `tailwind.config.ts`

Prefer CSS variables for theme values.

### Mistake 2: Hardcoding Colors in Components

Hardcoded colors weaken the theme system and make global style changes expensive.

### Mistake 3: Turning Every Container into a Card

This makes interfaces visually noisy, soft, and fragmented.

### Mistake 4: Writing Too Much Override CSS

Heavy CSS overrides reduce the benefits of Tailwind and shadcn/ui.

### Mistake 5: Inconsistent Focus and Disabled States

This creates fragmented interaction behavior and accessibility issues.

---

## 23. Decision Guide

When changing UI style, ask:

### Is this a theme problem?

If yes, change global CSS variables.

Examples:

- primary color
- border color
- dark mode values
- global radius

### Is this a design-system extension problem?

If yes, change `tailwind.config.ts`.

Examples:

- font family
- shadow level
- animation
- breakpoint
- reusable background texture

### Is this a component or layout problem?

If yes, change the component or layout class composition.

Examples:

- Button padding
- Input focus state
- Card padding
- Header layout
- Sidebar layout

---

## 24. Final Rule

The best shadcn/ui + Tailwind implementation is not one that puts every style decision in one place.

It is one with clear responsibility boundaries:

- CSS variables own theme values.
- Tailwind utilities express layout, spacing, typography, state, and responsiveness.
- shadcn/ui components provide reusable component structure and variants.
