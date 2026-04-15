---
name: Content Freshness Review
description: Check all published content for outdated information, stale publication dates, deprecated product references, and broken links. Produces a prioritized refresh backlog.
category: seo
model: Opus 4.6 1M
trigger_type: schedule
trigger_config:
  cron: "0 9 1 * *"
repository: workware-labs/workware-labs-nextjs
connectors:
  - name: Rampify
    url: https://rampify.dev
tools_used:
  - crawl_site
  - get_page_seo
---

You are running the monthly content freshness review. Your goal is to surface content that is stale, inaccurate, or degraded in quality so the content team can prioritize refresh work.

## Definition of "Stale"

A piece of content is considered stale if any of the following are true:
- The `datePublished` or visible "last updated" date is more than 12 months ago with no subsequent edits
- The content references a year or date that is now outdated (e.g., "in 2022", "the latest version is X" where X is no longer current)
- It contains external links returning 4xx or 5xx responses
- It references products, features, pricing, or tools that have been deprecated or renamed
- The organic traffic has declined more than 30% compared to 3 months ago (if GSC data is available)

## Steps

1. **Crawl all content pages** using `crawl_site`. Filter to pages under content-heavy paths (e.g., `/blog`, `/guides`, `/docs`, `/resources`). Record the URL, page title, and any published/modified date found in metadata.

2. **Retrieve SEO data** for each content page using `get_page_seo`. Note the `datePublished`, `dateModified`, schema markup presence, word count if available, and any broken internal links flagged.

3. **Flag pages older than 12 months** that have not been updated. Sort by age (oldest first).

4. **Check for explicit date references in titles and descriptions.** If a page title or meta description contains a year (e.g., "Best Tools in 2023"), flag it for update if the year is past.

5. **Identify broken external links.** For any outbound links returning errors, list the page, the broken URL, and what the link anchor text was.

6. **Prioritize refresh candidates.** Score each flagged page on:
   - Age since last update (older = higher priority)
   - Estimated organic traffic (higher traffic = higher priority)
   - Severity of staleness (broken links or wrong year = critical; general outdatedness = normal)

## Output Format

### Summary
- Total content pages reviewed
- Pages flagged for refresh: [n]
- Pages with broken external links: [n]
- Pages with outdated year references in title/meta: [n]

### Critical — Immediate Refresh Needed
Table: Page URL | Title | Last Updated | Issue | Estimated Traffic

### High Priority — Refresh This Month
Table: Page URL | Title | Last Updated | Issue | Estimated Traffic

### Normal Priority — Refresh Backlog
Table: Page URL | Title | Last Updated | Reason for Flagging

### Broken External Links
Table: Page URL | Broken Link | Anchor Text | HTTP Status

### Recommended Actions
- Up to 5 specific content refresh recommendations with suggested new angles or updated information to include.
- Note any content that should be consolidated (multiple thin pages on the same topic).
- Note any content that should be retired (no traffic, outdated, no strategic value).
