# Keyword Scoring Model

How we decide which keywords to target and in what order.

## The 4 signals

Every keyword is scored out of 100 points across 4 dimensions:

### 1. Volume (0-40 points) — "How big is the audience?"

| Monthly searches (CA + US combined) | Points |
|-------------------------------------|--------|
| >= 3,000 | 40 |
| >= 1,000 | 30 |
| >= 500 | 25 |
| >= 200 | 20 |
| >= 100 | 15 |
| >= 30 | 10 |

Higher volume = more potential traffic from one blog post.

### 2. Rankability (0-30 points) — "How easy is it to get on page 1?"

| Keyword Difficulty (KD) | Points | What it means |
|--------------------------|--------|---------------|
| 0-5 | 30 | Good content alone can rank in 2-4 weeks |
| 6-15 | 25 | Good content ranks in 3-6 weeks |
| 16-25 | 15 | Need content + 3-5 backlinks. 6-10 weeks. |
| 26-40 | 10 | Need great content + many backlinks. 2-3 months. |
| 40+ | 0 | Dominated by massive sites. Avoid. |

We heavily weight this because ranking faster = traffic sooner = compounding returns.

### 3. Trend (0-15 points) — "Is this growing or dying?"

| Direction | Points | How we calculate |
|-----------|--------|------------------|
| RISING (>15% growth) | 15 | Avg of last 3 months vs avg of months 4-6 |
| STABLE | 10 | Within +/- 15% |
| FALLING (>15% decline) | 3 | Don't invest in dying topics |

### 4. Intent (0-15 points) — "Are these buyers or browsers?"

| CPC (what advertisers pay per click) | Points | What it means |
|--------------------------------------|--------|---------------|
| >= $10 | 15 | Very high intent — ready to take action |
| >= $5 | 12 | Good intent — actively researching |
| >= $2 | 8 | Moderate intent |
| < $2 | 3 | Low intent — mostly informational |

CPC is the best proxy for commercial intent. If advertisers pay $16/click, those searchers are valuable.

## Decision thresholds

| Total score | Decision | Action |
|-------------|----------|--------|
| 80-100 | **WRITE NOW** | Drop everything and write this. Best ROI opportunity. |
| 65-79 | **HIGH PRIORITY** | Write within 2 weeks. Strong opportunity. |
| 50-64 | **GOOD** | Worth writing. Schedule for the coming month. |
| 35-49 | **BACKLOG** | Possible future topic. Don't prioritize. |
| < 35 | **SKIP** | Not worth the effort right now. |

## Publishing order logic

After scoring, we order by:

1. **Lowest KD first** — easiest wins build domain authority and momentum. Google starts trusting our site more, making harder keywords easier later.
2. **Time-sensitive topics second** — expiring tax rules, regulatory changes. Write while they're still relevant.
3. **Highest volume last** — these are often higher KD. By the time we write them, our domain authority from posts 1-4 helps us rank.

## Data sources

| Data point | Source | Cost |
|------------|--------|------|
| Search volume | DataForSEO → Google Ads Keyword Planner data | ~$0.075 per batch of 50 keywords |
| Keyword difficulty | DataForSEO → Bulk KD endpoint | ~$0.01 per batch |
| CPC | Included in search volume response | $0 (bundled) |
| Trend | Calculated from 12-month search volume history | $0 (bundled) |
| SERP competitors | DataForSEO → SERP API | ~$0.002 per keyword |
| Reddit validation | DataForSEO → SERP API with "reddit" appended | ~$0.002 per keyword |

**Total cost per full research run: ~$0.15-0.50**
