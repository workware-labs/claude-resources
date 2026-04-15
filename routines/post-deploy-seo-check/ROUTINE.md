---
name: Post-Deploy SEO Check
description: After each production deploy, verify no SEO regressions were introduced — broken canonicals, missing structured data, changed page titles, or removed meta descriptions.
category: seo
model: Opus 4.6 1M
trigger_type: github_event
trigger_config:
  event: deployment
  branch: main
repository: your-org/your-repo
connectors:
  - name: Rampify
    url: https://rampify.com
tools_used:
  - scan_page
  - get_page_seo
---

You are running the post-deploy SEO regression check. A new deployment to production has just completed. Your goal is to confirm that no SEO signals were degraded by this release.

## Context

The deployment was triggered on the `main` branch. Pull the deployed URL from the environment or deployment payload. If multiple URLs are available (preview vs. production), always check the production domain.

## Steps

1. **Define the pages to check.** Use the following priority list:
   - Homepage (`/`)
   - Top 5 pages by organic traffic (refer to the tracked list in the repo or use GSC data if available)
   - Any pages modified in the most recent deploy (inspect the git diff or deployment manifest for changed routes)

2. **Scan each page** using `scan_page`. Collect: HTTP status code, canonical URL, title tag, meta description, and any schema/structured data blocks.

3. **Deep-check each page** using `get_page_seo`. Compare the following against the known-good baseline (last successful deploy):
   - Title tag: text, length (50–60 chars ideal), keyword presence
   - Meta description: text, length (120–158 chars ideal), not missing
   - Canonical URL: must be self-referencing and use the production domain (not a staging URL)
   - Open Graph tags: `og:title`, `og:description`, `og:image` must be present
   - Structured data: type and `@id` must match pre-deploy values; no `null` or empty fields
   - Robots meta: must not be `noindex` on publicly intended pages

4. **Flag regressions.** A regression is any change from the previous deploy that worsens SEO signals. Examples: title changed without a release note, canonical pointing to a wrong domain, structured data removed, page newly marked `noindex`.

5. **Check redirect integrity.** For any known redirect rules (301/302), verify they still resolve correctly and do not chain more than 2 hops.

## Output Format

### Deploy Info
- Deploy timestamp
- Git SHA or deploy ID
- Production URL checked

### SEO Health: PASS / FAIL

### Regressions Found
If none: "No regressions detected."
If any: numbered list — Page URL | Signal | Before | After | Severity

### Warnings
Non-blocking issues worth noting (e.g., title length borderline, description slightly short).

### Recommended Immediate Actions
If any regressions are CRITICAL (e.g., noindex on homepage, wrong canonical), flag for immediate rollback or hotfix. Otherwise list remediation steps with ticket suggestions.
