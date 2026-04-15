---
name: Weekly Lighthouse Audit
description: Run Lighthouse on key pages, track Core Web Vitals trends over time, flag regressions from the prior week, and produce actionable optimization recommendations.
category: monitoring
model: Sonnet 4.6
trigger_type: schedule
trigger_config:
  cron: "0 2 * * 0"
connectors: []
tools_used:
  - bash
  - read_file
---

You are running the weekly Lighthouse audit. This job runs every Sunday at 2am when traffic is minimal, ensuring measurements are not skewed by concurrent load. Results feed into the performance trend tracker so the team can catch regressions before users notice them.

## Pages to Audit

Run Lighthouse against the following pages (update this list to match the site's key pages):
1. Homepage (`/`)
2. Core product or feature page (identify from the project's main routes)
3. Blog or content index page (`/blog` or `/articles` if present)
4. A representative long-form content page (e.g., the most-visited blog post)
5. Pricing page (`/pricing` if present)
6. Login / sign-up page (`/login` or `/signup` if present)

## Lighthouse Configuration

Use the following settings for consistency:
- **Device**: both mobile and desktop
- **Throttling**: simulated 4G (Lighthouse default) for mobile; no throttling for desktop
- **Categories**: Performance, Accessibility, Best Practices, SEO
- **Runs per page**: 3 (take the median result to reduce variance)

Run Lighthouse via CLI if available: `npx lighthouse <url> --output json --output-path ./lighthouse-results/[slug]-[date].json --preset desktop` (and repeat with `--preset mobile`).

If Lighthouse CLI is not available, note the limitation and perform manual checks against PageSpeed Insights API or equivalent.

## Core Web Vitals Thresholds

| Metric | Good | Needs Improvement | Poor |
|--------|------|-------------------|------|
| LCP    | ≤ 2.5 s | 2.5–4.0 s | > 4.0 s |
| INP    | ≤ 200 ms | 200–500 ms | > 500 ms |
| CLS    | ≤ 0.1 | 0.1–0.25 | > 0.25 |
| TTFB   | ≤ 800 ms | 800–1800 ms | > 1800 ms |
| FCP    | ≤ 1.8 s | 1.8–3.0 s | > 3.0 s |

## Steps

1. **Run the audits** on each page for both mobile and desktop. Collect scores (0–100) for each Lighthouse category plus the raw metric values listed above.

2. **Load last week's results** from `./lighthouse-results/` (most recent JSON files per page) to compute week-over-week deltas.

3. **Identify regressions.** A regression is defined as:
   - Overall Performance score drops 5+ points from last week
   - Any Core Web Vital moves from "Good" to "Needs Improvement" or worse
   - Accessibility score drops 3+ points
   - SEO score drops any points

4. **Identify improvements.** Note any metric that moved from a worse to a better category, or any score that improved 5+ points.

5. **Extract top Lighthouse opportunities.** From the audit results, collect the top 3 "Opportunities" and top 3 "Diagnostics" per page. These are Lighthouse's suggested fixes ranked by potential impact (in estimated seconds saved or score points).

6. **Compute trend.** Using the available historical results (up to 8 weeks of data if present), note the direction (improving / stable / declining) for each key metric on the homepage.

## Output Format

### Audit Date: [YYYY-MM-DD] | Time: 02:00 UTC

### Overall Status: ALL PASSING / REGRESSIONS DETECTED / CRITICAL REGRESSION

### Scores by Page (Mobile)
Table: Page | Performance | Accessibility | Best Practices | SEO | WoW Perf Change

### Scores by Page (Desktop)
Table: Page | Performance | Accessibility | Best Practices | SEO | WoW Perf Change

### Core Web Vitals — Mobile
Table: Page | LCP | INP | CLS | TTFB | FCP | LCP Status | CLS Status | INP Status

### Regressions
If none: "No regressions detected this week."
If any: Table: Page | Device | Metric | Last Week | This Week | Delta | Severity

### Improvements
Table: Page | Device | Metric | Last Week | This Week | Delta

### Top Optimization Opportunities
For each page with a Performance score below 90, list:
- Page URL
- Top 3 Lighthouse Opportunities (description + estimated savings)
- Top 2 Diagnostics (description + impact)

### 8-Week Trend (Homepage, Mobile Performance Score)
Mini table or sparkline data: Week | Score

### Recommended Actions
1. Prioritized list of the highest-impact optimizations to address this sprint (focus on items affecting Core Web Vitals or pages with score below 80).
2. Estimate effort for each (S / M / L).
3. Note any issues that require infrastructure changes vs. code changes.
