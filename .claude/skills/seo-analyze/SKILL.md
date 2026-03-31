---
name: seo-analyze
description: >
  Analyze SEO performance, update the learning flywheel, and produce an actionable brief.
  Reads GSC/GA4 data (pasted or described by user), compares against historical snapshots,
  identifies what's working and what's not, and updates all three persistent SEO files.
  Use when: "analyze seo", "seo analyze", "check seo performance", "update seo data",
  "how are our posts doing", "seo report", "daily seo check".
allowed-tools: Bash, Read, Write, Edit, Grep, Glob, WebSearch, WebFetch
---

# SEO Performance Analysis

Ingest performance data, detect patterns, update the flywheel files, and output an actionable brief.

## Setup

`$PROJECT_ROOT`: Resolve via `git rev-parse --show-toplevel` at start of every run. All paths below are relative to this root. If any persistent file is missing, fail with a clear message naming the missing file.

## Persistent Files (READ first, UPDATE at end)

1. **Performance log:** `$PROJECT_ROOT/strategy/seo-performance-log.md` — historical snapshots
2. **Learnings:** `$PROJECT_ROOT/strategy/seo-learnings.md` — confirmed/emerging patterns
3. **Playbook:** `$PROJECT_ROOT/strategy/content-playbook.md` — rules for content creation

## Data Sources

The user may provide data in different ways. Handle all of these:

### Option A: User pastes GSC/GA4 data
Parse whatever format they paste — tables, CSV, screenshots described in text, raw numbers.

### Option B: User describes performance verbally
"The LCGE post is getting 500 impressions but only 5 clicks" — extract structured data from natural language.

### Option C: User asks to check current state
Use WebSearch to check indexation status:
```
site:successionai.org/blog/[slug]
```
Check if pages are indexed and what Google shows. This is the lightweight "daily check" mode.

### Option D: User provides a GSC/GA4 export file
Read the file and parse it.

### Option E: Pull live data from Rybbit
If the user says "pull from rybbit", "use rybbit", or "check traffic":
1. Read `$PROJECT_ROOT/.env` for `RYBBIT_API_KEY`, `RYBBIT_SITE_ID`, `RYBBIT_BASE_URL`
2. Pull overview, page-titles, referrer, and country data via curl (see `/rybbit-analyze` skill for API patterns)
3. Rybbit provides: sessions, pageviews, users, pages/session, bounce rate, session duration, per-page time-on-page, referrer breakdown, geo breakdown
4. Rybbit does NOT provide: GSC impressions, keyword rankings, CTR, avg position — note these gaps in the output
5. Combine Rybbit data with any GSC data the user provides for the fullest picture

## Analysis Steps

### 1. Read all three persistent files

Always start by reading the performance log, learnings, and playbook to understand historical context.

### 2. Normalize input data

Whatever format the user provides, extract into this structure per page:

```
Page: /blog/[slug]
Period: [date range]
Impressions: [N]
Clicks: [N]
CTR: [X%]
Avg Position: [X]
Sessions: [N] (if available)
Avg Time on Page: [Xs] (if available)
Bounce Rate: [X%] (if available)
Top Queries: [list of queries driving traffic] (if available)
```

### 3. Compare against historical data

For each page, compare current metrics against the most recent snapshot in the performance log:

- **Impressions trend:** Growing, flat, or declining?
- **CTR trend:** Improving or degrading? CTR below 2% at position 1-3 = title/description problem.
- **Position trend:** Moving up, down, or stable?
- **New keywords appearing:** Any queries ranking that weren't targeted?

### 4. Cross-page pattern detection

Compare metrics ACROSS pages to find patterns:

- Do posts with embedded tools get more time-on-page? (Compare LCGE post vs non-tool posts)
- Do posts with FAQ schema get higher CTR? (Compare FAQ vs non-FAQ)
- Which post structures (length, format, content type) correlate with better performance?
- Are Canadian-specific keywords ranking faster than generic ones?
- Which funnel stage posts convert best?

### 5. Keyword gap detection

Look for:
- **Unexpected queries:** Keywords driving traffic that we didn't target — these are expansion opportunities
- **Cannibalization:** Multiple pages ranking for the same query — pick a winner, redirect/deoptimize the loser
- **Position 5-15 keywords:** These are "almost there" — can we improve the page to push into top 3?

### 6. Competitive movement

If user provides competitor data or asks, use WebSearch to check:
- Are competitors publishing new content on our target keywords?
- Have SERP features (featured snippets, PAA) changed?

## Updates

### 7. Update performance log

Add a new snapshot entry to `seo-performance-log.md`:

```markdown
### YYYY-MM-DD

| Page | Impressions | Clicks | CTR | Avg Pos | Sessions | Avg Time | Bounce |
|------|-------------|--------|-----|---------|----------|----------|--------|
| /blog/lcge | ... | ... | ... | ... | ... | ... | ... |

**Top gaining keywords:** [keywords with biggest position improvement]
**Declining keywords:** [keywords losing position]
**New keywords discovered:** [queries ranking that we didn't target]
**Notes:** [anything notable]
```

### 8. Update learnings

Review patterns from step 4. For each finding:

- If it matches an existing [EMERGING] learning with new supporting data → upgrade to [CONFIRMED]
- If it's a new pattern → add as [EMERGING] with evidence
- If data contradicts an existing learning → move to [DISPROVED] with explanation
- If an [ASSUMPTION] in the playbook now has data → update the learning and playbook

### 9. Update playbook

If a learning changes from [EMERGING] to [CONFIRMED], update the corresponding playbook rule:
- Remove [ASSUMPTION] tag and add [CONFIRMED]
- Tighten the rule based on what the data actually shows
- Add to the Changelog table at the bottom

If data disproves an assumption, remove or modify the rule.

## Output

### 10. Generate brief

Output a concise brief in this format:

```markdown
## SEO Analysis — YYYY-MM-DD

### Performance Summary
- **Best performing:** [page] — [why]
- **Needs attention:** [page] — [why]
- **Overall trend:** [improving/flat/declining]

### Key Findings
1. [Finding with data]
2. [Finding with data]
3. [Finding with data]

### Opportunities
- [Actionable opportunity with specific next step]
- [Actionable opportunity with specific next step]

### Updated Files
- Performance log: [what was added]
- Learnings: [what changed]
- Playbook: [what changed, or "no changes"]

### Recommended Next Actions
1. [Most impactful action]
2. [Second action]
3. [Third action]
```

## Lightweight Daily Check Mode

If the user just wants a quick check (no data paste), do a minimal version:

1. Read the three persistent files
2. WebSearch `site:successionai.org` to check indexation count
3. WebSearch for top 2-3 target keywords to check current ranking
4. Report: indexed pages, visible rankings, any issues
5. Only update performance log if something notable changed

Output: "Quick check complete — [N] pages indexed, [notable finding]. Full analysis needs GSC/GA4 data."

## Important Rules

- **Never fabricate data.** If you don't have a metric, leave it blank or write "N/A". Don't estimate.
- **Always read the files first.** Context from previous snapshots is critical.
- **Be specific in learnings.** "Tool posts do better" is too vague. "Posts with embedded calculators average 2.5x time-on-page vs non-tool posts (LCGE: 4:32, succession guide: 1:48)" is useful.
- **Date everything.** Every entry, learning, and playbook change should be timestamped.
- **Keep it actionable.** Every finding should connect to a "so what" — what should we do differently?
