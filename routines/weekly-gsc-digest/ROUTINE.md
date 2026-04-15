---
name: Weekly GSC Digest
description: Pull Google Search Console data, summarize top queries, click-through rates, and impressions trends over the past 7 days.
category: seo
model: Opus 4.6 1M
trigger_type: schedule
trigger_config:
  cron: "0 8 * * 1"
repository: your-org/your-repo
connectors:
  - name: Rampify
    url: https://rampify.com
tools_used:
  - get_gsc_insights
---

You are generating the weekly Google Search Console digest. This report is sent to the marketing and content teams every Monday morning to inform their priorities for the week.

## Steps

1. **Fetch GSC insights** using `get_gsc_insights` for the trailing 7 days (current week) and the prior 7 days (previous week) so you can compute week-over-week deltas.

2. **Top queries analysis.** From the data, identify:
   - Top 20 queries by clicks — include impressions, CTR, and average position
   - Top 10 queries by impressions that have a CTR below 3% (high-impression, low-click opportunities)
   - Queries that rose or fell 5+ positions week-over-week (volatility flags)

3. **Top pages analysis.** Identify:
   - Top 10 landing pages by clicks
   - Pages that gained or lost more than 20% clicks week-over-week
   - Pages with average position between 6–15 (page-2 candidates with upside potential)

4. **Overall metrics summary.** Calculate week-over-week change for:
   - Total clicks
   - Total impressions
   - Average CTR
   - Average position

5. **CTR improvement opportunities.** For pages ranked in positions 1–5 with a CTR below expected benchmarks (pos 1: >25%, pos 2: >15%, pos 3: >10%), flag them as candidates for title/description optimization.

6. **Branded vs. non-branded split.** If query data includes the site's brand name, segment total clicks and impressions into branded and non-branded to show organic growth independently of brand search.

## Output Format

### Week: [date range]

### Overall Performance
Table: Metric | This Week | Last Week | Change (%)

### Top 20 Queries by Clicks
Table: Query | Clicks | Impressions | CTR | Avg Position | WoW Position Change

### High-Impression / Low-CTR Opportunities
Table: Query | Impressions | CTR | Avg Position | Suggested Action

### Top 10 Pages by Clicks
Table: Page | Clicks | Impressions | CTR | Avg Position

### Pages to Watch (Position 6–15)
Table: Page | Avg Position | Clicks | Impressions | CTR

### CTR Improvement Candidates
Numbered list: Page URL | Current CTR | Expected CTR | Recommendation (e.g., rewrite title, improve description)

### Key Takeaways
3–5 bullet points summarizing the most important trends and recommended actions for the week.
