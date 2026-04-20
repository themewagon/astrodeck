# Self-Audit: Design Cascade Consistency

## Context
You are auditing an Astro.js v6 project with Tailwind CSS v4 (@tailwindcss/vite), OKLCH color system, and shadcn/ui. The design system follows a strict cascade: design tokens in `src/styles/globals.css` flow down through layouts, sections, and components. Each level has specific responsibilities. Violations of this cascade cause visual inconsistency and maintenance burden.

## Rules

### The Design Cascade Hierarchy

```
src/styles/globals.css (Level 0: Token Definitions)
    |
    v
src/layouts/*.astro (Level 1: Page Structure)
    |
    v
src/sections/*.astro (Level 2: Page Sections)
    |
    v
src/components/**/*.{astro,tsx} (Level 3: Reusable Components)
```

#### Level 0: globals.css — Token Definitions
- ALL design tokens (`--color-*`, `--radius`, `--font-*`, spacing scales) defined here
- Both light and dark mode values defined here
- No component-specific styles belong here
- Uses `@theme` directive for Tailwind v4 integration
- OKLCH color format exclusively

#### Level 1: Layouts — Page Structure
- Apply `bg-background text-foreground` on the root container
- Set global font family and base text size
- Define page-level structure (header, main, footer placement)
- No section-specific backgrounds or component styles
- Container width constraints belong here

#### Level 2: Sections — Page Sections
- Sections are the primary unit for background variation
- Use semantic section background classes (see below)
- Control vertical padding (`py-12 md:py-16 lg:py-20` or similar)
- Contain horizontal padding or container constraints
- NEVER override `--color-*` tokens with inline styles or arbitrary values

#### Level 3: Components — Reusable UI
- Components consume tokens, never redefine them
- Components MUST NOT set their own background color for page context (e.g. a Card should use `bg-card`, not `bg-white`)
- Components should be agnostic to which section they're in
- shadcn/ui components already use tokens — don't override their colors

### Section Background System
Sections should use a consistent class-based system for backgrounds:

| Class/Pattern | Purpose | Background token |
|--------------|---------|-----------------|
| (default) | Standard section | `bg-background` (inherits from layout) |
| `bg-muted` or section-muted class | Alternating/muted sections | `--color-muted` or `--color-section-muted` |
| Inverted section class | Dark/inverted sections (CTAs, footers) | `--color-section-inverted` |

Rules:
- Section backgrounds MUST use token-based classes, never hardcoded colors
- Alternating section backgrounds should use `bg-muted` or a `section-muted` utility
- Inverted sections (dark background, light text) should use dedicated tokens
- Never use `bg-gray-100` or `bg-slate-50` for section differentiation

### Padding Consistency
All sections should follow a consistent vertical padding scale:

| Element | Expected padding pattern |
|---------|------------------------|
| Main sections | `py-12 md:py-16 lg:py-20` or `py-16 md:py-20 lg:py-24` (pick one and be consistent) |
| Hero section | May have larger padding, but should be intentional |
| Inner content containers | Consistent `px-4 md:px-6 lg:px-8` or use `container` class |
| Space between section title and content | Consistent gap (`gap-8`, `gap-12`, etc.) |

Rule: If 5 sections each use different padding, that's a violation. Pick a pattern and stick to it.

### Card Pattern Consistency
Cards throughout the site must follow a consistent pattern:

| Property | Expected value |
|----------|---------------|
| Background | `bg-card` |
| Text color | `text-card-foreground` |
| Border | `border border-border` (if bordered) |
| Border radius | `rounded-xl` (consistent across all cards) |
| Padding | Consistent inner padding (e.g. `p-6` everywhere) |
| Shadow | Same shadow level across similar cards (`shadow-sm`, `shadow-md`, etc.) |

Rule: If feature cards use `rounded-lg` but pricing cards use `rounded-2xl`, that's a violation.

### Border Radius Consistency
The project should use a consistent border radius token:
- `--radius` token defined in globals.css
- Primary radius: `rounded-xl` for cards, panels, large containers
- Secondary radius: `rounded-lg` for buttons, inputs, smaller elements
- `rounded-full` for avatars, pills, circular elements only
- Never mix `rounded-md`, `rounded-lg`, `rounded-xl`, `rounded-2xl` arbitrarily

### CSS Variable Override Rules
- `--color-*` tokens MUST ONLY be overridden in globals.css (light/dark definitions)
- Sections MUST NOT contain `style="--color-primary: ..."` or similar overrides
- Components MUST NOT redefine token values
- The only exception: scoped theme variations explicitly designed as sub-themes

## Audit Steps

1. **Check globals.css has all tokens defined:**
   ```
   grep -n "@theme\|--color-\|--radius\|--font-" src/styles/globals.css
   ```
   Verify tokens are complete and use OKLCH.

2. **Check layouts set base styles correctly:**
   ```
   grep -rn "bg-background\|text-foreground" src/layouts/
   ```
   Layout root should have `bg-background text-foreground`.

3. **Check sections for hardcoded backgrounds:**
   ```
   grep -rn "bg-gray-\|bg-slate-\|bg-white\|bg-zinc-\|bg-neutral-" src/sections/
   ```
   Sections should use `bg-muted`, `bg-background`, or section-specific token classes.

4. **Check section padding consistency:**
   ```
   grep -rn "py-[0-9]\|py-\[" src/sections/
   ```
   All main sections should use the same vertical padding pattern.

5. **Check card patterns for consistency:**
   ```
   grep -rn "bg-card\|rounded-xl\|rounded-lg\|rounded-2xl\|rounded-md" src/components/ src/sections/
   ```
   Cards should consistently use `bg-card` and the same border radius.

6. **Check for CSS variable overrides outside globals.css:**
   ```
   grep -rn "style=.*--color-\|style=.*--radius" src/components/ src/sections/ src/layouts/
   ```
   Token overrides via inline styles are a violation.

7. **Check component backgrounds don't assume context:**
   ```
   grep -rn "bg-background\|bg-white" src/components/
   ```
   Components should use `bg-card` or `bg-popover`, not `bg-background` (that's for layouts).

8. **Check border-radius consistency across components:**
   ```
   grep -rn "rounded-" src/components/ | sort | uniq -c | sort -rn
   ```
   One or two radius values should dominate. If there are 5+ different values, the system is inconsistent.

9. **Check shadow consistency:**
   ```
   grep -rn "shadow-" src/components/ src/sections/ | sort | uniq -c | sort -rn
   ```
   Similar elements should use the same shadow level.

10. **Check for components that redefine token values:**
    ```
    grep -rn "var(--color-\|var(--radius" src/components/
    ```
    Components should use Tailwind utility classes that reference tokens, not raw `var()` calls with overrides.

11. **Check container usage consistency:**
    ```
    grep -rn "container\|max-w-\|mx-auto" src/sections/ src/layouts/
    ```
    All sections should use the same container strategy.

## Evaluation
For each finding:
- **Severity:** Critical | Important | Nice-to-have
- **File:** Affected file(s)
- **Problem:** What exactly is wrong
- **Fix:** Concrete fix suggestion

### Severity Guide
- **Critical:** CSS variable overrides in components/sections, components setting `bg-white`/`bg-black` directly, sections using hardcoded Tailwind color classes for backgrounds
- **Important:** Inconsistent card border radius, inconsistent section padding, inconsistent shadow usage, mixed border radius values, container strategy mismatch
- **Nice-to-have:** Minor padding variations, shadow refinement, container padding fine-tuning

## Output Format

### [Severity] [Short description]
**File:** `src/...`
**Problem:** ...
**Fix:** ...
