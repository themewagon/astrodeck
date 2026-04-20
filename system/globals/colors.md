# Colors — Canonical Color Definition

> **Source:** `src/styles/globals.css` (`@theme` + `.dark`)
> **Format:** OKLCH — `oklch(lightness% chroma hue)`
> **Configuration:** Only edit in `globals.css`, NEVER in `tailwind.config.mjs`

---

## OKLCH Color Format

```
oklch(lightness% chroma hue)
```

- **Lightness**: 0-100% (0 = black, 100 = white)
- **Chroma**: Color intensity (0 = gray, 0.1-0.4 = vivid)
- **Hue**: Color angle in degrees (0-360)

Common hues: Red ~25, Orange ~70, Yellow ~100, Green ~145, Blue ~250, Purple ~300

---

## Token Palette (Light Mode)

| Token | Value | Usage |
|-------|-------|-------|
| `--color-background` | `oklch(100% 0 0)` | Page background |
| `--color-foreground` | `oklch(9.8% 0.0016 286.75)` | Default text |
| `--color-card` | `oklch(100% 0 0)` | Card background |
| `--color-card-foreground` | `oklch(9.8% 0.0016 286.75)` | Card text |
| `--color-popover` | `oklch(100% 0 0)` | Popover background |
| `--color-popover-foreground` | `oklch(9.8% 0.0016 286.75)` | Popover text |
| `--color-primary` | `oklch(11.2% 0.0079 286.75)` | Primary actions, CTAs |
| `--color-primary-foreground` | `oklch(98% 0.0011 286.75)` | Text on primary |
| `--color-secondary` | `oklch(96.1% 0.0011 286.75)` | Secondary surfaces |
| `--color-secondary-foreground` | `oklch(11.2% 0.0079 286.75)` | Text on secondary |
| `--color-muted` | `oklch(96.1% 0.0011 286.75)` | Muted surfaces |
| `--color-muted-foreground` | `oklch(40% 0.0018 286.75)` | Subtle text |
| `--color-accent` | `oklch(96.1% 0.0011 286.75)` | Accent surfaces |
| `--color-accent-foreground` | `oklch(11.2% 0.0079 286.75)` | Text on accent |
| `--color-destructive` | `oklch(60.2% 0.177 29.23)` | Errors, delete actions |
| `--color-destructive-foreground` | `oklch(98% 0.0011 286.75)` | Text on destructive |
| `--color-border` | `oklch(91.4% 0.009 286.75)` | Borders, dividers |
| `--color-input` | `oklch(91.4% 0.009 286.75)` | Input borders |
| `--color-ring` | `oklch(9.8% 0.0016 286.75)` | Focus ring |

## Token Palette (Dark Mode)

| Token | Value |
|-------|-------|
| `--color-background` | `oklch(1.5% 0 0)` |
| `--color-foreground` | `oklch(98% 0 0)` |
| `--color-card` | `oklch(4% 0 0)` |
| `--color-card-foreground` | `oklch(98% 0 0)` |
| `--color-popover` | `oklch(4% 0 0)` |
| `--color-popover-foreground` | `oklch(98% 0 0)` |
| `--color-primary` | `oklch(98% 0 0)` |
| `--color-primary-foreground` | `oklch(1.5% 0 0)` |
| `--color-secondary` | `oklch(10% 0 0)` |
| `--color-secondary-foreground` | `oklch(98% 0 0)` |
| `--color-muted` | `oklch(10% 0 0)` |
| `--color-muted-foreground` | `oklch(71% 0 0)` |
| `--color-accent` | `oklch(10% 0 0)` |
| `--color-accent-foreground` | `oklch(98% 0 0)` |
| `--color-destructive` | `oklch(30.6% 0.132 29.23)` |
| `--color-destructive-foreground` | `oklch(98% 0 0)` |
| `--color-border` | `oklch(25% 0 0)` |
| `--color-input` | `oklch(25% 0 0)` |
| `--color-ring` | `oklch(80% 0 0)` |

---

## Section Variants

| Class | Effect |
|-------|--------|
| `section-muted` | Muted background (`--color-muted`) |
| `section-inverted` | Inverted colors (dark in light mode, light in dark mode) |

---

## Color Semantics

| Purpose | Utility Class | Example |
|---------|--------------|---------|
| Page background | `bg-background` | Body, layouts |
| Default text | `text-foreground` | Paragraphs, headlines |
| Subtle text | `text-muted-foreground` | Descriptions, meta info |
| Primary button | `bg-primary text-primary-foreground` | CTAs, main actions |
| Secondary button | `bg-secondary text-secondary-foreground` | Secondary actions |
| Error state | `text-destructive` | Validation, warnings |
| Card background | `bg-card` | Cards, panels |
| Borders | `border-border` | Dividers, card borders |
| Focus ring | `ring-ring` | Focus indicators |

---

## Rules

1. **Only use CSS variables** — `bg-primary`, not `bg-blue-500`
2. **OKLCH only** — no HSL, no hex, no RGB
3. **No `dark:` prefix** — colors switch automatically via `.dark` class
4. **`--color-` prefix** — all tokens start with `--color-` (not `--background`)
5. **No `tailwind.config.mjs`** — config lives in `src/styles/globals.css`
6. **No `@astrojs/tailwind`** — project uses `@tailwindcss/vite`

## Common Mistakes

| Mistake | Correct |
|---------|---------|
| `bg-blue-500` | `bg-primary` |
| `text-gray-600` | `text-muted-foreground` |
| `bg-white dark:bg-gray-900` | `bg-background` |
| `hsl(220 14% 96%)` | `oklch(96.27% 0.0044 286.32)` |
| `style="color: red"` | `text-destructive` |
