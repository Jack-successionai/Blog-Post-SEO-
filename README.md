# Blog Post SEO — Succession AI

Source of truth for Succession AI's data-driven SEO content strategy. Everything here drives what blog posts we write, why, and in what order.

## How this works

```
1. DISCOVER    →  2. SCORE    →  3. COMPETE    →  4. RECOMMEND    →  5. BRIEF    →  6. WRITE
   Find what        Rank by        Check who         Pick the          Generate       Publish
   people           volume,        ranks and         top 5 and         keyword        blog
   search for       difficulty,    what Reddit       produce           placement      posts
                    trend          says              titles/angles     blueprint
```

Skills 1-4 answer: **WHAT topic should we write about?**
Skill 5 answers: **HOW should we write the post to rank for that topic?**

We use **DataForSEO API** (Google Ads data) to get real search volume, keyword difficulty, SERP competition, and related keyword data. Results are stored in `/research/` by date. All skills live in `.claude/skills/`.

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
│   └── blog-post-briefs/        ← Individual keyword blueprints per post
│       └── sde-vs-ebitda.md     ← First brief (ready to write)
├── technical-seo/
│   └── completed.md             ← Technical SEO work done
└── .claude/skills/              ← The 5 Claude Code skills
    ├── keyword-discover/        ← Expand seeds into keyword lists
    ├── keyword-score/           ← Score by volume, KD, trend, intent
    ├── keyword-compete/         ← SERP + Reddit competitive analysis
    ├── keyword-recommend/       ← "Write these 5 next"
    └── blog-brief/              ← Keyword blueprint for each post
```

## Quick Links

- [What to write next →](content-plan/publishing-order.md)
- [First blog brief (SDE vs EBITDA) →](content-plan/blog-post-briefs/sde-vs-ebitda.md)
- [Latest keyword research →](research/2026-03-26/recommendations.md)
- [How our scoring model works →](strategy/scoring-model.md)
- [Content pipeline (5 skills) →](strategy/content-pipeline.md)
- [Technical SEO status →](technical-seo/completed.md)
