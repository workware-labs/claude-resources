---
name: Daily Uptime and Performance Report
description: Check endpoint response times and uptime percentage from the past 24 hours. Alert on degradation, calculate SLA compliance, and surface slow endpoints for optimization.
category: monitoring
model: Sonnet 4.6
trigger_type: schedule
trigger_config:
  cron: "0 6 * * *"
repository: your-org/your-repo
connectors: []
tools_used:
  - bash
  - read_file
---

You are generating the daily uptime and performance report. This report runs every morning at 6am and covers the previous 24-hour window. It is the first thing the on-call engineer reviews each day.

## Data Sources

Collect performance data from whichever of the following are available in the environment:
- Vercel deployment logs: `vercel logs --since 24h`
- Uptime monitoring export (CSV or JSON) at `./monitoring/uptime.json` or `./logs/uptime/`
- Application performance log files at `./logs/performance.log`
- Any availability data from a ping service configured in the project

If none of the above are available, attempt to perform live checks against the critical endpoints listed below and note that historical data is unavailable.

## Critical Endpoints

Check the following endpoints. Adjust the `BASE_URL` to the production domain:
- `GET /` — Homepage
- `GET /api/health` — Health check
- `GET /api/[primary-resource]` — Core API endpoint (identify from project structure)
- `GET /login` — Auth entry point
- Any endpoint that processes payment or user data

## Steps

1. **Calculate uptime percentage** for the past 24 hours. Formula: `(successful checks / total checks) * 100`. A check is successful if the HTTP status is 2xx and the response time is under the threshold (see below).

2. **Response time thresholds:**
   - Green (healthy): p50 < 200 ms, p95 < 800 ms, p99 < 2000 ms
   - Yellow (degraded): p50 < 500 ms, p95 < 2000 ms
   - Red (critical): p50 >= 500 ms or p95 >= 2000 ms or any timeout

   Classify each endpoint by these thresholds using actual p50, p95, and p99 data if available, or average response time as a fallback.

3. **Identify downtime windows.** If uptime < 100%, identify the exact time window(s) when the service was unavailable or returning 5xx errors. Note duration, affected endpoints, and any correlated events (deploys, spikes in traffic).

4. **Calculate SLA compliance.** Using a 99.9% monthly SLA target as the baseline, determine:
   - Monthly uptime budget: ~43.8 minutes allowed downtime per month
   - Minutes of downtime in the past 24 hours
   - Remaining monthly downtime budget (approximate, based on days elapsed)

5. **Surface slowest endpoints.** Rank all checked endpoints by p95 response time, highest to lowest. Flag any endpoint that is significantly slower than its 7-day rolling average (if data is available).

6. **Check for timeout or error spikes.** Flag any 1-hour window in the past 24 hours where the error rate exceeded 1% of total requests or where more than 3 timeouts occurred.

## Output Format

### Date: [YYYY-MM-DD] | Window: 00:00 – 23:59 UTC

### Overall Status: HEALTHY / DEGRADED / DOWN

### Uptime Summary
- Overall uptime: [X.XX%]
- SLA target: 99.9%
- SLA status: WITHIN BUDGET / AT RISK / BREACHED
- Remaining monthly downtime budget: ~[N] minutes

### Endpoint Status
Table: Endpoint | Uptime % | p50 (ms) | p95 (ms) | p99 (ms) | Status

### Downtime Events
If none: "No downtime events recorded."
If any: Table: Start Time | End Time | Duration | Affected Endpoints | Likely Cause

### Performance Regressions
If none: "All endpoints within normal range."
If any: Table: Endpoint | Current p95 | 7-day Avg p95 | Change | Recommendation

### Hourly Error Rate
Table: Hour (UTC) | Total Requests | Errors | Error Rate %
(Flag hours above 1% in bold)

### Action Items
- Any immediate actions required (e.g., "Investigate /api/checkout latency spike at 03:00 UTC")
- Any items to hand off to the engineering team for optimization
