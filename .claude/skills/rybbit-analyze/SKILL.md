---
name: rybbit-analyze
description: >
  Pull live traffic data from Rybbit analytics, analyze page performance, referrers, and geo,
  compare against historical snapshots, and output actionable insights for the SEO flywheel.
  Use when: "rybbit analyze", "check traffic", "how's the site doing", "traffic report",
  "rybbit data", "analytics report", "site performance", "check rybbit".
allowed-tools: Bash, Read, Write, Edit, Grep, Glob
---

# Rybbit Analytics Analysis

Pull live traffic data from Rybbit and feed it into the SEO flywheel.

## Setup

`$PROJECT_ROOT`: Resolve via `git rev-parse --show-toplevel` at start of every run.

### Load API credentials

Read `$PROJECT_ROOT/.env` and extract:
- `RYBBIT_API_KEY` — Bearer token for API auth
- `RYBBIT_SITE_ID` — numeric site ID (default: 4956)
- `RYBBIT_BASE_URL` — API base (default: https://app.rybbit.io)

If `.env` is missing, fail with: "Missing .env file — create one with RYBBIT_API_KEY, RYBBIT_SITE_ID, RYBBIT_BASE_URL"

### API base pattern

All API calls use this pattern:
```bash
curl -s -X GET "${RYBBIT_BASE_URL}/api/sites/${RYBBIT_SITE_ID}/{endpoint}?{params}" \
  -H "Authorization: Bearer ${RYBBIT_API_KEY}"
```

## Persistent Files (READ first, UPDATE at end)

1. **Performance log:** `$PROJECT_ROOT/strategy/seo-performance-log.md`
2. **Learnings:** `$PROJECT_ROOT/strategy/seo-learnings.md`
3. **Playbook:** `$PROJECT_ROOT/strategy/content-playbook.md`

## Data Pull

### 1. Determine date range

- **Default:** Last 30 days (today minus 30 days to today)
- **If user specifies:** Use their range
- **Always use:** `time_zone=America%2FToronto`

### 2. Pull all data (run these curl calls in parallel)

**Overview:**
```
/api/sites/{siteId}/overview?start_date={start}&end_date={end}&time_zone=America%2FToronto
```
Returns: sessions, pageviews, users, pages_per_session, bounce_rate, session_duration

**Page titles (top pages):**
```
/api/sites/{siteId}/page-titles?start_date={start}&end_date={end}&time_zone=America%2FToronto
```
Returns: array of {value (title), pathname, count, percentage, time_on_page_seconds}

**Referrers:**
```
/api/sites/{siteId}/metric?start_date={start}&end_date={end}&time_zone=America%2FToronto&parameter=referrer
```
Returns: {data: {data: [{value, count, percentage, bounce_rate}]}}

**Countries:**
```
/api/sites/{siteId}/metric?start_date={start}&end_date={end}&time_zone=America%2FToronto&parameter=country
```
Returns: {data: {data: [{value (country code), count, percentage, bounce_rate}]}}

**Devices:**
```
/api/sites/{siteId}/metric?start_date={start}&end_date={end}&time_zone=America%2FToronto&parameter=device_type
```

**Overview bucketed (daily trend):**
```
/api/sites/{siteId}/overview-bucketed?start_date={start}&end_date={end}&time_zone=America%2FToronto&bucket=day
```

### 3. Optional deep dives (only if user asks or data warrants it)

**Sessions list:**
```
/api/sites/{siteId}/sessions?start_date={start}&end_date={end}&time_zone=America%2FToronto&limit=20
```

**Live user count:**
```
/api/sites/{siteId}/live-user-count
```

**Outbound links:**
```
/api/sites/{siteId}/outbound-links?start_date={start}&end_date={end}&time_zone=America%2FToronto
```

## Analysis Steps

### 4. Read all three persistent files

Load performance log, learnings, and playbook for historical context.

### 5. Page-level analysis

For each page in the page-titles response, extract:

```
Page: [pathname]
Title: [value]
Pageviews: [count]
% of Total: [percentage]
Avg Time on Page: [time_on_page_seconds]s
```

Identify:
- **Best performers:** Highest pageviews AND highest time-on-page (engagement, not just volume)
- **High-intent pages:** High time-on-page but low traffic (underexposed — SEO opportunity)
- **Bounce concerns:** Compare page bounce rates if available from sessions data
- **Blog vs non-blog:** Separate /blog/* pages from /sell-business, /succession-planning, /valuation, /about, /contact to analyze content marketing vs product pages separately

### 6. Referrer analysis

- **Organic search %:** google.com + bing.com + duckduckgo.com + other search engines
- **Social %:** linkedin.com + facebook.com + twitter.com
- **Direct/other %:** Everything else
- **Referrer quality:** Which referrers have lowest bounce rate? (= highest quality traffic)
- **Emerging channels:** Any new referrers appearing that weren't in previous snapshots?

### 7. Geographic analysis

- **Target market fit:** What % is CA + US? (should be >80% for Succession AI)
- **Unexpected geo:** Any country with significant traffic that isn't a target market?
- **Geo bounce rates:** Do non-target countries bounce more? (expected — confirms targeting is right)

### 8. Trend analysis (if bucketed data available)

- **Traffic direction:** Is daily traffic growing, flat, or declining over the period?
- **Day-of-week patterns:** Any consistent spikes? (e.g., more traffic on weekdays = B2B signal)
- **Anomalies:** Any single-day spikes? (could indicate a mention, backlink, or viral post)

### 9. Cross-reference with SEO data

Compare Rybbit data against the performance log:
- **Blog posts with GSC impressions but low Rybbit sessions:** Ranking but not converting clicks to engaged visits
- **Pages with high Rybbit time-on-page:** Strong content — what can we learn from their structure?
- **Referrer trends vs previous snapshot:** Growing or shrinking channels?

## Updates

### 10. Update performance log

Add a Rybbit snapshot entry:

```markdown
### YYYY-MM-DD (Rybbit)

**Site Overview (last 30 days):**
| Sessions | Users | Pageviews | Pages/Session | Bounce Rate | Avg Duration |
|----------|-------|-----------|---------------|-------------|-------------|

**Top Pages:**
| Page | Pageviews | % Total | Avg Time |
|------|-----------|---------|----------|

**Referrer Breakdown:**
| Source | Sessions | % | Bounce Rate |
|--------|----------|---|-------------|

**Geo:** CA [X]% | US [X]% | Other [X]%

**Notes:** [notable findings, changes from last snapshot]
```

### 11. Update learnings (if warranted)

Only update if data reveals a new pattern or confirms/disproves an existing one:
- High time-on-page on tool pages → supports [EMERGING] tool engagement learning
- Referrer quality differences → new learning about traffic sources
- Geo patterns → confirms/updates Canada-focused strategy

### 12. Update playbook (if warranted)

Only if a learning moves from [EMERGING] to [CONFIRMED] based on Rybbit data.

## Output

### 13. Generate brief

```markdown
## Rybbit Traffic Analysis — YYYY-MM-DD

### Site Health
- **Sessions:** [N] ([+/- vs last snapshot])
- **Users:** [N]
- **Pageviews:** [N]
- **Engagement:** [pages/session] pages/session, [duration]s avg duration
- **Bounce rate:** [X]%

### Top Performing Content
1. [Page] — [pageviews] views, [time]s avg time
2. [Page] — [pageviews] views, [time]s avg time

### Underexposed High-Intent Pages
- [Page] — only [N] views but [X]s time-on-page (opportunity to drive more traffic here)

### Traffic Sources
- Organic: [X]% (google + bing + others)
- Referral: [X]% (top: [source])
- Social: [X]% (top: [source])

### Key Insights
1. [Insight with data]
2. [Insight with data]
3. [Insight with data]

### Recommended Actions
1. [Action tied to data]
2. [Action tied to data]
3. [Action tied to data]

### Files Updated
- Performance log: [what was added]
- Learnings: [what changed, or "no changes"]
- Playbook: [what changed, or "no changes"]
```

## Quick Check Mode

If user just wants a fast pulse check:

1. Pull overview only (1 API call)
2. Compare against last snapshot in performance log
3. Output: "[N] sessions last 30 days ([+/- vs last]). Bounce rate [X]%. Top page: [page]. Full analysis: run `/rybbit-analyze`."

## Important Rules

- **Never fabricate data.** Only report what the API returns.
- **Always read persistent files first.** Historical context makes the analysis valuable.
- **Be specific.** "Traffic is up" is useless. "Sessions up 23% (1039 vs 845) driven by organic search growth" is useful.
- **Connect to the flywheel.** Every insight should connect to "what should we write next" or "what should we optimize."
- **Date everything.** Every snapshot entry is timestamped.
- **Respect API credits.** Don't make unnecessary calls. The 6 parallel calls in step 2 are sufficient for most analyses.
