# Effects — Canonical Effects Definition

> **Source:** ui-design Skill (card pattern) + `src/styles/globals.css` (tokens)

---

## Border Radius

| Element | Class | Rule |
|---------|-------|------|
| Cards | `rounded-xl` | Standard for all cards |
| Buttons | `rounded-xl` | Consistent with cards |
| Inputs | `rounded-md` | shadcn/ui default |
| Badges | `rounded-full` | Pill shape |
| Mark element | `0.2em` | Inline highlight |

**Rule:** `rounded-xl` is the standard. No `rounded-lg` or `rounded-md` for cards or buttons.

---

## Shadows

| Type | Class | Usage |
|------|-------|-------|
| Default card | `shadow-sm` | Cards at rest |
| Hover card | `shadow-lg` | Cards on hover |
| No shadow | — | Most elements |

Hover transition: `hover:shadow-lg transition-shadow`

---

## Borders

| Type | Class | Usage |
|------|-------|-------|
| Card border | `border border-border` | All cards |
| Input border | `border border-input` | Form inputs |
| Divider | `border-t border-border` | Horizontal lines |

**Rule:** Always use `border-border`, never `border-gray-200` or similar.

---

## Card Pattern (Reference)

```html
<div class="rounded-xl border border-border bg-card p-6 shadow-sm">
  <h3 class="text-xl font-semibold mb-2">{title}</h3>
  <p class="text-muted-foreground">{description}</p>
</div>
```

Components:
- `rounded-xl` — consistent radius
- `border border-border` — subtle border
- `bg-card` — theme-aware background
- `p-6` — standard padding
- `shadow-sm` — subtle shadow

---

## Hover Effects

```
hover:shadow-lg transition-shadow    → Card hover
hover:bg-primary/90                  → Button hover (10% transparent)
hover:bg-accent                      → Nav item hover
```

---

## Mark/Highlight

```css
mark {
  background-color: oklch(92% 0.05 150);  /* Light */
  color: oklch(25% 0.02 150);
  font-weight: 600;
  padding: 0.1em 0.2em;
  border-radius: 0.2em;
}

.dark mark {
  background-color: oklch(35% 0.05 150);  /* Dark */
  color: oklch(95% 0.02 150);
}
```

---

## Rules

1. **`rounded-xl` for cards** — no deviations
2. **`border border-border`** — always use semantic border color
3. **`bg-card`** — never `bg-white` or `bg-gray-50`
4. **Shadows sparingly** — `shadow-sm` default, `shadow-lg` only on hover
5. **No glassmorphism** — not part of the current design system
