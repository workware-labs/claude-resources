---
name: Post-Deploy Smoke Test
description: After each production deploy, hit critical application endpoints, verify HTTP responses, check for JavaScript errors, and confirm key user flows are operational.
category: devops
model: Sonnet 4.6
trigger_type: github_event
trigger_config:
  event: deployment
  branch: main
repository: workware-labs/workware-labs-nextjs
connectors: []
tools_used:
  - bash
  - read_file
---

You are running the post-deploy smoke test. A deployment to production just completed. Your job is to verify the application is healthy before the team considers the deploy done.

## Critical Endpoints to Check

Check the following endpoint categories. Replace `BASE_URL` with the production domain from the deployment environment.

### Public Pages (expect HTTP 200)
- `GET /` — Homepage
- `GET /about`
- `GET /pricing` (if applicable)
- `GET /blog` or `/articles` (if applicable)
- `GET /sitemap.xml` — must return valid XML with at least one `<url>` entry
- `GET /robots.txt` — must not block all crawlers on production

### API Endpoints (expect HTTP 200 or appropriate status)
- `GET /api/health` or `/api/ping` — health check endpoint; expect `{ "status": "ok" }` or HTTP 200
- `GET /api/version` (if present) — verify the deployed version matches the expected git SHA or version tag

### Auth Endpoints (expect correct redirect or response, NOT 500)
- `GET /login` — must return 200, not redirect loop
- `GET /dashboard` — must redirect to login (expect 302/307), not return 500

### Static Assets
- Verify the main JS bundle loads (check for a `<script src="/...">` tag in the homepage HTML)
- Verify the main CSS bundle loads (check for a `<link rel="stylesheet">` tag)
- Confirm no asset URLs return 404

## Steps

1. **Run HTTP checks** against each endpoint above. For each, record: URL, expected status, actual status, response time (ms), and PASS/FAIL.

2. **Check for server errors in responses.** If any 5xx response is received, immediately mark the deploy as FAILED and escalate.

3. **Inspect the homepage HTML** for obvious render failures:
   - `<title>` tag must be present and non-empty
   - `<body>` must not contain a raw error stack trace or "Internal Server Error" text
   - `__NEXT_DATA__` (or framework equivalent) must be present and parseable JSON

4. **Verify the health check endpoint** returns a successful response within 1000 ms. If it times out or returns an error, mark FAILED.

5. **Check for redirect correctness.** Confirm `http://` redirects to `https://`, and `www.` redirects to the apex domain (or vice versa per site convention).

## Output Format

### Deploy Info
- Timestamp
- Environment: production
- Base URL checked

### Smoke Test Result: PASS / FAIL / DEGRADED

### Endpoint Check Results
Table: URL | Expected Status | Actual Status | Response Time | Result

### Issues Found
If none: "All checks passed."
If any failures: numbered list with URL, failure type, and recommended action.

### Escalation
If overall result is FAIL: "ESCALATE: Production is degraded. Notify on-call engineer. Consider rollback."
If overall result is DEGRADED: "WARNING: Non-critical issues detected. Review before next deploy."
