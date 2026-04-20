# Imagery — Canonical Image Definition

> **Source:** accessibility Skill (alt text) + ui-design Skill (performance) + qa Skill (optimization)

---

## Image Formats

| Format | Usage | Priority |
|--------|-------|----------|
| AVIF | Photographs, complex images | Preferred (best compression) |
| WebP | Fallback when AVIF not supported | Standard |
| SVG | Icons, logos, simple graphics | For vectors |
| PNG | Screenshots, graphics with transparency | Only when necessary |

---

## Astro Image Component

```astro
import { Image } from 'astro:assets';

<Image
  src={myImage}
  alt="Descriptive alt text"
  width={800}
  height={600}
  loading="lazy"
/>
```

- Astro optimizes images automatically (resize, format conversion)
- `loading="lazy"` for below-the-fold images
- Hero images: `loading="eager"` (above the fold)

---

## Alt Text Conventions

### Rules

1. **Every `<img>` needs an `alt` attribute** — no exceptions
2. **Decorative images:** `alt=""` + `aria-hidden="true"`
3. **Informative images:** Describe the content, not the format
4. **Functional images (links/buttons):** Describe the action

### Examples

```html
<!-- Informative -->
<img src="team.jpg" alt="The AstroDeck team working together in the office" />

<!-- Decorative -->
<img src="decoration.svg" alt="" aria-hidden="true" />

<!-- Functional (in link) -->
<a href="/"><img src="logo.svg" alt="AstroDeck — Go to homepage" /></a>

<!-- Icon button -->
<button aria-label="Open menu">
  <img src="menu.svg" alt="" aria-hidden="true" />
</button>
```

### Anti-Patterns

- `alt="Image"` — meaningless
- `alt="image.png"` — filename instead of description
- `alt="Photo of our team"` — "Photo of" is redundant
- Missing `alt` — accessibility error

---

## Performance Rules

| Rule | Threshold |
|------|-----------|
| Maximum image size | 200KB per image |
| Hero images | Max 400KB (higher tolerance) |
| OG image | 1200x630px, PNG or JPG |
| Favicon | `public/favicon.svg` |

---

## Responsive Images

```html
<!-- Responsive with Astro -->
<Image
  src={hero}
  widths={[400, 800, 1200]}
  sizes="(max-width: 768px) 100vw, (max-width: 1200px) 80vw, 1200px"
  alt="Hero image"
/>
```

- Mobile: load smaller image variant
- Desktop: load larger image variant
- Astro generates `srcset` automatically
