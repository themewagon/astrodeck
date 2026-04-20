# Self-Audit: Performance

## Context
You are auditing an Astro.js v6 project with Tailwind CSS v4 (@tailwindcss/vite), OKLCH color system, and shadcn/ui + Radix UI for React components. Astro's island architecture means components are static HTML by default — only components with `client:*` directives ship JavaScript. The project uses TypeScript. Performance budget targets Lighthouse Performance score > 90.

## Rules

### Core Web Vitals Targets
| Metric | Target | Description |
|--------|--------|-------------|
| **FCP** (First Contentful Paint) | < 1.8s | Time until first text/image renders |
| **LCP** (Largest Contentful Paint) | < 2.5s | Time until largest visible element renders |
| **CLS** (Cumulative Layout Shift) | < 0.1 | Visual stability score |
| **TBT** (Total Blocking Time) | < 200ms | Time main thread is blocked |
| **Lighthouse Performance** | > 90 | Overall performance score |

### Astro Island Architecture
- Components are server-rendered HTML by default (zero JS)
- `client:load` — hydrates immediately on page load (heaviest, use sparingly)
- `client:idle` — hydrates when browser is idle (preferred for below-fold interactivity)
- `client:visible` — hydrates when component enters viewport (best for far-below-fold)
- `client:media` — hydrates when media query matches
- `client:only` — skips SSR, renders only on client (avoid unless necessary)
- RULE: Never use `client:load` for components below the fold
- RULE: Prefer `client:visible` or `client:idle` over `client:load` when possible
- RULE: Static components (no interactivity) should have NO client directive

### Image Optimization
- Use `<Image>` from `astro:assets` for automatic optimization
- Prefer WebP or AVIF format
- Hero/LCP images: < 200KB, use `loading="eager"` and `fetchpriority="high"`
- Below-fold images: use `loading="lazy"` (default in Astro Image)
- Always provide explicit `width` and `height` to prevent CLS
- Avoid oversized images — serve at display dimensions or max 2x for retina

### JavaScript & Bundle Size
- Minimize client-side JavaScript — Astro's strength is zero-JS by default
- Audit every `client:load` directive — is interactivity actually needed?
- Check for large dependencies pulled into client bundles
- External scripts must use `async` or `defer`
- Third-party scripts should have Subresource Integrity (SRI) hashes

### CSS Performance
- Tailwind v4 with @tailwindcss/vite handles CSS purging automatically
- No unused CSS frameworks or large CSS libraries
- Avoid `@import` chains in CSS (increases load time)
- Critical CSS is automatically inlined by Astro

### Font Loading
- Use `font-display: swap` to prevent FOIT (Flash of Invisible Text)
- Preload critical fonts: `<link rel="preload" as="font" crossorigin>`
- Limit font variants to what's actually used
- Self-host fonts when possible instead of Google Fonts CDN

## Audit Steps

1. **Run Lighthouse CLI (all 4 categories):**
   ```
   npx lighthouse http://localhost:4321 --output=json --output-path=./lighthouse-report.json --chrome-flags="--headless" --only-categories=performance,accessibility,best-practices,seo
   ```
   Or in browser: Chrome DevTools > Lighthouse > Performance

2. **Find all client:* directives:**
   ```
   grep -rn "client:load\|client:idle\|client:visible\|client:media\|client:only" src/
   ```
   For each `client:load`, ask: Can this be `client:idle` or `client:visible`?

3. **Check for images not using Astro Image component:**
   ```
   grep -rn '<img ' src/ | grep -v "astro"
   ```
   Native `<img>` tags miss out on automatic optimization.

4. **Check for images missing dimensions:**
   ```
   grep -rn '<img\|<Image' src/ | grep -v "width\|height"
   ```
   Missing dimensions cause CLS.

5. **Check LCP image optimization:**
   ```
   grep -rn 'fetchpriority\|loading="eager"' src/
   ```
   The hero/LCP image should have `fetchpriority="high"` and `loading="eager"`.

6. **Check for large image files:**
   ```
   find src/ public/ -name "*.png" -o -name "*.jpg" -o -name "*.jpeg" | xargs ls -la | sort -k5 -rn | head -20
   ```
   Flag any image over 200KB.

7. **Check for external scripts without SRI:**
   ```
   grep -rn '<script.*src=' src/ | grep -v "integrity="
   ```

8. **Check font loading strategy:**
   ```
   grep -rn "font-display" src/
   grep -rn "preload.*font" src/
   ```

9. **Check for unnecessary large imports:**
   ```
   grep -rn "import.*from" src/ | grep -i "lodash\|moment\|jquery\|animate.css"
   ```
   These are red flags for bundle size.

10. **Check build output size:**
    ```
    npm run build && du -sh dist/
    ls -la dist/_astro/*.js | sort -k5 -rn | head -10
    ls -la dist/_astro/*.css | sort -k5 -rn | head -10
    ```

## Evaluation
For each finding:
- **Severity:** Critical | Important | Nice-to-have
- **File:** Affected file(s)
- **Problem:** What exactly is wrong
- **Fix:** Concrete fix suggestion

### Severity Guide
- **Critical:** Lighthouse Performance < 80, LCP > 4s, CLS > 0.25, unnecessary `client:load` on heavy components, images > 500KB
- **Important:** Lighthouse Performance 80-90, missing eager/fetchpriority on LCP image, no font preloading, images 200-500KB, `client:load` replaceable with `client:idle`
- **Nice-to-have:** Lighthouse Performance 90-95, minor font optimization, SRI on external scripts

## Output Format

### [Severity] [Short description]
**File:** `src/...`
**Problem:** ...
**Fix:** ...
