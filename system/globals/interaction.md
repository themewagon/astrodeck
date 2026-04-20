# Interaction — Canonical Interaction Definition

> **Source:** `src/styles/globals.css` (animations) + tailwind Skill (utility patterns)

---

## Scroll Animations

AstroDeck uses a CSS-only scroll animation system via `data-animate` attributes.

### Base

```html
<div data-animate>Content fades in + slides up</div>
```

Base state: `opacity: 0; transform: translateY(20px)`
Visible: `opacity: 1; transform: translateY(0)`
Transition: `0.6s ease-out`

### Variants

| Attribute | Effect |
|-----------|--------|
| `data-animate` | Fade + slide up (default) |
| `data-animate="fade"` | Fade only (no movement) |
| `data-animate="slide-left"` | Slide from left (-30px) |
| `data-animate="slide-right"` | Slide from right (+30px) |
| `data-animate="scale"` | Scale up (0.95 → 1) |

### Stagger Delays

```html
<div data-animate data-delay="1">100ms</div>
<div data-animate data-delay="2">200ms</div>
<div data-animate data-delay="3">300ms</div>
<div data-animate data-delay="4">400ms</div>
<div data-animate data-delay="5">500ms</div>
```

### Reduced Motion

When `prefers-reduced-motion: reduce`, all animations are disabled:
- Opacity: immediately visible
- Transform: no movement
- Transition: no animation

---

## Hover/Focus States

```
hover:bg-primary/90          → Hover feedback on buttons
focus:ring-2 focus:ring-ring → Focus indicator
transition-colors duration-200 → Smooth color transitions
```

---

## Transitions

| Type | Class | Duration |
|------|-------|----------|
| Colors | `transition-colors duration-200` | 200ms |
| Shadows | `transition-shadow duration-200` | 200ms |
| All | `transition-all duration-200` | 200ms |

Default easing: `ease-out` (CSS default for Tailwind transitions)

---

## shadcn/ui Animations

For shadcn components (Accordion, Dialog, Dropdown):

| Animation | Duration | Usage |
|-----------|----------|-------|
| Accordion open/close | 200ms ease-out | `--animate-accordion-down/up` |
| Dialog enter | 150ms | `animate-in fade-in-0 zoom-in-95` |
| Dialog exit | 150ms | `animate-out fade-out-0 zoom-out-95` |
| Slide-in | 150ms | `slide-in-from-top-2` etc. |

---

## Rules

1. **No JavaScript animation libraries** — CSS-only via `data-animate`
2. **Always respect `prefers-reduced-motion`**
3. **Transition duration: 200ms** as default — not longer
4. **No `animate-` classes** outside of shadcn components
5. **Use scroll animations sparingly** — don't animate every element
