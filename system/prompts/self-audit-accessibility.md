# Self-Audit: Accessibility (WCAG 2.1 AA)

## Context
You are auditing an Astro.js v6 project with Tailwind CSS v4 (@tailwindcss/vite), OKLCH color system, and shadcn/ui + Radix UI for React components. The site must meet WCAG 2.1 AA compliance. Design tokens are defined in `src/styles/globals.css`. Interactive components use Radix UI primitives which provide built-in ARIA support — but only when used correctly.

## Rules

### 1. Perceivable

#### 1.1 Text Alternatives (WCAG 1.1.1)
- Every `<img>` MUST have an `alt` attribute
- Decorative images MUST use `alt=""` (empty string, not missing)
- `<Image>` components from `astro:assets` MUST have `alt` prop
- SVG icons used for meaning MUST have `aria-label` or `<title>`
- SVG icons that are decorative MUST have `aria-hidden="true"`
- Background images conveying information need text alternatives

#### 1.2 Color Contrast (WCAG 1.4.3 / 1.4.6)
- Normal text (<18px or <14px bold): minimum **4.5:1** contrast ratio
- Large text (>=18px or >=14px bold): minimum **3:1** contrast ratio
- UI components and graphical objects: minimum **3:1** contrast ratio
- Focus indicators: minimum **3:1** contrast ratio against adjacent colors
- Do not rely on color alone to convey information (WCAG 1.4.1)

#### 1.3 Heading Hierarchy (WCAG 1.3.1)
- Exactly **one `<h1>`** per page
- Headings MUST NOT skip levels (h1 -> h3 without h2 is a violation)
- Headings must be semantic (`<h1>`-`<h6>`), not styled divs
- Reading order must match visual order

### 2. Operable

#### 2.1 Keyboard Navigation (WCAG 2.1.1 / 2.1.2)
- All interactive elements MUST be reachable via Tab key
- No keyboard traps — user must be able to navigate away from any element
- Custom interactive elements need `tabindex="0"` and keyboard event handlers
- `tabindex` values greater than 0 are forbidden
- Modal dialogs MUST trap focus within themselves while open
- Dropdown menus must support Arrow, Enter, Escape keys

#### 2.2 Focus Indicators (WCAG 2.4.7)
- All focusable elements MUST have visible focus styles
- Required Tailwind classes: `focus-visible:ring-2 focus-visible:ring-ring focus-visible:outline-none`
- Focus ring must have at least 3:1 contrast against the background
- Never use `outline: none` or `outline-0` without a replacement focus indicator

#### 2.3 Touch Targets (WCAG 2.5.8)
- Minimum touch target size: **44x44px**
- Inline links within text are exempt
- Buttons, nav links, form controls, and icon buttons must meet this
- Use `min-h-11 min-w-11` (44px) for icon-only buttons

#### 2.4 Skip Links
- A "Skip to main content" link MUST be the first focusable element
- It should be visually hidden until focused: `sr-only focus:not-sr-only`
- Target must be `<main id="main-content">` or similar

### 3. Understandable

#### 3.1 Language (WCAG 3.1.1)
- `<html>` tag MUST have `lang` attribute (e.g. `lang="en"`)
- Content in other languages needs `lang` attribute on the container

#### 3.2 Form Labels (WCAG 1.3.1 / 3.3.2)
- Every form input MUST have an associated `<label>` (via `for`/`id` or wrapping)
- Placeholder text is NOT a substitute for labels
- Error messages must be associated with inputs via `aria-describedby`
- Required fields must be indicated (not by color alone)

### 4. Robust

#### 4.1 ARIA Usage
- Prefer semantic HTML over ARIA (`<button>` over `<div role="button">`)
- ARIA roles must be valid and complete (e.g. `role="tab"` needs `role="tablist"` parent)
- `aria-label` and `aria-labelledby` must not be empty
- Interactive `<div>` and `<span>` elements MUST have a role

## Audit Steps

1. **Check for images missing alt text:**
   ```
   grep -rn "<img" src/ | grep -v "alt="
   grep -rn "<Image" src/ | grep -v "alt="
   ```

2. **Check for skip link:**
   ```
   grep -rn "skip" src/layouts/ src/components/
   grep -rn 'id="main' src/layouts/
   ```

3. **Check for focus-visible styles on interactive elements:**
   ```
   grep -rn "focus-visible:" src/components/
   grep -rn "outline-none" src/ | grep -v "focus-visible"
   ```

4. **Check heading hierarchy — find all heading tags:**
   ```
   grep -rn "<h[1-6]" src/pages/
   grep -rn "<h[1-6]" src/sections/
   grep -rn "<h[1-6]" src/components/
   ```
   Verify: one h1 per page, no skipped levels.

5. **Check for lang attribute on html:**
   ```
   grep -rn '<html' src/layouts/
   ```

6. **Check for non-semantic interactive elements:**
   ```
   grep -rn 'onClick\|@click\|on:click' src/ | grep -v "<button\|<a \|<input\|<select"
   ```

7. **Check touch target sizes — icon buttons should have min-h-11:**
   ```
   grep -rn "icon\|Icon" src/components/ | grep -i "button"
   ```
   Verify these have min-h-11 min-w-11 or equivalent padding.

8. **Check for form labels:**
   ```
   grep -rn "<input\|<textarea\|<select" src/ | grep -v "label\|aria-label"
   ```

9. **Check SVG accessibility:**
   ```
   grep -rn "<svg" src/ | grep -v 'aria-hidden\|aria-label\|role="img"'
   ```

10. **Check for color-only information:**
    Review any use of red/green for error/success states — must also include text or icons.

## Evaluation
For each finding:
- **Severity:** Critical | Important | Nice-to-have
- **File:** Affected file(s)
- **Problem:** What exactly is wrong
- **Fix:** Concrete fix suggestion

### Severity Guide
- **Critical:** Missing alt text, no skip link, keyboard traps, no focus indicators, no lang attribute, missing form labels
- **Important:** Heading hierarchy issues, insufficient contrast, small touch targets, decorative SVGs without aria-hidden
- **Nice-to-have:** ARIA enhancements, additional landmark roles

## Output Format

### [Severity] [Short description]
**File:** `src/...`
**Problem:** ...
**Fix:** ...
