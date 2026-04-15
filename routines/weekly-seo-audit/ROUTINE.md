---
name: Weekly SEO Audit
description: Crawl the site, check for broken links, missing meta tags, slow pages, and SEO issues. Produces a prioritized action list.
category: seo
model: Opus 4.6 1M
trigger_type: schedule
trigger_config:
  cron: "0 9 * * 1"
repository: workware-labs/workware-labs-nextjs
connectors:
  - name: Rampify
    url: https://rampify.dev
tools_used:
  - crawl_site
  - get_issues
  - get_page_seo
---

You are running the weekly SEO audit for this site. Follow each step in order and compile a structured report at the end.

## Steps

1. **Crawl the site** using `crawl_site` to discover all indexed pages. Note the total page count and any crawl errors encountered.

2. **Retrieve open issues** using `get_issues`. Categorize each issue by type: missing meta title, missing meta description, duplicate titles, broken canonical URLs, missing structured data, missing alt text, and HTTP errors. Note the severity (critical / warning / info) assigned by the tool.

3. **Spot-check the top 10 pages by estimated traffic** using `get_page_seo` on each. For every page record:
   - Title tag (present, length, keyword alignment)
   - Meta description (present, length, CTA quality)
   - Canonical URL (correct, self-referencing)
   - Structured data / schema markup (type, validity)
   - H1 presence and uniqueness
   - Core Web Vitals flags if surfaced

4. **Identify broken internal links** from the crawl data. List each broken URL, its HTTP status code, and the pages that link to it.

5. **Flag slow pages** (LCP > 2.5 s or TTFB > 600 ms if reported). List the page URL, metric name, and current value.

## Output Format

Produce a Markdown report with these sections:

### Summary
- Total pages crawled
- Total open issues (by severity)
- New issues since last audit (if baseline exists)

### Critical Issues (fix this week)
Numbered list. Each item: issue type, affected URL(s), recommended fix.

### Warnings (fix this sprint)
Numbered list. Same format.

### Spot-Check Results
Table: Page | Title OK | Desc OK | Canonical OK | Schema OK | Notes

### Broken Links
Table: Broken URL | Status | Linking Pages

### Slow Pages
Table: Page URL | Metric | Value | Threshold

### Recommended Next Actions
Up to 5 prioritized action items with estimated effort (S / M / L).
