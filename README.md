# Blog Post SEO — Succession AI

Source of truth for Succession AI's data-driven SEO content strategy. Everything here drives what blog posts we write, why, and in what order.

## How this works

```
1. DISCOVER    →  2. SCORE    →  3. COMPETE    →  4. RECOMMEND    →  5. WRITE
   Find what        Rank by        Check who         Pick the           Publish
   people           volume,        ranks and         top 5 and          blog
   search for       difficulty,    what Reddit       produce            posts
                    trend          says              titles/angles
```

We use **DataForSEO API** (Google Ads data) to get real search volume, keyword difficulty, and SERP competition data. Results are stored in `/research/` by date.

## Repository Structure

```
Blog-Post-SEO-/
├── README.md                    ← You are here
├── strategy/
│   ├── how-seo-works.md         ← SEO fundamentals for the team
│   ├── scoring-model.md         ← How we score and prioritize keywords
│   ├── content-pipeline.md      ← The 4-skill pipeline explained
│   └── competitive-landscape.md ← Who we're up against
├── research/
│   └── 2026-03-26/              ← First research run
│       ├── discovered.json      ← Raw keyword list
│       ├── scored.json          ← Keywords ranked by score
│       ├── competition.md       ← SERP + Reddit analysis
│       └── recommendations.md   ← "Write these 5 next"
├── content-plan/
│   ├── publishing-order.md      ← What to write and when
│   └── blog-post-briefs/        ← Individual briefs per post
└── technical-seo/
    └── completed.md             ← Technical SEO work done
```

## Quick Links

- [What to write next →](content-plan/publishing-order.md)
- [Latest keyword research →](research/2026-03-26/recommendations.md)
- [How our scoring model works →](strategy/scoring-model.md)
- [Technical SEO status →](technical-seo/completed.md)
