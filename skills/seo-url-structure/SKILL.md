---
title: SEO URL Structure & Internal Linking
description: Rules for clean URL architecture, breadcrumbs, pagination, and optimal internal linking patterns.
version: 2.0.0
---

# SEO: URL Structure & Internal Linking

## When to use
Use this skill when:
- Designing URL hierarchies for a new site or feature.
- Implementing breadcrumb navigation.
- Adding paginated content (blog listings, search results, product catalogs).
- Optimizing internal linking to distribute page authority.
- Migrating URLs or implementing redirects.

## Inputs required
- The site's content taxonomy (categories, tags, types).
- The primary navigation structure.
- Whether content is paginated and the pagination strategy.
- Any existing URL patterns that must be preserved (for migrations).

## Procedure

### 1. Clean URL Structure
- Use lowercase, hyphen-separated, human-readable URLs.
- Keep URLs short and descriptive — avoid IDs, hashes, or unnecessary parameters.
- Reflect content hierarchy in the URL path.

```
✅ CORRECT
/blog/technical-seo-guide
/products/running-shoes
/docs/getting-started/installation

❌ WRONG
/blog?id=123&ref=abc
/page.php?p=456
/products/ITEM_SKU_12345
/Blog/Technical-SEO-Guide  (mixed case)
```

### 2. Breadcrumb Navigation
- Every page deeper than the homepage SHOULD have breadcrumbs.
- Use a `<nav aria-label="Breadcrumb">` wrapper with an ordered list.
- Add `BreadcrumbList` JSON-LD schema.

```html
<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/blog">Blog</a></li>
    <li aria-current="page">Technical SEO Guide</li>
  </ol>
</nav>
```

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    { "@type": "ListItem", "position": 1, "name": "Home", "item": "https://example.com/" },
    { "@type": "ListItem", "position": 2, "name": "Blog", "item": "https://example.com/blog" },
    { "@type": "ListItem", "position": 3, "name": "Technical SEO Guide" }
  ]
}
</script>
```

### 3. Pagination
- Use `rel="next"` and `rel="prev"` links (still useful for some crawlers).
- Consider a "View All" page if content is small enough.
- Never block paginated pages from indexing — let crawlers discover all content.

```html
<!-- On page 2 of results -->
<link rel="prev" href="https://example.com/blog?page=1" />
<link rel="next" href="https://example.com/blog?page=3" />
<link rel="canonical" href="https://example.com/blog?page=2" />
```

### 4. Internal Linking Strategy
- Every important page should be reachable within 3 clicks from the homepage.
- Use descriptive anchor text — avoid "click here" or "read more."
- Link contextually from within content, not just from navigation menus.

```html
<!-- ✅ CORRECT — Descriptive anchor text -->
<p>Learn how to optimize your
  <a href="/blog/core-web-vitals">Core Web Vitals performance</a>
  for better search rankings.</p>

<!-- ❌ WRONG — Non-descriptive anchor text -->
<p>To learn about Core Web Vitals, <a href="/blog/core-web-vitals">click here</a>.</p>
```

### 5. Trailing Slashes
- Choose one pattern (with or without trailing slash) and be consistent.
- Redirect the non-canonical version with a 301 redirect.

```
# Pick ONE and stick to it:
https://example.com/blog/     ← trailing slash
https://example.com/blog      ← no trailing slash

# Redirect the other with 301
```

### 6. Redirects
- Use **301** (permanent) for URL migrations.
- Use **302** (temporary) only when the move is genuinely temporary.
- Never chain more than one redirect (A → B → C is bad; A → C is good).

### 7. Hub-and-Spoke Internal Linking Architecture
Structure internal links as topic clusters for topical authority:

- **Hub page** links to all spoke (cluster) pages with descriptive anchor text.
- **Every spoke** links back to the hub with a contextual anchor.
- **Spokes interlink** to each other where the topics are related.
- Use descriptive anchor text (not "click here") to pass topical context.

```html
<!-- Hub page linking to spokes -->
<p>Learn more about
  <a href="/seo/core-web-vitals">optimizing Core Web Vitals</a>,
  <a href="/seo/schema-markup">implementing Schema markup</a>, and
  <a href="/seo/javascript-seo">JavaScript SEO for SPAs</a>.
</p>

<!-- Spoke page linking back to hub -->
<p>This is part of our comprehensive
  <a href="/seo/technical-guide">Technical SEO Guide</a>.
</p>
```

### 8. Orphan Page Detection
Pages with **zero** incoming internal links are "orphan pages" and are unlikely to be indexed:

- Audit your site regularly with a crawl tool to detect orphan pages.
- Every content page must have at least one internal link pointing to it.
- New content should be immediately linked from relevant existing pages.

## Verification
- **URL audit:** Ensure all URLs are lowercase, hyphenated, and human-readable.
- **Breadcrumb check:** Verify breadcrumbs appear on all sub-pages and schema validates.
- **Internal link depth:** Use a crawl tool (Screaming Frog, Sitebulb) to verify all key pages are ≤ 3 clicks deep.
- **Redirect chain check:** Verify no redirect chains exist using `curl -I` or a crawl tool.
- **Orphan page check:** Verify no content pages have zero incoming internal links.

## Failure modes / debugging
| Problem | Cause | Fix |
|---|---|---|
| Duplicate content from trailing slashes | Both `/blog` and `/blog/` return 200 | 301 redirect one version to the other |
| Orphan pages not indexed | No internal links pointing to the page | Add contextual links from related content; integrate into topic cluster |
| "Click here" anchor text | Non-descriptive link text | Rewrite anchors to describe the linked content |
| Redirect chains (3xx → 3xx) | Old migrations stacked on top of each other | Update all redirects to point directly to the final URL |
| Mixed case URLs causing duplicates | Inconsistent casing in links | Lowercase all URLs; 301 redirect uppercase variants |
| Topic cluster not ranking | Spokes don't link back to hub | Ensure bidirectional hub ↔ spoke links with descriptive anchors |

## Escalation
- If a URL migration involves hundreds of redirects, consult an SEO engineer for redirect mapping and monitoring.
- If the CMS generates non-clean URLs by default, escalate to the platform admin for permalink configuration.
