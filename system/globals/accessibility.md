# Accessibility — Canonical Accessibility Definition

> **Source:** accessibility Skill + AGENTS.md
> **Standard:** WCAG 2.1 AA

---

## WCAG 4 Principles

1. **Perceivable** — Content accessible to all senses
2. **Operable** — Keyboard-navigable, enough time
3. **Understandable** — Readable, predictable, error-tolerant
4. **Robust** — Compatible with assistive technologies

---

## Color Contrast

| Element | Minimum Ratio |
|---------|--------------|
| Normal text (< 18pt) | 4.5:1 |
| Large text (>= 18pt or >= 14pt bold) | 3:1 |
| UI elements & graphics | 3:1 |

---

## Semantic HTML

```html
<nav>...</nav>        <!-- Navigation -->
<main>...</main>      <!-- Main content -->
<article>...</article> <!-- Standalone content -->
<aside>...</aside>    <!-- Supplementary content -->
<header>...</header>  <!-- Header area -->
<footer>...</footer>  <!-- Footer area -->
<section aria-labelledby="id">...</section> <!-- Named section -->
```

**Never use `<div>` as a replacement for semantic elements.**

---

## ARIA Patterns

```html
<!-- Buttons with icons (no visible text) -->
<button aria-label="Open menu">
  <MenuIcon aria-hidden="true" />
</button>

<!-- Expandable content -->
<button aria-expanded="false" aria-controls="panel-1">Toggle</button>
<div id="panel-1" role="region">...</div>

<!-- Live regions (dynamic updates) -->
<div aria-live="polite" aria-atomic="true">
  {statusMessage}
</div>

<!-- Decorative elements -->
<img src="decoration.svg" alt="" aria-hidden="true" />
```

---

## Keyboard Navigation

- All interactive elements reachable via Tab
- Enter/Space activates buttons
- Escape closes modals/dropdowns
- Arrow keys for lists/tabs/menus
- Focus must always be visible

---

## Focus Styles

```
focus:outline-none focus-visible:ring-2 focus-visible:ring-ring
```

- `focus-visible` instead of `focus` — shows ring only on keyboard navigation
- `ring-ring` uses the semantic focus token
- `focus:ring-offset-2` for buttons with background

---

## Touch Targets

Minimum: **44x44px** for all interactive elements on mobile.

Applies to: Buttons, links, nav items, toggle switches, checkboxes.

---

## Forms

```html
<label for="email">Email</label>
<input id="email" type="email" aria-required="true" aria-describedby="email-help" />
<p id="email-help">Help text</p>

<!-- Error message -->
<input aria-invalid="true" aria-describedby="email-error" />
<p id="email-error" role="alert">Error message</p>
```

---

## Skip Links

```html
<a href="#main-content"
   class="sr-only focus:not-sr-only focus:absolute focus:top-4 focus:left-4 focus:z-50 focus:px-4 focus:py-2 focus:bg-background focus:text-foreground focus:rounded-md">
  Skip to content
</a>
```

Already included in AstroDeck BaseLayout.

---

## AstroDeck-Specific Checks

- [ ] Dark mode toggle: `aria-label` + current state
- [ ] Mobile menu: `aria-expanded`, focus trap, Escape to close
- [ ] Logo link: `aria-label` (e.g. "AstroDeck — Go to homepage")
- [ ] Social links: `aria-label` with platform name
- [ ] Cookie banner: keyboard-navigable, focus trap
- [ ] Heading hierarchy: h1 → h2 → h3, no gaps
- [ ] All images: `alt` attribute present
- [ ] Interactive elements: never `<div>` instead of `<button>`

---

## Heading Hierarchy

- Every page has exactly one `<h1>`
- `<h2>` for section titles
- `<h3>` for sub-titles within sections
- Never skip levels
- Screen readers navigate primarily via headings
