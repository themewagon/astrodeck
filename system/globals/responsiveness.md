# Responsiveness — Canonical Responsive Definition

> **Source:** ui-design Skill + qa Skill + AGENTS.md

---

## Breakpoints

| Breakpoint | Width | Tailwind Prefix | Primary Checks |
|------------|-------|-----------------|----------------|
| Mobile S | 375px | (base) | Single column, no overflow, touch targets >= 44px |
| Mobile L | 428px | (base) | Same as above, slightly more room |
| Tablet | 768px | `md:` | Grid 1→2 columns, sidebar still hidden |
| Laptop | 1024px | `lg:` | Full navigation, sidebar visible |
| Desktop | 1280px | `xl:` | Max-width containers centered |

**Test order:** Mobile S → Tablet → Laptop → Desktop

---

## Mobile-First Principle

```
text-base md:text-lg lg:text-xl     ← Correct (scale up)
text-xl lg:text-base                ← Wrong (scale down)
```

Mobile is the base case. Larger viewports extend.

---

## Responsive Grid Patterns

```
grid-cols-1 → md:grid-cols-2 → lg:grid-cols-3
```

Grids ALWAYS stack to a single column on mobile.

---

## Navigation

| Viewport | Behavior |
|----------|----------|
| Mobile (< `lg:`) | Hamburger menu or dropdown |
| Desktop (`lg:+`) | Full navigation + optional sidebar |

**Required:** Every `hidden lg:block` element MUST have a mobile equivalent (`lg:hidden`).

---

## Container Padding (responsive)

```
px-4 sm:px-6 md:px-12
```

Padding increases with viewport width.

---

## Common Anti-Patterns

| Anti-Pattern | Problem | Solution |
|-------------|---------|----------|
| `hidden` without mobile alternative | Content disappears on mobile | Add `lg:hidden` equivalent |
| `w-[500px]` without responsive fallback | Overflow on mobile | Use `w-full max-w-[500px]` |
| Sidebar only on desktop | No navigation on mobile | Collapsible "On this page" dropdown |
| `<pre>` without `overflow-x-auto` | Horizontal scroll | Add `overflow-x-auto` |
| Touch targets < 44px | Hard to tap on mobile | Min. 44x44px for all interactive elements |
| Text < 14px on mobile | Hard to read | Minimum `text-sm` (14px) |
| Desktop-first breakpoints | Overloaded mobile layout | Mobile-first: start small, scale up |

---

## Responsive Required Checks

- [ ] No horizontal overflow at 375px
- [ ] Navigation accessible on mobile
- [ ] Grids stack correctly (1 column on mobile)
- [ ] Container padding increases with viewport
- [ ] Touch targets at least 44x44px
- [ ] Text readable on mobile (min. 14px)
- [ ] `hidden lg:block` always has an `lg:hidden` counterpart
- [ ] Sidebar pages have mobile sub-navigation
