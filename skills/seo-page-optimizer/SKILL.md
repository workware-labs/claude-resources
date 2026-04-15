---
name: seo-page-optimizer
description: Analyzes an existing page and produces a concrete SEO improvement plan with updated meta tags, structured data, and on-page fixes. Use this when a page needs an SEO audit or you want to improve its search visibility.
category: seo
connectors:
  - name: Rampify
    url: https://rampify.dev
tools_used:
  - scan_page
  - get_page_seo
  - generate_meta
  - generate_schema
usage_examples:
  - "Optimize the homepage for SEO"
  - "Run SEO analysis on /blog/my-post"
  - "Analyze and optimize the /pricing page for search"
  - "Do a full SEO audit of the /about page"
---

You are performing a full SEO audit and optimization of a specific page. The user will provide a URL path or page name. Follow each step in order.

## Step 1 — Resolve the Target Page

Identify the full URL to analyze. If the user provides a path (e.g. `/blog/my-post`), resolve it against the site's base domain. If the base domain is not obvious, ask the user to confirm before proceeding.

## Step 2 — Crawl and Scan the Page

Use `scan_page` on the target URL to collect raw page data: HTTP status, title, meta description, canonical URL, Open Graph tags, robots directives, heading structure, word count, and link inventory.

## Step 3 — Retrieve SEO Analysis

Use `get_page_seo` on the same URL to get the platform's scored analysis. Record:
- Overall SEO score
- Title tag: present, length, primary keyword alignment
- Meta description: present, length, includes CTA
- Canonical: correct, self-referencing, no conflicts
- Heading hierarchy: H1 count, H2/H3 structure
- Keyword usage: primary keyword in title, H1, first 100 words
- Image alt text: missing or generic alt attributes
- Internal links: count and anchor text quality
- Page speed / Core Web Vitals flags (if surfaced)
- Mobile-friendliness flags (if surfaced)

## Step 4 — Generate Optimized Meta Tags

Use `generate_meta` for the page. Produce:
- Optimized `<title>` tag (50–60 characters, primary keyword near the front)
- Optimized `<meta name="description">` (140–155 characters, includes primary keyword and a CTA)
- `<link rel="canonical">` value
- Open Graph tags: `og:title`, `og:description`, `og:url`, `og:type`, `og:image` placeholder
- Twitter Card tags: `twitter:card`, `twitter:title`, `twitter:description`

## Step 5 — Generate Structured Data

Use `generate_schema` for the page. Select the most appropriate schema type(s):
- `WebPage` or `Article` for content pages
- `BreadcrumbList` for any page with a clear hierarchy
- `FAQPage` if the page contains Q&A content
- `Product` / `Service` for commercial pages
- `Organization` / `WebSite` for the homepage

Output the schema as a valid JSON-LD `<script>` block.

## Step 6 — Identify On-Page Fixes

Based on the scan and SEO analysis, compile a prioritized fix list. Categorize each issue:

- **Critical** (blocks indexing or significantly hurts rankings): missing/duplicate title, missing canonical, noindex set unintentionally, broken H1
- **High** (measurable ranking impact): meta description missing or too short, primary keyword absent from title or H1, duplicate H1s
- **Medium** (user experience and engagement): images missing alt text, generic anchor text on internal links, thin content (under 300 words)
- **Low** (hygiene / best practice): OG tags missing, schema absent, heading hierarchy skipped levels

## Output Format

Deliver a structured Markdown report:

### SEO Audit: [Page URL]
- **Current Score:** (from get_page_seo)
- **Page:** title, URL, word count

### Optimized Meta Tags
Ready-to-paste HTML snippet for `<head>`.

### Structured Data
Ready-to-paste JSON-LD `<script>` block.

### Prioritized Fix List
Table: Priority | Issue | Current Value | Recommended Fix | Effort (S/M/L)

### Implementation Checklist
- [ ] Replace title tag
- [ ] Replace meta description
- [ ] Add/fix canonical
- [ ] Insert JSON-LD schema block
- [ ] (any additional page-specific fixes)
