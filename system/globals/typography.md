# Typography — Canonical Typography Definition

> **Source:** `src/styles/globals.css` (font faces) + ui-design Skill (heading scale)

---

## Font Stack

| Type | Family | File | Weights |
|------|--------|------|---------|
| Sans | Geist | `/fonts/Geist-Variable.woff2` | 100-900 (Variable) |
| Mono | Geist Mono | `/fonts/GeistMono-Variable.woff2` | 100-900 (Variable) |

Both fonts use `font-display: swap` for optimal loading.

---

## Heading Scale

```
h1: text-5xl md:text-7xl font-bold      → Hero headlines
h2: text-3xl md:text-5xl font-bold      → Section headlines
h3: text-xl md:text-2xl font-semibold   → Sub-headlines, card titles
h4: text-lg font-semibold               → Feature titles
p:  text-base/text-lg text-muted-foreground → Body text
```

### Font Weights

| Element | Weight | Utility |
|---------|--------|---------|
| h1, h2 | Bold | `font-bold` |
| h3, h4 | Semibold | `font-semibold` |
| Body | Normal | `font-normal` (default) |

**Forbidden:** `font-extrabold`, `font-black` — never used in this design system.

---

## Body Text

- Default body: `text-base` (16px) or `text-lg` (18px)
- Color: `text-muted-foreground` for descriptions
- Color: `text-foreground` for primary body text

---

## Responsive Typography

Always scale mobile-first — start small, go bigger:

```
text-3xl md:text-5xl     ← Correct (mobile-first)
text-5xl lg:text-3xl     ← Wrong (desktop-first)
```

Minimum text size on mobile: `text-sm` (14px) for body text.

---

## Heading Hierarchy

- Every page has exactly one `<h1>`
- `<h2>` for main sections (section titles)
- `<h3>` for subsections (card titles)
- Never skip levels (h1 → h3 without h2)
- Headings structure the page for screen readers and SEO
