# Spacing — Canonical Spacing Definition

> **Source:** ui-design Skill + AGENTS.md (container widths, grid patterns)

---

## Section Padding System

| Section Type | Padding | Usage |
|-------------|---------|-------|
| Standard | `py-20 px-6` | Most sections |
| Compact | `py-12 px-6` | Less prominent sections, newsletter |
| Hero | `py-24 px-6` or `min-h-[80vh] px-6` | Hero sections |
| CTA | `py-16 px-6` | Call-to-action sections |

**Rule:** Section padding is consistently `py-20 px-6`. Deviations only for the types listed above.

---

## Container Widths

| Type | Class | Usage |
|------|-------|-------|
| Standard | `max-w-7xl mx-auto` | Most content containers |
| Narrow | `max-w-4xl mx-auto` | Text-heavy, centered content |
| Wide | `max-w-none` | Full-width backgrounds |
| Card container | `max-w-5xl mx-auto` | BaseLayout default |

---

## Grid Patterns

| Layout | Class |
|--------|-------|
| 2 columns | `grid md:grid-cols-2 gap-8` |
| 3 columns | `grid md:grid-cols-2 lg:grid-cols-3 gap-8` |
| 4 columns | `grid grid-cols-2 md:grid-cols-4 gap-6` |
| Features | `grid md:grid-cols-2 lg:grid-cols-3 gap-8` |

Grids always stack to a single column on mobile: `grid-cols-1` is the base case.

---

## Responsive Padding

Container padding increases with viewport:

```
px-4 sm:px-6 md:px-12
```

---

## Card Spacing

| Element | Spacing |
|---------|---------|
| Card padding | `p-6` (standard), `p-8` (large cards) |
| Card title gap | `mb-2` |
| Section title to content | `mb-12` |
| Title to description | `mb-4` |

---

## Vertical Rhythm

- Sections: `py-20` (80px vertical) as base rhythm
- Within sections: `mb-12` between title block and content
- Between cards: `gap-8` (32px)
- Between text elements: `mb-4` (16px)
