# Self-Audit: SEO

## Context
You are auditing an Astro.js v6 project with Tailwind CSS v4 (@tailwindcss/vite), OKLCH color system, and shadcn/ui. Astro generates static HTML by default which is excellent for SEO. Pages live in `src/pages/`. Layouts in `src/layouts/`. The site should implement technical SEO best practices for maximum search engine visibility.

## Rules

### Meta Tags (Per Page)
- Every page MUST have a unique `<title>` tag (50-60 characters optimal)
- Every page MUST have a unique `<meta name="description">` (150-160 characters)
- Meta descriptions must be compelling and include target keywords
- No duplicate meta descriptions across pages
- `<meta name="viewport" content="width=device-width, initial-scale=1">` must be present

### Open Graph Tags (Per Page)
Required OG tags:
- `<meta property="og:title">`
- `<meta property="og:description">`
- `<meta property="og:image">` (minimum 1200x630px)
- `<meta property="og:url">`
- `<meta property="og:type">` (usually "website" or "article")
- `<meta property="og:site_name">`

### Twitter Card Tags (Per Page)
Required Twitter tags:
- `<meta name="twitter:card" content="summary_large_image">`
- `<meta name="twitter:title">`
- `<meta name="twitter:description">`
- `<meta name="twitter:image">`

### Canonical URLs
- Every page MUST have `<link rel="canonical" href="...">` with absolute URL
- Canonical URL must match the page's actual URL
- No trailing slash inconsistencies between canonical and actual URL
- Duplicate content pages must canonical to the primary version

### Structured Data (JSON-LD)
- Homepage: `Organization` or `WebSite` schema
- Blog posts: `Article` or `BlogPosting` schema
- Product/pricing pages: `Product` or `Offer` schema
- FAQ sections: `FAQPage` schema
- JSON-LD must be valid (test at https://validator.schema.org)

### Heading Hierarchy (SEO Perspective)
- Exactly **one `<h1>` per page** containing the primary keyword
- `<h2>` tags for major sections (secondary keywords)
- `<h3>` tags for subsections
- Never skip heading levels
- Headings should be descriptive, not generic ("Our Services" is weak; "Web Development Services" is better)

### Sitemap & RSS
- `sitemap-index.xml` must be generated (Astro's `@astrojs/sitemap`)
- Sitemap must be referenced in `robots.txt`
- RSS feed for blog content (if applicable)
- `robots.txt` must exist and be properly configured

### URL Structure
- Clean URLs without file extensions
- Lowercase, hyphen-separated
- No special characters or spaces
- Logical hierarchy reflecting site structure

### Internal Linking
- All pages should be reachable within 3 clicks from homepage
- No orphan pages (pages with no internal links pointing to them)
- Navigation should include key pages
- Footer should include secondary pages

## Audit Steps

1. **Check for meta descriptions on all pages:**
   ```
   grep -rn 'meta.*description\|name="description"' src/pages/ src/layouts/
   ```
   Verify each page has a unique description of 150-160 characters.

2. **Check for Open Graph tags:**
   ```
   grep -rn "og:title\|og:description\|og:image\|og:url\|og:type" src/layouts/ src/pages/ src/components/
   ```
   All 6 required OG properties must be present.

3. **Check for Twitter card tags:**
   ```
   grep -rn "twitter:card\|twitter:title\|twitter:description\|twitter:image" src/layouts/ src/pages/ src/components/
   ```

4. **Check for canonical URLs:**
   ```
   grep -rn 'rel="canonical"\|rel=.canonical.' src/layouts/ src/pages/
   ```
   Every page needs a canonical link with absolute URL.

5. **Check for structured data (JSON-LD):**
   ```
   grep -rn "application/ld+json\|schema.org" src/
   ```
   At minimum, the homepage should have Organization or WebSite schema.

6. **Check heading hierarchy — one h1 per page:**
   ```
   grep -rn "<h1" src/pages/
   grep -rn "<h1" src/sections/
   grep -rn "<h1" src/components/
   ```
   Cross-reference with layouts to ensure exactly one h1 renders per page.

7. **Check sitemap configuration:**
   ```
   grep -rn "sitemap" astro.config.* package.json
   ```
   Verify `@astrojs/sitemap` is installed and configured.

8. **Check robots.txt:**
   ```
   cat public/robots.txt
   ```
   Must reference sitemap URL.

9. **Check for title tags:**
   ```
   grep -rn "<title\|Astro.props.*title\|title:" src/layouts/ src/pages/
   ```
   Every page must have a unique title.

10. **Check for alt text on images (SEO value):**
    ```
    grep -rn '<img\|<Image' src/ | grep -v 'alt="[^"]\+'
    ```
    Images should have descriptive alt text with relevant keywords (not keyword-stuffed).

11. **Check for RSS feed (if blog exists):**
    ```
    grep -rn "rss\|feed" src/pages/ astro.config.*
    ls src/pages/rss* src/pages/feed* 2>/dev/null
    ```

12. **Check for noindex tags that shouldn't be there:**
    ```
    grep -rn "noindex" src/
    ```
    Make sure important pages are not accidentally noindexed.

## Evaluation
For each finding:
- **Severity:** Critical | Important | Nice-to-have
- **File:** Affected file(s)
- **Problem:** What exactly is wrong
- **Fix:** Concrete fix suggestion

### Severity Guide
- **Critical:** Missing meta descriptions, no canonical URLs, no sitemap, duplicate h1 tags, noindex on important pages, missing title tags
- **Important:** Missing OG tags, no structured data, missing Twitter cards, heading hierarchy issues, no robots.txt
- **Nice-to-have:** Meta description length optimization, enhanced structured data, RSS feed, keyword optimization in headings

## Output Format

### [Severity] [Short description]
**File:** `src/...`
**Problem:** ...
**Fix:** ...
