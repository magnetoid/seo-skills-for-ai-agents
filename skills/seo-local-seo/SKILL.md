---
title: Local SEO & Location Intelligence Standards
description: Rules for implementing LocalBusiness schema, geographical signals, NAP consistency, and multi-location site architectures.
version: 2.0.0
---

# Local SEO & Location Intelligence Standards

## When to use
Use this skill when:
- Building or refactoring pages for businesses with physical locations (brick-and-mortar storefronts, service area businesses, regional offices).
- Implementing structured data for local entities (e.g., `LocalBusiness`, `Store`, `Restaurant`, `ProfessionalService`).
- Designing footer or contact page elements containing business Name, Address, and Phone (NAP).
- Structuring location directories, locator maps, or multi-location subfolders (e.g., `/locations/`).

## Inputs required
- Full registered legal/operating name of the business.
- Exact address details matching the Google Business Profile (GBP).
- Dedicated local telephone number.
- Geocoordinates (latitude and longitude).
- Operating hours and price range (if applicable).
- Target URL structure for multi-location businesses.

## Procedure

### 1. Robust `LocalBusiness` JSON-LD Structured Data
- Every location page or primary site homepage (if single location) MUST contain high-fidelity JSON-LD schema.
- Use the most specific Schema.org type (e.g., `Dentist` instead of `LocalBusiness`, `AutomotiveBusiness` instead of `Organization`).
- Link your schema to authority sources using `sameAs` (e.g., Wikipedia/Wikidata entity pages, official social profiles, GBP CID link).

```html
<!-- ✅ CORRECT — Embedded JSON-LD for a Specific Local Business -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "LegalService",
  "@id": "https://example.com/#legal-service",
  "name": "Apex Law Partners",
  "url": "https://example.com",
  "telephone": "+1-555-019-2834",
  "priceRange": "$$$",
  "logo": "https://example.com/assets/logo.png",
  "image": "https://example.com/assets/office-facade.jpg",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "100 Pine Street, Suite 1200",
    "addressLocality": "San Francisco",
    "addressRegion": "CA",
    "postalCode": "94111",
    "addressCountry": "US"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 37.7936,
    "longitude": -122.3998
  },
  "openingHoursSpecification": [
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
      "opens": "09:00",
      "closes": "18:00"
    }
  ],
  "sameAs": [
    "https://www.facebook.com/apexlawpartners",
    "https://www.linkedin.com/company/apex-law-partners",
    "https://maps.google.com/?cid=1234567890123456789"
  ]
}
</script>
```

### 2. Strict NAP Consistency
- Business Name, Address, and Phone (NAP) listed on the page MUST match Google Business Profile (GBP) and other citations character-for-character.
- Present NAP in a clean, semantic semantic block. Never embed NAP within an image; it must be fully machine-readable crawlable text.

```html
<!-- ✅ CORRECT — Semantic HTML for NAP -->
<address class="contact-info">
  <strong class="brand-name">Apex Law Partners</strong><br>
  <span class="street-address">100 Pine Street, Suite 1200</span><br>
  <span class="locality-zip">San Francisco, CA 94111</span><br>
  <span class="phone-label">Phone: </span><a href="tel:+15550192834" class="phone-link">+1 (555) 019-2834</a>
</address>

<!-- ❌ WRONG — Non-semantic styling, un-clickable phone number -->
<div class="footer-contact">
  Apex Law Partners SF Office<br>
  100 Pine St, #1200, San Francisco CA<br>
  Call: 555-019-2834
</div>
```

### 3. Multi-Location Directory & URL Structure
- Multi-location sites must have a clean URL structure: `example.com/locations/state/city/` or `example.com/locations/store-name`.
- Use a single, comprehensive locator page (`/locations`) that links to state/city directories with clean crawlable links. Do not block these directories in `robots.txt`.
- Each location page must feature unique local content (local team bios, local landmarks/directions, specific local reviews). Avoid boilerplate text duplication.

```html
<!-- ✅ CORRECT — Multi-Location Breadcrumbs and Canonicalization -->
<!-- URL: https://example.com/locations/ca/san-francisco -->
<link rel="canonical" href="https://example.com/locations/ca/san-francisco" />

<nav aria-label="Breadcrumb">
  <ol class="breadcrumb-list">
    <li><a href="/">Home</a></li>
    <li><a href="/locations">Locations</a></li>
    <li><a href="/locations/ca">California</a></li>
    <li><a href="/locations/ca/san-francisco" aria-current="page">San Francisco</a></li>
  </ol>
</nav>
```

### 4. Geo-Targeted Header & Title Optimization
- Title tags and Main Heading (`<h1>`) must contain primary geocomponents organically.
- Format: `[Service] in [City, State] | [Brand]`

```html
<!-- ✅ CORRECT Title and H1 alignment -->
<title>Corporate Law Firm in San Francisco, CA | Apex Law Partners</title>
<h1>San Francisco Corporate Attorneys</h1>
```

## Verification
- **Schema Validation:** Use the Schema.org validator or local JSON schemas to confirm the structured data passes without errors or warnings.
- **NAP Audit:** Cross-reference the exact spelling, formatting, and digits of the business name, address, and phone number against the client's Google Business Profile.
- **Link Audits:** Ensure every location listing in the central index page links to its individual location page using an `<a href>` tag rather than an interactive JS map click handler.
- **Mobile Friendliness:** Check tap target sizes for the click-to-call link (`tel:`); they must be at least 48x48px to prevent errors in mobile usability audits.

## Failure modes / debugging
| Problem | Cause | Fix |
|---|---|---|
| "Schema warning: missing field" | JSON-LD lacks `priceRange`, `telephone`, or `image` | Add the requested fields. Set `priceRange` to `$$` or similar fallback if not publicly listed. |
| "NAP Mismatch" | The street address on the website uses abbreviations (e.g., "St.") while GBP uses full words ("Street") | Format the website HTML exactly to match the official Google Business Profile. |
| "Location pages not indexed" | Directory relies on AJAX/interactive maps to reveal location pages | Build a flat HTML link index (sitemap or HTML directory) listing all location links. |
| "Duplicate Content Penalization" | Boilerplate content copied exactly across multiple city landing pages | Rewrite the introduction, directions, reviews, and team profiles for each specific location. |

## Escalation
- If the business name or location address is undergoing a rebrand/change, do not update the website until the official Google Business Profile change has been submitted and verified to avoid instant suspensions.
- If there is uncertainty regarding geocoordinates, consult an SEO strategist to verify lat/long inputs from map APIs.
