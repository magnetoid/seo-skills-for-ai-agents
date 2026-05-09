---
title: Advanced SEO Schema & Snippets
description: Guidelines for implementing JSON-LD structured data, optimizing for Featured Snippets, and comprehensive schema coverage.
version: 2.0.0
---

# Advanced SEO: Schema & Snippets

## When to use
Use this skill when:
- Creating content-heavy pages (blogs, articles, FAQs, product pages, local business pages).
- Writing informational text blocks that could qualify for Featured Snippets.
- Adding structured data to improve rich result eligibility in SERPs.
- Implementing e-commerce product markup or review aggregation.

## Inputs required
- The content type of the page (article, FAQ, product, local business, recipe, event, etc.).
- Key data points: author, dates, prices, ratings, addresses, etc.
- Whether the content aims for paragraph, list, or table Featured Snippets.

## Procedure

### 1. JSON-LD Schema Implementation
- Inject a `<script type="application/ld+json">` block in the `<head>` or end of `<body>`.
- Always set `@context` to `"https://schema.org"`.
- Match `@type` to the page content.

```html
<!-- Article page -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Complete Guide to Technical SEO",
  "author": {
    "@type": "Person",
    "name": "Jane Smith"
  },
  "datePublished": "2025-01-15",
  "dateModified": "2025-02-01",
  "image": "https://example.com/images/seo-guide.webp",
  "publisher": {
    "@type": "Organization",
    "name": "Example Corp",
    "logo": {
      "@type": "ImageObject",
      "url": "https://example.com/logo.png"
    }
  }
}
</script>
```

```html
<!-- FAQ page -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is technical SEO?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Technical SEO refers to optimizations that help search engines crawl and index your site more effectively."
      }
    }
  ]
}
</script>
```

### 2. The Inverted Pyramid
- The first 40-50 words of an informational block MUST directly answer the user's query.
- Provide the concise answer first, then expand on details.

```html
<!-- ✅ CORRECT — Direct answer first -->
<p>Technical SEO is the practice of optimizing a website's infrastructure
so search engines can crawl, index, and render it effectively. It covers
site speed, mobile-friendliness, structured data, and crawl efficiency.
Unlike content SEO, it focuses on the code and architecture rather than
the words on the page.</p>

<!-- ❌ WRONG — Buries the answer -->
<p>In today's digital landscape, businesses need to understand many
aspects of online marketing. One important area that often gets
overlooked is something called technical SEO, which we'll explain
below after some background context...</p>
```

### 3. Data Extraction Formats
- Use `<ul>` / `<ol>` for list-based snippets.
- Use `<table>` for comparison/tabular data.
- Never render structured data as comma-separated text in a `<p>` tag.

```html
<!-- ✅ CORRECT — Structured list -->
<h2>Benefits of Technical SEO</h2>
<ul>
  <li>Faster page load times</li>
  <li>Improved crawl efficiency</li>
  <li>Better mobile experience</li>
  <li>Enhanced rich results in SERPs</li>
</ul>

<!-- ✅ CORRECT — Comparison table -->
<table>
  <thead>
    <tr><th>Feature</th><th>Technical SEO</th><th>Content SEO</th></tr>
  </thead>
  <tbody>
    <tr><td>Focus</td><td>Infrastructure</td><td>Content quality</td></tr>
    <tr><td>Tools</td><td>Lighthouse, GSC</td><td>Ahrefs, SEMrush</td></tr>
  </tbody>
</table>
```

### 4. WebSite Schema (Sitelinks Search Box & Site Name)
- Add `WebSite` schema to your homepage to enable a sitelinks search box in Google results.
- This also controls the site name displayed in SERPs.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "name": "Example Corp",
  "url": "https://example.com",
  "potentialAction": {
    "@type": "SearchAction",
    "target": {
      "@type": "EntryPoint",
      "urlTemplate": "https://example.com/search?q={search_term_string}"
    },
    "query-input": "required name=search_term_string"
  }
}
</script>
```

### 5. VideoObject Schema
- Add `VideoObject` structured data when embedding videos to enable video rich results.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "VideoObject",
  "name": "How to Optimize Core Web Vitals",
  "description": "Step-by-step guide to improving LCP, CLS, and INP scores.",
  "thumbnailUrl": "https://example.com/thumbnails/cwv-guide.jpg",
  "uploadDate": "2025-01-15",
  "duration": "PT8M30S",
  "contentUrl": "https://example.com/videos/cwv-guide.mp4",
  "embedUrl": "https://www.youtube.com/embed/abc123"
}
</script>
```

### 6. Sitelinks Best Practices
- Google automatically generates sitelinks for sites with clear structure.
- To improve sitelinks quality:
  - Use descriptive, unique page titles.
  - Create a logical site hierarchy.
  - Ensure important pages are linked from navigation.
  - Use `BreadcrumbList` schema to define page hierarchy.

### 7. Organization Schema
Add `Organization` schema on your homepage for Knowledge Panel eligibility:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Example Corp",
  "url": "https://example.com",
  "logo": "https://example.com/logo.png",
  "description": "We help developers build SEO-optimized web applications.",
  "contactPoint": {
    "@type": "ContactPoint",
    "telephone": "+1-555-123-4567",
    "contactType": "customer service"
  },
  "sameAs": [
    "https://twitter.com/examplecorp",
    "https://www.linkedin.com/company/examplecorp",
    "https://www.facebook.com/examplecorp"
  ]
}
</script>
```

### 8. Event Schema
Use `Event` structured data for event pages:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Event",
  "name": "Tech SEO Conference 2025",
  "startDate": "2025-06-15T09:00:00-07:00",
  "endDate": "2025-06-15T17:00:00-07:00",
  "eventAttendanceMode": "https://schema.org/OfflineEventAttendanceMode",
  "eventStatus": "https://schema.org/EventScheduled",
  "location": {
    "@type": "Place",
    "name": "Convention Center",
    "address": {
      "@type": "PostalAddress",
      "streetAddress": "123 Main St",
      "addressLocality": "San Francisco",
      "addressRegion": "CA",
      "postalCode": "94105",
      "addressCountry": "US"
    }
  },
  "image": "https://example.com/events/tech-seo-conf.jpg",
  "description": "Annual conference covering the latest in technical SEO.",
  "offers": {
    "@type": "Offer",
    "url": "https://example.com/events/tech-seo-conf/tickets",
    "price": "199.00",
    "priceCurrency": "USD",
    "availability": "https://schema.org/InStock",
    "validFrom": "2025-01-01"
  },
  "organizer": {
    "@type": "Organization",
    "name": "Example Corp",
    "url": "https://example.com"
  }
}
</script>
```

### 9. Speakable Schema (Voice Search)
Mark content as suitable for text-to-speech with `speakable`:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "What is Technical SEO?",
  "speakable": {
    "@type": "SpeakableSpecification",
    "cssSelector": [".article-summary", ".key-points"]
  },
  "url": "https://example.com/blog/what-is-technical-seo"
}
</script>
```

### 10. Entity SEO via `@id` and `sameAs`
Build entity confidence for Knowledge Panel eligibility and AI attribution:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "@id": "https://example.com/#organization",
  "name": "Example Corp",
  "url": "https://example.com",
  "logo": "https://example.com/logo.png",
  "sameAs": [
    "https://www.wikidata.org/wiki/Q12345678",
    "https://www.linkedin.com/company/examplecorp",
    "https://twitter.com/examplecorp",
    "https://www.crunchbase.com/organization/examplecorp",
    "https://www.facebook.com/examplecorp"
  ]
}
</script>
```

**Key Entity SEO rules:**
- Use a persistent `@id` URI (e.g., `https://example.com/#organization`) and reference it consistently across all schema on the site.
- Link `sameAs` to authoritative, verified profiles (Wikidata is strongest).
- Ensure brand name, logo, and URL are identical across website, schema, and all external profiles.
- Nest `author` within `Article` schema and reference a `Person` entity with its own `@id`.

```html
<!-- Cross-referencing entities across pages -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "SEO Best Practices 2025",
  "publisher": { "@id": "https://example.com/#organization" },
  "author": {
    "@type": "Person",
    "@id": "https://example.com/#person-jane-smith",
    "name": "Jane Smith",
    "url": "https://example.com/authors/jane-smith",
    "sameAs": [
      "https://www.linkedin.com/in/janesmith",
      "https://twitter.com/janesmith"
    ]
  }
}
</script>
```

### 11. LocalBusiness Schema
For businesses with physical locations:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "@id": "https://example.com/#business",
  "name": "Example Marketing Agency",
  "image": "https://example.com/photos/storefront.jpg",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "123 Main Street",
    "addressLocality": "San Francisco",
    "addressRegion": "CA",
    "postalCode": "94105",
    "addressCountry": "US"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 37.7749,
    "longitude": -122.4194
  },
  "telephone": "+1-555-123-4567",
  "openingHoursSpecification": [
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
      "opens": "09:00",
      "closes": "17:00"
    }
  ],
  "priceRange": "$$"
}
</script>
```

### 12. Retired Schema Types (2025)
Google retired rich result support for these schema types. Remove or deprioritize them:

| Retired Schema | Status |
|---|---|
| Book Actions | No longer generates rich results |
| Course Info | Deprecated |
| Claim Review | Removed |
| Estimated Salary | Removed |
| Learning Video | Deprecated |
| Special Announcement | Removed |
| Vehicle Listing | Removed |
| Practice Problem | Deprecated (Jan 2026) |

> **Focus instead on high-impact schemas:** Organization, Product, Person, LocalBusiness, Article, Event, FAQ, HowTo, Breadcrumb, Video, Review Snippet.

## Verification
- **JSON-LD validation:** Paste output into [Google Rich Results Test](https://search.google.com/test/rich-results) or [Schema.org Validator](https://validator.schema.org/).
- **Content review:** Check if informational blocks start with a direct answer within the first ~50 words.
- **HTML structure check:** Verify `<ul>`, `<ol>`, and `<table>` elements are used for structured data—not `<p>` with comma-separated text.
- **Entity check:** Verify `@id` is consistent across all pages referencing the same entity.

## Failure modes / debugging
| Problem | Cause | Fix |
|---|---|---|
| "Missing field" errors in Rich Results Test | Required properties omitted (e.g., `author`, `datePublished`) | Add all required properties for the schema `@type` |
| Schema not detected | JSON-LD syntax error (trailing commas, unescaped quotes) | Validate JSON with a linter before deploying |
| No Featured Snippet despite good content | Answer not in first 40-50 words, or not using structured HTML | Restructure content using the inverted pyramid pattern |
| Duplicate schema on page | Multiple components inject the same schema type | Centralize schema injection at the page/layout level |
| Knowledge Panel not appearing | Low entity confidence, inconsistent `sameAs` | Ensure `@id`, `sameAs`, and brand name consistency across all profiles |
| Retired schema not generating rich results | Google removed support in 2025 | Remove retired schemas, focus on high-impact types |

## Escalation
- If the page requires a complex nested schema (e.g., `Product` with `AggregateRating`, `Offer`, and `Review`), consult the [Schema.org documentation](https://schema.org/) or a human SEO engineer.
- If schema conflicts with CMS-generated structured data, escalate to the platform admin.
- For Knowledge Panel disputes, use Google's [Knowledge Panel claim process](https://support.google.com/knowledgepanel/answer/7534842).
