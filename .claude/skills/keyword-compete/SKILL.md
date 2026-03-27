---
name: keyword-compete
description: >
  Analyze SERP competition and Reddit engagement for top-scored keywords.
  Shows who ranks, how strong they are, and what real people ask on Reddit.
  Third step of the SEO research pipeline.
  Use when: "check competition", "who ranks for", "keyword compete", "analyze competitors".
allowed-tools: Bash, Read, Write, Grep, Glob
---

# Competitive Analysis

Check who ranks and validate with Reddit for the top keywords from `/keyword-score`.

## Config

- Credentials: `source /Users/jack0518/Blog-Post-SEO-/.env`
- Input: latest `/Users/jack0518/Blog-Post-SEO-/research/YYYY-MM-DD/scored.json`
- Output: same directory → `competition.md`
- API cost: ~$0.03-0.05 per run

## Steps

### 1. Load top keywords

Read latest `scored.json`. Take **top 10 by score** where decision is WRITE NOW or HIGH PRIORITY. If fewer than 10 qualify, take all that scored >= 50 (GOOD or above).

### 2. SERP analysis — who ranks in Canada?

Batch keywords into one request where possible:

```bash
source /Users/jack0518/Blog-Post-SEO-/.env
curl -s -X POST "https://api.dataforseo.com/v3/serp/google/organic/live/regular" \
  -H "Content-Type: application/json" \
  -u "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" \
  -d '[
    {"keyword": "keyword1", "location_code": 2124, "language_code": "en"},
    {"keyword": "keyword2", "location_code": 2124, "language_code": "en"}
  ]'
```

Extract top 5 organic results per keyword: `domain`, `title`.

### 3. Classify each ranking domain

Use these exact type labels in the output:

| Type | Examples | What it means for us |
|------|---------|---------------------|
| **Gov** | canada.ca, sba.gov, irs.gov | Won't outrank for #1, aim for #2-5 |
| **Big brand** | Deloitte, BMO, Investopedia, Wealthsimple, TurboTax, HBR | Strong domain, but content is usually generic |
| **Mid-tier** | Accounting firms, M&A blogs, brokers, banks | Beatable with better, more focused content |
| **Reddit** | reddit.com | Weak content landscape — easy to beat |
| **Tool/calc** | Calculator sites, SaaS tools | We may have a comparable tool (valuation) |
| **Competitor** | Acquire.com, BizBuySell, Empire Flippers | Direct M&A marketplace competitors |

### 4. Reddit validation

For each keyword, search with "reddit" appended:

```bash
curl -s -X POST "https://api.dataforseo.com/v3/serp/google/organic/live/regular" \
  -H "Content-Type: application/json" \
  -u "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" \
  -d '[{"keyword": "KEYWORD reddit", "location_code": 2840, "language_code": "en"}]'
```

Uses US (2840) because most English Reddit content is US-based. Still returns r/SmallBusinessCanada and r/personalfinancecanada results.

Extract: thread titles, subreddits, what questions people ask.

Key subreddits: r/smallbusiness, r/Entrepreneur, r/SmallBusinessCanada, r/SellMyBusiness, r/personalfinancecanada, r/CFP

If no Reddit results: note "No Reddit activity — first-mover opportunity."

### 5. Save

**`competition.md`:**
```markdown
# Competitive Analysis — YYYY-MM-DD

Keywords analyzed: [N] | API cost: $[X]

## [KEYWORD 1] (Score: XX, KD: XX)

### SERP — Canada
| # | Domain | Type | Title |
|---|--------|------|-------|
| 1 | ... | Mid-tier | ... |

### Reddit
- r/smallbusiness: "Thread title" — question being asked
- No Reddit activity — first-mover opportunity.

### Gap
- Competitors write: [what they cover]
- Competitors miss: [what they don't cover]
- Our angle: [how Succession AI differentiates]

---

## [KEYWORD 2] (Score: XX, KD: XX)
...
```

Output: "Competition analysis complete — [N] keywords analyzed. Run `/keyword-recommend` for final content plan."
