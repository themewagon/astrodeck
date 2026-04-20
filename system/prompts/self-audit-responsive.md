# Self-Audit: Responsive Design

## Context
You are auditing an Astro.js v6 project with Tailwind CSS v4 (@tailwindcss/vite), OKLCH color system, and shadcn/ui. Tailwind v4 uses a mobile-first approach — base styles apply to the smallest screens, and responsive prefixes (`sm:`, `md:`, `lg:`, `xl:`) add styles at wider breakpoints. The site must work seamlessly across mobile phones, tablets, and desktops.

## Rules

### Breakpoints (Tailwind v4 defaults)
| Prefix | Min-width | Typical devices |
|--------|-----------|-----------------|
| (base) | 0px | Small phones |
| `sm:` | 640px | Large phones, small tablets |
| `md:` | 768px | Tablets |
| `lg:` | 1024px | Small desktops, landscape tablets |
| `xl:` | 1280px | Desktops |
| `2xl:` | 1536px | Large desktops |

### Test Matrix
Test the site at these exact widths:
| Width | Device representation | Key checks |
|-------|----------------------|------------|
| **375px** | iPhone SE / small phone | Navigation collapses, text readable, no horizontal scroll, single column layouts |
| **768px** | iPad / tablet | Two-column grids may appear, navigation may still be mobile or switch to desktop |
| **1024px** | Small desktop / iPad landscape | Full desktop nav, multi-column grids, sidebar layouts |
| **1280px** | Standard desktop | Full layout, max-width containers, comfortable spacing |

### Layout Rules
- **No horizontal overflow at any viewport size.** The page must never have a horizontal scrollbar.
- **Grids must stack to 1 column on mobile.** A `grid-cols-3` must have a base of `grid-cols-1` (e.g. `grid-cols-1 md:grid-cols-2 lg:grid-cols-3`).
- **Flex layouts must wrap on mobile.** Use `flex-wrap` or switch to vertical `flex-col` on small screens.
- **Container must have horizontal padding.** `px-4` on mobile, `px-6` or `px-8` on larger screens. Content must never touch screen edges.
- **No fixed widths without responsive fallback.** `w-[600px]` alone is a violation. Must be `w-full md:w-[600px]` or `max-w-[600px] w-full`.
- **Images must be responsive.** Use `w-full` or `max-w-full` and never exceed container width.

### Typography
- Minimum font size on mobile: **14px** (`text-sm` = 14px in Tailwind)
- Body text should be at least `text-base` (16px) for readability
- Headings should scale down on mobile (e.g. `text-3xl md:text-4xl lg:text-5xl`)
- Line length should not exceed ~75 characters on desktop (use `max-w-prose` or `max-w-2xl`)

### Navigation
- Mobile navigation MUST exist (hamburger menu, slide-out, bottom nav, or similar)
- Desktop navigation links must be hidden on mobile and replaced with mobile nav
- Pattern: `hidden lg:flex` for desktop nav, `lg:hidden` for mobile menu trigger
- Mobile menu must be accessible (focusable, keyboard navigable, proper ARIA)

### Touch Targets
- All interactive elements must be at least **44x44px** on touch devices
- This includes buttons, links in navigation, form controls, close buttons
- Small inline text links are exempt
- Use `min-h-11 min-w-11` (44px) or sufficient padding

### Visibility Patterns
- If something uses `hidden lg:block`, there should be a mobile alternative
- If something uses `lg:hidden`, it should only show on mobile
- Verify no content is permanently hidden at any viewport (unless intentional)
- `sr-only` content is always acceptable (screen reader only)

### Section Padding
- Sections should have consistent vertical padding that scales with viewport
- Pattern: `py-12 md:py-16 lg:py-20` or similar progressive scaling
- Horizontal padding must prevent content from touching edges: `px-4 md:px-6 lg:px-8`

## Audit Steps

1. **Check for grids missing mobile column count:**
   ```
   grep -rn "grid-cols-[2-9]" src/ | grep -v "grid-cols-1"
   ```
   Any `grid-cols-2` or higher without a `grid-cols-1` base means it stays multi-column on mobile.

2. **Check for fixed widths without responsive fallback:**
   ```
   grep -rn 'w-\[.*px\]\|w-\[.*rem\]' src/ | grep -v "max-w-\|w-full"
   ```

3. **Check for horizontal overflow risks — fixed positioning and overflow:**
   ```
   grep -rn "overflow-x-hidden\|overflow-hidden" src/layouts/
   ```
   `overflow-x-hidden` on body is a band-aid; find the actual overflow cause.

4. **Check navigation has mobile version:**
   ```
   grep -rn "lg:hidden\|md:hidden\|hamburger\|mobile.*nav\|menu.*toggle" src/components/
   grep -rn "hidden lg:flex\|hidden md:flex" src/components/
   ```

5. **Check container padding:**
   ```
   grep -rn "container\|px-4\|px-6\|px-8" src/layouts/ src/sections/
   ```
   Verify padding increases at larger breakpoints.

6. **Check for responsive text scaling:**
   ```
   grep -rn "text-4xl\|text-5xl\|text-6xl" src/ | grep -v "md:\|lg:\|sm:"
   ```
   Large text sizes without responsive prefixes may be too large on mobile.

7. **Check for hidden content parity:**
   ```
   grep -rn "hidden lg:block\|hidden lg:flex\|hidden md:block\|hidden md:flex" src/
   ```
   For each match, verify there's a mobile alternative shown with `lg:hidden` or `md:hidden`.

8. **Check images are responsive:**
   ```
   grep -rn '<img\|<Image' src/ | grep -v "w-full\|max-w-\|object-"
   ```
   Images should have `w-full` or `max-w-full` to prevent overflow.

9. **Check for tiny text on mobile:**
   ```
   grep -rn "text-xs" src/
   ```
   `text-xs` (12px) should only be used for badges, captions, or metadata — never for body content.

10. **Check section padding consistency:**
    ```
    grep -rn "py-[0-9]\|py-\[" src/sections/
    ```
    Sections should use consistent padding patterns.

11. **Manual viewport testing:**
    Open the site and test at 375px, 768px, 1024px, and 1280px:
    - No horizontal scrollbar at any width
    - Text is readable without zooming
    - Navigation works at each breakpoint
    - Images don't overflow their containers
    - Forms are usable on mobile
    - Modals/popovers fit the screen

## Evaluation
For each finding:
- **Severity:** Critical | Important | Nice-to-have
- **File:** Affected file(s)
- **Problem:** What exactly is wrong
- **Fix:** Concrete fix suggestion

### Severity Guide
- **Critical:** Horizontal overflow, no mobile navigation, content cut off at mobile widths, fixed widths causing layout break, grids not stacking on mobile
- **Important:** Missing responsive text scaling, inconsistent section padding, touch targets too small, images overflowing containers
- **Nice-to-have:** Padding refinement, text-xs usage review, minor visibility pattern gaps

## Output Format

### [Severity] [Short description]
**File:** `src/...`
**Problem:** ...
**Fix:** ...
