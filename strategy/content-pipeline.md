# Content Pipeline — The 4-Skill Workflow

How we go from "what should we write?" to a data-backed content plan.

## Overview

```
/keyword-discover  →  /keyword-score  →  /keyword-compete  →  /keyword-recommend
     │                      │                    │                      │
  Expand seeds         Volume, KD,          SERP analysis,       Final "write
  into 100s of         trend, CPC,          Reddit threads,      these 5 next"
  keywords,            scoring model,       competitor types,     with titles,
  filter junk          ranked table         gap analysis          angles, data
     │                      │                    │                      │
  discovered.json  →   scored.json    →    competition.md   →  recommendations.md
```

Run frequency: 2-3 times per week. Total cost per run: ~$0.15-0.50.

## Skill 1: /keyword-discover

**What it does:** Takes 5 seed topics, calls DataForSEO to discover hundreds of related keywords, filters out irrelevant ones, saves a clean list.

**Seeds:** succession planning, sell my business, business valuation, business exit strategy, M&A tax Canada (+ any custom seeds you add)

**API endpoint:** `keywords_for_keywords` — returns 400-700 related keywords per seed

**Filtering rules:**
- KEEP: M&A, succession, exit, valuation, sale tax, due diligence, buyer search
- REMOVE: Corporate HR succession, estate planning/wills, unrelated homonyms ("sell my idea")
- Minimum volume: 30 searches/month

**Output:** `discovered.json` + `discovered.md`

**Cost:** ~$0.075 per seed (~$0.40 for 5 seeds)

---

## Skill 2: /keyword-score

**What it does:** Takes discovered keywords, pulls volume for Canada AND US, gets keyword difficulty, calculates trend, runs the scoring model, checks content gaps against existing blog posts.

**API endpoints:**
- `search_volume` — called twice (Canada 2124, US 2840)
- `bulk_keyword_difficulty` — KD scores

**Scoring:** Volume (40pts) + Rankability (30pts) + Trend (15pts) + Intent (15pts) = 100 max. See [scoring-model.md](scoring-model.md) for full details.

**Content gap check:** Reads blog.ts to mark keywords as COVERED / PARTIAL / NO CONTENT.

**Output:** `scored.json` + `scored.md`

**Cost:** ~$0.15-0.25

---

## Skill 3: /keyword-compete

**What it does:** For the top 10 scored keywords, checks who currently ranks on Google (Canada) and searches for Reddit discussions to validate real-world demand.

**API endpoints:**
- SERP API — top 5 results per keyword in Canada
- SERP API with "reddit" appended — finds Reddit threads

**Competitor classification:** Gov, Big brand, Mid-tier, Reddit, Tool/calc, Competitor

**Reddit subreddits we watch:** r/smallbusiness, r/Entrepreneur, r/SmallBusinessCanada, r/SellMyBusiness, r/personalfinancecanada, r/CFP

**Output:** `competition.md`

**Cost:** ~$0.03-0.05

---

## Skill 4: /keyword-recommend

**What it does:** Reads all previous outputs, selects top 5 keywords (mixing funnel stages), and produces actionable blog post recommendations with titles, angles, competition summary, and traffic projections.

**Selection criteria:**
- WRITE NOW or HIGH PRIORITY score
- NOT already covered by existing blog posts
- Mix of funnel stages (awareness, valuation, active intent, tax/structure)

**Output per recommendation:** Title, target keyword, data (volume/KD/trend/CPC), audience stage, angle vs competitors, companion asset, timeline to page 1.

**Output:** `recommendations.md`

**Cost:** $0 (reads existing files, no API calls)

---

## How to run the pipeline

In Claude Code:

```
/keyword-discover              ← Run this first (or with custom seeds: "employee ownership trust")
/keyword-score                 ← Run after discover
/keyword-compete               ← Run after score
/keyword-recommend             ← Run after compete — gives you "write these 5 next"
```

Each skill reads from the previous skill's output in `/keyword-research/YYYY-MM-DD/`.

You can also re-run individual skills:
- Re-run `/keyword-score` to re-rank with fresh volume data
- Re-run `/keyword-compete` if you want updated SERP results
- Re-run `/keyword-discover` with new seeds to expand the keyword list

---

## Pipeline location

Skills are stored in the main app repo: `/Users/jack0518/app/.claude/skills/`
- `keyword-discover/SKILL.md`
- `keyword-score/SKILL.md`
- `keyword-compete/SKILL.md`
- `keyword-recommend/SKILL.md`

Research outputs are stored in: `/Users/jack0518/app/keyword-research/YYYY-MM-DD/`

DataForSEO credentials: `/Users/jack0518/app/.env`
