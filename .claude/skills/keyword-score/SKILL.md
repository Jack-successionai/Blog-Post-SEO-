---
name: keyword-score
description: >
  Score and rank discovered keywords by volume, difficulty, trend, and intent.
  Reads from the latest keyword-discover output and produces a ranked table.
  Second step of the SEO research pipeline.
  Use when: "score keywords", "rank keywords", "keyword score", "which keywords should we target".
allowed-tools: Bash, Read, Write, Grep, Glob
---

# Keyword Scoring

Pull volume (CA+US), difficulty, and trend for each keyword. Score and rank them.

## Config

- Credentials: `source /Users/jack0518/Blog-Post-SEO-/.env`
- Input: latest `/Users/jack0518/Blog-Post-SEO-/research/YYYY-MM-DD/discovered.json`
- Output: same directory → `scored.json` + `scored.md`
- Blog posts: `/Users/jack0518/app/landing/lib/blog.ts`
- API cost: ~$0.15-0.25 per run

## Steps

### 1. Load input

Find the most recent date directory in `/Users/jack0518/Blog-Post-SEO-/research/`. Read `discovered.json`.

### 2. Get volume — TWO separate calls (Canada + US)

Batch up to 50 keywords per request. If > 50 keywords, split into multiple requests.

```bash
source /Users/jack0518/Blog-Post-SEO-/.env

# Canada
curl -s -X POST "https://api.dataforseo.com/v3/keywords_data/google_ads/search_volume/live" \
  -H "Content-Type: application/json" \
  -u "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" \
  -d '[{"keywords": ["kw1","kw2",...], "location_code": 2124, "language_code": "en"}]'

# US (same endpoint, location_code 2840)
curl -s ... -d '[{"keywords": ["kw1","kw2",...], "location_code": 2840, "language_code": "en"}]'
```

Per keyword, extract: `search_volume`, `competition`, `competition_index`, `cpc`, `monthly_searches`.

Merge: store `ca_volume` + `us_volume` separately. `total_volume = ca_volume + us_volume`.

### 3. Calculate trend

Using `monthly_searches` array (most recent first):

```
IF monthly_searches is null OR length < 6 → STABLE
recent  = avg of index 0, 1, 2
previous = avg of index 3, 4, 5
IF previous == 0 → STABLE
change = (recent - previous) / previous
IF change > +0.15 → RISING
IF change < -0.15 → FALLING
ELSE → STABLE
```

### 4. Get keyword difficulty

Max 1000 keywords per request.

```bash
curl -s -X POST "https://api.dataforseo.com/v3/dataforseo_labs/google/bulk_keyword_difficulty/live" \
  -H "Content-Type: application/json" \
  -u "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" \
  -d '[{"keywords": ["kw1","kw2",...], "location_code": 2840, "language_code": "en"}]'
```

Response field: `keyword_difficulty` (0-100).
- 0-15: EASY
- 16-30: MODERATE
- 31-50: HARD
- 50+: AVOID
- null: treat as 0 (EASY) — too niche to score = very low competition

### 5. Score

**Null defaults:** CPC null → $0 | KD null → 0 | volume null → 0

```
VOLUME (0-40):           RANKABILITY (0-30):
  >= 3000 total: 40        KD 0-5: 30
  >= 1000: 30              KD 6-15: 25
  >= 500: 25               KD 16-25: 15
  >= 200: 20               KD 26-40: 10
  >= 100: 15               KD 40+: 0
  >= 30: 10

TREND (0-15):            INTENT (0-15):
  RISING: 15               CPC >= $10: 15
  STABLE: 10               CPC >= $5: 12
  FALLING: 3               CPC >= $2: 8
                           CPC < $2: 3

TOTAL = VOLUME + RANKABILITY + TREND + INTENT (max 100)

80-100: WRITE NOW | 65-79: HIGH PRI | 50-64: GOOD | 35-49: BACKLOG | <35: SKIP
```

### 6. Content gap check

Read `/Users/jack0518/app/landing/lib/blog.ts`. Extract each post's `slug`, `title`, `keywords[]`.

Match each scored keyword against blog posts (case-insensitive):
- **COVERED** — keyword appears in a post's `keywords` array OR slug directly targets it
- **PARTIAL** — a related term appears (e.g. "succession planning" in keywords but keyword is "family business succession planning")
- **NO CONTENT** — no match

### 7. Save

**`scored.json`:**
```json
[
  {
    "keyword": "lifetime capital gains exemption",
    "ca_volume": 1900, "us_volume": 110, "total_volume": 2010,
    "kd": 16, "kd_level": "MODERATE",
    "trend": "STABLE", "cpc": 6.86, "competition": "LOW",
    "score": 89, "decision": "WRITE NOW",
    "content_status": "NO CONTENT",
    "monthly_searches_ca": [...], "monthly_searches_us": [...]
  }
]
```

**`scored.md`:**
```markdown
# Keyword Scores — YYYY-MM-DD

Scored: [N] keywords | API cost: $[X]

| Rank | Keyword | CA | US | Total | KD | Trend | CPC | Score | Decision | Content |
|------|---------|----|----|-------|----|-------|-----|-------|----------|---------|
```

Sort by Score descending.

Output: "Scoring complete — [N] keywords ranked. [X] are WRITE NOW, [Y] are HIGH PRI. Run `/keyword-compete` next."
