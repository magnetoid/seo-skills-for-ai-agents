---
title: Advanced SEO AI Crawlers & Citations
description: Instructions for optimizing content for LLMs, AI search bots, Google AI Overviews, and controlling AI crawler access.
version: 2.0.0
---

# Advanced SEO: AI Crawlers & Citations

## When to use
Use this skill when:
- Optimizing a website for AI search engines (ChatGPT Search, Perplexity, Google AI Overviews).
- Establishing E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness) signals.
- Providing readable content fallback for non-browser-based crawlers.
- Adding or referencing factual claims that need attribution.

## Inputs required
- The site's primary content areas and page inventory.
- Whether the site publishes factual/statistical content that needs citations.
- The framework's ability to serve alternative content formats (e.g., query parameters or API routes).

## Procedure

### 1. LLM Root Files
- Create and maintain a `public/llms.txt` file at the domain root.
- Format it as clean Markdown summarizing the site's structure, purpose, and key pages.
- Update it whenever major pages or features are added.

```markdown
# Example Corp

> Example Corp helps developers build SEO-optimized web applications.

## Main Pages
- [Homepage](https://example.com/) — Overview and getting started
- [Blog](https://example.com/blog) — Technical articles and guides
- [Documentation](https://example.com/docs) — API reference and tutorials
- [Pricing](https://example.com/pricing) — Plans and features

## Key Topics
- Technical SEO best practices
- Core Web Vitals optimization
- Structured data implementation
```

### 2. Citation Architecture
- When stating facts, data points, or statistics, wrap them in `<blockquote>` with `<cite>`.
- Link the citation to its original source.
- Never use visual-only quoting styles (e.g., `<div class="quote">`) without semantic elements.

```html
<!-- ✅ CORRECT — Semantic citation -->
<blockquote cite="https://web.dev/vitals/">
  <p>Core Web Vitals are a set of metrics that measure real-world user
  experience for loading performance, interactivity, and visual stability.</p>
  <footer>— <cite><a href="https://web.dev/vitals/">web.dev</a></cite></footer>
</blockquote>

<!-- ❌ WRONG — No semantic markup -->
<div class="quote-box">
  <p>"Core Web Vitals are a set of metrics..."</p>
  <span class="source">Source: web.dev</span>
</div>
```

### 3. Markdown Fallbacks
- For text-heavy dynamic routes, support a `?format=md` query parameter.
- When detected, return raw Markdown content without UI chrome, CSS, or JavaScript.
- This allows LLM crawlers to ingest clean text content.

```javascript
// Next.js API route example
export async function GET(request) {
  const { searchParams } = new URL(request.url);
  const format = searchParams.get('format');

  const content = await getPageContent();

  if (format === 'md') {
    return new Response(content.markdown, {
      headers: { 'Content-Type': 'text/markdown; charset=utf-8' },
    });
  }

  return renderFullPage(content);
}
```

### 4. Google AI Overviews & AI Features
Per [Google's AI features documentation](https://developers.google.com/search/docs/appearance/ai-features):

- Google's AI Overviews use existing Search index content — no separate opt-in needed.
- Content that ranks well in Search is eligible for AI Overviews.
- To **opt out** of AI Overviews, use the `nosnippet` meta tag (this also removes your regular snippets).

```html
<!-- Opt out of AI Overviews AND regular snippets -->
<meta name="robots" content="nosnippet" />

<!-- Opt out of specific text being used -->
<span data-nosnippet>This text won't appear in AI Overviews or snippets.</span>
```

> **Important:** There is no way to opt out of AI Overviews while keeping regular search snippets.

### 5. AI Crawler Control via robots.txt
Control which AI services can crawl your content:

```txt
# robots.txt — Control AI crawler access

# Block Google's AI training crawler (does NOT affect Search indexing)
User-agent: Google-Extended
Disallow: /

# Block OpenAI's crawler
User-agent: GPTBot
Disallow: /

# Block Common Crawl
User-agent: CCBot
Disallow: /

# Allow traditional Googlebot (required for Search)
User-agent: Googlebot
Allow: /
```

| Crawler | Company | Blocking Effect |
|---|---|---|
| `Google-Extended` | Google | Blocks use in Gemini/AI training; does NOT affect Google Search |
| `GPTBot` | OpenAI | Blocks use in ChatGPT training |
| `CCBot` | Common Crawl | Blocks Common Crawl datasets |
| `anthropic-ai` | Anthropic | Blocks Claude training |
| `Googlebot` | Google | **Never block** — required for Google Search indexing |

> **Critical:** Blocking `Google-Extended` does NOT affect your Google Search rankings or AI Overviews. AI Overviews use Googlebot data.

### 6. Structured Authorship for AI Attribution
AI search engines are more likely to cite content with clear authorship signals:

```html
<!-- Author markup for AI citation -->
<article>
  <header>
    <h1>Article Title</h1>
    <address>
      By <a rel="author" href="/authors/name">Author Name</a>,
      <span>Senior Developer at Example Corp</span>
    </address>
  </header>
  <!-- Content with inline citations -->
</article>
```

## Verification
- **Route check:** Confirm `https://yoursite.com/llms.txt` returns valid, up-to-date Markdown.
- **Semantic HTML check:** Validate `<blockquote>` and `<cite>` usage on pages with factual claims.
- **Endpoint check:** Confirm `?format=md` strips UI wrappers and returns clean Markdown.
- **Crawler test:** Use `curl` to verify responses are plain text/Markdown when requested.

## Failure modes / debugging
| Problem | Cause | Fix |
|---|---|---|
| `llms.txt` returns 404 | File not placed in `public/` or not deployed | Ensure the file is at `public/llms.txt` and included in the build output |
| AI search engines don't cite your content | Missing `<cite>` / `<blockquote>` semantic markup | Wrap factual claims in proper HTML citation elements |
| `?format=md` returns full HTML | Query parameter not handled in route logic | Add format detection middleware or route handler |
| Stale `llms.txt` content | File not updated when pages are added | Add `llms.txt` update to the content publishing workflow |

## Escalation
- If the site's architecture makes it difficult to serve alternate content formats (e.g., statically generated sites without middleware), consult a human engineer for edge function or CDN-level solutions.
- If citation requirements involve legal or compliance considerations, escalate to the legal/content team.
