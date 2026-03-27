---
name: keyword-recommend
description: >
  Produce final content recommendations from keyword research data.
  Reads all previous pipeline outputs and gives actionable "write these next" with
  titles, angles, and reasoning. Final step of the SEO research pipeline.
  Use when: "recommend content", "what should we write", "keyword recommend", "content plan".
allowed-tools: Bash, Read, Write, Grep, Glob
---

# Content Recommendations

Read all pipeline data and produce "write these 5 next" with real data behind every recommendation.

## Config

- Input: latest `/Users/jack0518/Blog-Post-SEO-/research/YYYY-MM-DD/` containing `scored.json`, `competition.md`
- Blog posts: `/Users/jack0518/app/landing/lib/blog.ts`
- Output: same directory → `recommendations.md`
- Existing tool: Succession AI has a free AI-powered business valuation tool at `/valuation`

## Steps

### 1. Load all data

From the most recent date directory:
- `scored.json` — keywords with volume, KD, trend, CPC, score, decision, content_status
- `competition.md` — SERP results, Reddit threads, gap analysis

Also read `/Users/jack0518/app/landing/lib/blog.ts` for existing content (slugs, titles, keywords arrays).

### 2. Select top 5 keywords

From `scored.json`, pick 5 that are:
1. Decision = WRITE NOW or HIGH PRIORITY
2. content_status = NO CONTENT (skip COVERED, deprioritize PARTIAL)
3. Mix of funnel stages — don't pick 5 from the same stage:
   - Stage 1: Awareness ("starting to think about exit")
   - Stage 2: Valuation ("what's my business worth?")
   - Stage 3: Active intent ("ready to sell")
   - Stage 4: Tax & structure ("how to keep more money")

If fewer than 5 qualify as WRITE NOW / HIGH PRI, fill remaining slots from GOOD keywords.

### 3. For each keyword, provide:

**Title:** Compelling, keyword-natural. Not generic — should signal the specific value to a founder.

**Data:** Volume (CA + US) | KD | Trend | CPC

**Audience:** Stage 1-4 + who specifically is searching this (e.g. "family business owners 5-10 years from exit")

**Angle:** What current competitors write vs what they miss. How Succession AI differentiates (founder-focused, $500K-$50M, practical not theoretical).

**Competition:** From `competition.md` — who ranks #1-5, their type (Gov/Big brand/Mid-tier/Reddit), and what it takes to beat them.

**Companion asset:** PDF template, tool embed, calculator, checklist? Reference existing valuation tool at `/valuation` rather than suggesting a new one.

**Timeline to page 1:**
- KD 0-10: 2-6 weeks
- KD 11-20: 6-10 weeks
- KD 21-30: 8-12 weeks

### 4. Traffic projection

For all 5 posts combined:

| Scenario | How to calculate |
|----------|-----------------|
| Conservative (pos 3-5) | Sum of (each keyword's total_volume × 6% CTR) |
| Optimistic (pos 1-2) | Sum of (each keyword's total_volume × 20% CTR) |

Caveat: Featured snippets, ads, and "People Also Ask" can suppress organic CTR. These are estimates.

### 5. Publishing order

Optimize for:
1. **Lowest KD first** — easiest wins, builds domain authority
2. **Time-sensitive topics second** — expiring tax exemptions, regulatory deadlines
3. **Highest volume after** — domain authority from early posts helps rank for harder keywords

### 6. Save

**`recommendations.md`:**
```markdown
# Content Recommendations — YYYY-MM-DD

## Summary
[2-3 sentences: top finding, what to write first, expected impact]

---

## 1. [Blog Post Title]
**Keyword:** [exact keyword] | **Volume:** [CA + US] | **KD:** [X] | **Trend:** [X] | **CPC:** $[X]
**Audience:** Stage [X] — [who is searching]
**Angle:** [how we differentiate]
**Beats:** [who ranks now + their weakness]
**Asset:** [PDF / tool embed / none]
**Timeline:** [X weeks to page 1]

---

## 2. [Blog Post Title]
...

---

## Traffic Projection
| Scenario | Monthly visits (5 posts) |
|----------|------------------------|
| Conservative (pos 3-5) | [X] |
| Optimistic (pos 1-2) | [X] |

## Publishing Order
1. [Title] — KD [X], easiest win
2. [Title] — KD [X], [reason]
3. ...

## Pipeline Cost
- Discovery: $[from discovered.md]
- Scoring: $[from scored.md]
- Competition: $[from competition.md]
- Total: $[sum]
```

Output the top recommendation and ask: "Want to start writing [Title]?"
