---
name: Daily Error Log Summary
description: Review application error logs from the past 24 hours, categorize by severity and type, identify recurring patterns, and surface anything requiring immediate attention.
category: devops
model: Sonnet 4.6
trigger_type: schedule
trigger_config:
  cron: "0 7 * * *"
repository: your-org/your-repo
connectors: []
tools_used:
  - bash
  - read_file
---

You are generating the daily error log summary. Review all application error output from the past 24 hours and produce a concise briefing for the engineering team.

## Log Sources

Check the following locations for error data (use whichever are present in the environment):
- Application logs: `/var/log/app/error.log` or `./logs/error.log`
- Next.js / Node.js stderr output captured in the deployment platform logs
- Vercel function logs (via `vercel logs --since 24h` if CLI is available)
- Any structured JSON log files under `./logs/` with a timestamp within the past 24 hours

If log access requires a command, run it. If logs are unavailable, note that explicitly and stop.

## Steps

1. **Collect errors from the past 24 hours.** Filter for log entries at level `ERROR`, `FATAL`, `CRITICAL`, or HTTP status >= 500. Parse timestamps to confirm the 24-hour window.

2. **Deduplicate and count occurrences.** Group identical or near-identical error messages together. Track: error message (normalized), first seen, last seen, count.

3. **Categorize each error group** into one of these types:
   - **Application crash / unhandled exception** — process exited or threw uncaught error
   - **Database / query error** — connection failures, timeout, constraint violations
   - **External API / third-party failure** — upstream service errors, 5xx from external calls
   - **Authentication / authorization error** — 401/403 errors beyond normal user mistakes
   - **Not Found / routing error** — unexpected 404s that may indicate broken links or bad deploys
   - **Rate limit / throttle** — 429 responses from external services
   - **Validation / input error** — 400 errors or schema validation failures
   - **Unknown** — anything that doesn't fit above

4. **Identify recurring patterns.** Flag any error group that:
   - Appeared more than 10 times in 24 hours
   - Is new (not seen in the prior 7 days, if a baseline exists)
   - Shows an increasing frequency trend within the 24-hour window

5. **Check for critical signals.** Immediately escalate if any of the following are found:
   - An unhandled exception or process crash
   - Database connection failures sustained for more than 5 minutes
   - Any error affecting the payment or authentication flow
   - Error rate spike: more than 100 errors in any single 1-hour window

## Output Format

### Date: [YYYY-MM-DD] | Window: Last 24 hours

### Overall Health: HEALTHY / DEGRADED / CRITICAL

### Error Summary
Table: Category | Unique Error Groups | Total Occurrences | Trend (↑ / → / ↓)

### Top 10 Errors by Occurrence
Table: # | Error Message (truncated) | Type | Count | First Seen | Last Seen | New?

### Recurring / Spike Alerts
If none: "No spikes or new recurring errors detected."
If any: numbered list with error, count, window, and recommended action.

### Critical Escalations
If none: "No critical issues requiring immediate action."
If any: bold header "ACTION REQUIRED" followed by description and on-call notification recommendation.

### Notes
Any context that would help the team understand the errors (e.g., correlates with a deploy at 14:32, elevated traffic from a marketing campaign, etc.).
