# Self-Audit: Dark Mode

## Context
You are auditing an Astro.js v6 project with Tailwind CSS v4 (@tailwindcss/vite), OKLCH color system, and shadcn/ui. Design tokens are defined in `src/styles/globals.css` using `@theme` directive. Dark mode is implemented via the `.dark` class on the `<html>` element (class-based strategy), NOT via Tailwind's `dark:` prefix variant. All color tokens must have both light and dark mode values defined in CSS.

## Rules

### Dark Mode Strategy
- Dark mode uses the `.dark` class on `<html>` element
- Token values are overridden inside `.dark { }` or `html.dark { }` selector in `src/styles/globals.css`
- Do NOT use Tailwind `dark:` prefix for color changes (e.g. `dark:bg-gray-900` is a violation)
- The `dark:` prefix is only acceptable for non-token utilities like `dark:shadow-lg` (rare)
- Theme toggle must persist choice in `localStorage`
- System preference (`prefers-color-scheme`) should be respected as default

### Token Parity
Every `--color-*` token defined in the light theme MUST have a corresponding value in the `.dark` block. Missing dark tokens cause broken styling in dark mode.

**Required token pairs (light and dark values must both exist):**

| Token | Purpose |
|-------|---------|
| `--color-background` | Page background |
| `--color-foreground` | Primary text color |
| `--color-card` | Card background |
| `--color-card-foreground` | Card text color |
| `--color-popover` | Popover/dropdown background |
| `--color-popover-foreground` | Popover text color |
| `--color-primary` | Primary brand color |
| `--color-primary-foreground` | Text on primary color |
| `--color-secondary` | Secondary accent color |
| `--color-secondary-foreground` | Text on secondary |
| `--color-muted` | Muted backgrounds |
| `--color-muted-foreground` | Muted/secondary text |
| `--color-accent` | Accent highlights |
| `--color-accent-foreground` | Text on accent |
| `--color-destructive` | Error/destructive actions |
| `--color-border` | Default border color |
| `--color-input` | Input field borders |
| `--color-ring` | Focus ring color |
| `--color-chart-1` through `--color-chart-5` | Chart colors (if used) |

Additional project-specific tokens (check your globals.css):
| Token | Purpose |
|-------|---------|
| `--color-sidebar-*` | Sidebar-specific tokens |
| `--color-section-muted` | Section background variant |
| `--color-section-inverted` | Inverted section background |
| `--color-section-inverted-foreground` | Text in inverted sections |

### Hardcoded Colors to Flag
These bypass the token system and break in dark mode:
- `bg-white` — use `bg-background` or `bg-card`
- `bg-black` — use `bg-foreground` or inverted section tokens
- `text-white` — use `text-background` or `text-primary-foreground`
- `text-black` — use `text-foreground`
- `bg-gray-*` — use `bg-muted` or `bg-secondary`
- `text-gray-*` — use `text-muted-foreground`
- `border-gray-*` — use `border-border`
- Any hardcoded `oklch(...)` or `hsl(...)` value in component files

### Contrast Requirements in Both Modes
- Light mode: text on background must have 4.5:1 contrast
- Dark mode: text on background must have 4.5:1 contrast
- Both modes must be independently checked
- Common dark mode mistake: `--color-muted-foreground` too close to `--color-muted`

### Images and Icons in Dark Mode
- SVG icons should use `currentColor` for fill/stroke
- Images with transparent backgrounds must be visible on both light and dark backgrounds
- Consider providing alternate images for dark mode if needed
- Logos with dark text on transparent background will disappear in dark mode

### Toggle Implementation
- Theme toggle button must be accessible (proper ARIA label)
- Choice must persist in `localStorage` under a consistent key
- Must prevent flash of wrong theme on page load (script in `<head>`)
- System preference detection: `window.matchMedia('(prefers-color-scheme: dark)')`

## Audit Steps

1. **Verify light mode tokens exist in globals.css:**
   ```
   grep -n "--color-" src/styles/globals.css
   ```
   List all `--color-*` tokens defined in the base/light theme.

2. **Verify dark mode tokens exist:**
   ```
   grep -A 100 "\.dark" src/styles/globals.css | grep "--color-"
   ```
   Every token from step 1 must appear here. Flag any missing ones.

3. **Check for dark: prefix usage (violation):**
   ```
   grep -rn "dark:" src/components/ src/sections/ src/pages/ src/layouts/
   ```
   `dark:` prefix for color utilities is a violation. Only token-based switching is allowed.

4. **Check for hardcoded bg-white / bg-black / text-white / text-black:**
   ```
   grep -rn "bg-white\|bg-black\|text-white\|text-black" src/
   ```
   Each must be reviewed — are they inside inverted sections (acceptable) or general use (violation)?

5. **Check for hardcoded gray utilities:**
   ```
   grep -rn "bg-gray-\|text-gray-\|border-gray-" src/
   ```

6. **Check SVG icons use currentColor:**
   ```
   grep -rn "fill=\|stroke=" src/components/ | grep -v "currentColor\|none\|transparent"
   ```
   Hardcoded fill/stroke colors will not adapt to dark mode.

7. **Check for theme toggle component:**
   ```
   grep -rn "theme\|dark-mode\|darkMode\|color-scheme" src/components/
   ```

8. **Check for flash-prevention script in head:**
   ```
   grep -rn "localStorage.*theme\|classList.*dark" src/layouts/
   ```
   There should be an inline script in `<head>` that sets `.dark` before render.

9. **Check for system preference detection:**
   ```
   grep -rn "prefers-color-scheme" src/
   ```

10. **Visual check (manual):**
    Toggle dark mode and check:
    - All text is readable on its background
    - No sections "disappear" (same bg as parent)
    - Images and logos are visible
    - Form inputs have visible borders
    - Cards are distinguishable from background

## Evaluation
For each finding:
- **Severity:** Critical | Important | Nice-to-have
- **File:** Affected file(s)
- **Problem:** What exactly is wrong
- **Fix:** Concrete fix suggestion

### Severity Guide
- **Critical:** Missing dark mode tokens (causes broken UI), hardcoded `bg-white`/`bg-black` on main containers, no flash-prevention script, text invisible in dark mode
- **Important:** `dark:` prefix color overrides, hardcoded gray utilities, SVGs with hardcoded colors, missing theme persistence
- **Nice-to-have:** Missing system preference detection, chart colors not defined for dark mode

## Output Format

### [Severity] [Short description]
**File:** `src/...`
**Problem:** ...
**Fix:** ...
