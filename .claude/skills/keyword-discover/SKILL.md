---
name: keyword-discover
description: >
  Discover keyword opportunities for Succession AI. Takes seed topics, expands into
  hundreds of related keywords via DataForSEO, filters to relevant ones, and saves
  the raw list. First step of the SEO research pipeline.
  Use when: "discover keywords", "find keywords", "keyword discover", "what are people searching for".
allowed-tools: Bash, Read, Write, Grep, Glob
---

# Keyword Discovery

Expand seed topics into a filtered list of relevant keywords for Succession AI.

## Config

- Credentials: `source $PROJECT_ROOT/.env` (exports `$DATAFORSEO_LOGIN`, `$DATAFORSEO_PASSWORD`)
- Output: `$PROJECT_ROOT/keyword-research/YYYY-MM-DD/`
- `$PROJECT_ROOT`: Resolve via `git rev-parse --show-toplevel` at start of every run
- Location codes: Canada = 2124, US = 2840
- API cost: ~$0.075 per seed (~$0.40 for 5 seeds)

## Default seeds

Always include unless user overrides. If user provides extras, ADD to this list.

1. succession planning
2. sell my business
3. business valuation
4. business exit strategy
5. M&A tax Canada

## Steps

### 1. Setup

```bash
PROJECT_ROOT=$(git rev-parse --show-toplevel)
TODAY=$(date +%Y-%m-%d)
mkdir -p "$PROJECT_ROOT/keyword-research/$TODAY"
source "$PROJECT_ROOT/.env"
```

If `.env` does not exist or credentials are empty, fail immediately with: "DataForSEO credentials not found. Create $PROJECT_ROOT/.env with DATAFORSEO_LOGIN and DATAFORSEO_PASSWORD."

### 2. Discover keywords

For EACH seed, call:

```bash
curl -s -X POST "https://api.dataforseo.com/v3/keywords_data/google_ads/keywords_for_keywords/live" \
  -H "Content-Type: application/json" \
  -u "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" \
  -d '[{"keywords": ["SEED_HERE"], "location_code": 2124, "language_code": "en", "sort_by": "search_volume"}]'
```

Each seed typically returns 400-700 keywords. Sum the `"cost"` field from each response for the cost report.

### 3. Filter

**KEEP** keywords about: M&A, succession planning, business exit, business valuation, business sale tax, buyer search, due diligence, owner transition — AND `search_volume >= 30`.

**REMOVE:**
- Corporate HR succession (CEO replacement, talent management, workforce planning, "30 60 90 day plan")
- Estate planning, wills, personal finance (unless about selling a business)
- Unrelated homonyms ("sell my idea", "selling home", "sell my car")
- Generic business terms not tied to exit/sale ("business plan template", "small business loans")

### 4. Deduplicate

Same keyword from multiple seeds → keep ONE entry with the highest `search_volume`. Use `competition`, `cpc` from that same entry. Track which seed discovered it.

Sort by `search_volume` descending.

### 5. Save

**`discovered.json`** — array sorted by volume:
```json
[
  {
    "keyword": "business succession planning",
    "search_volume": 720,
    "competition": "LOW",
    "competition_index": 0,
    "cpc": 7.57,
    "seed": "succession planning"
  }
]
```

**`discovered.md`** — human-readable:
```markdown
# Keyword Discovery — YYYY-MM-DD

Seeds: [list]
Raw discovered: [N] | After filter: [N] | API cost: $[X]

| # | Keyword | Volume | Competition | CPC | Seed |
|---|---------|--------|-------------|-----|------|
```

### 6. Validate

Before saving, check:
- JSON parses without error
- >= 20 keywords remain (if fewer, warn: "Seeds too narrow — try adding more")
- Every entry has `keyword` (string) and `search_volume` (int >= 0)
- No duplicate keywords

Output: "Discovery complete — [N] keywords found. Run `/keyword-score` next."
