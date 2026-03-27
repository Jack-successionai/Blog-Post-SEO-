---
name: blog-brief
description: >
  Generate a data-driven blog post blueprint from a target keyword. Pulls related
  keywords Google uses for ranking, analyzes competitor content structure, identifies
  questions to answer, and outputs a writing brief with keyword placement KPIs.
  Use when: "blog brief", "write brief", "post blueprint", "what goes in the post",
  "keyword brief for [topic]".
allowed-tools: Bash, Read, Write, Grep, Glob
---

# Blog Post Brief Generator

Takes a target keyword and produces a quantitative writing blueprint — what keywords to include, where to place them, what questions to answer, and what headings to use.

## Context

This is the 5th skill in the pipeline:
```
/keyword-discover → /keyword-score → /keyword-compete → /keyword-recommend → /blog-brief
```

The first 4 skills answer: "WHAT topic should we write about?"
This skill answers: "HOW should we write the post to rank for that topic?"

## Config

- Credentials: `source /Users/jack0518/Blog-Post-SEO-/.env`
- Input: keyword from `/keyword-recommend` output OR user-provided keyword
- Competition data: `/Users/jack0518/Blog-Post-SEO-/research/YYYY-MM-DD/competition.md`
- Output: `/Users/jack0518/Blog-Post-SEO-/content-plan/blog-post-briefs/[keyword-slug].md`

## Steps

### 1. Get the target keyword

From user input (e.g. `/blog-brief "SDE vs EBITDA"`) or from the latest `recommendations.md`.

Check `scored.json` for this keyword's data: volume, KD, CPC, trend, competition.

### 2. Pull related keywords (the ranking fuel)

Google doesn't just look for your exact keyword. It expects a page about "SDE vs EBITDA" to also mention related terms like "seller's discretionary earnings", "valuation multiple", "owner compensation", etc.

**API call:**
```bash
source /Users/jack0518/Blog-Post-SEO-/.env
curl -s -X POST "https://api.dataforseo.com/v3/keywords_data/google_ads/keywords_for_keywords/live" \
  -H "Content-Type: application/json" \
  -u "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" \
  -d '[{"keywords": ["TARGET_KEYWORD"], "location_code": 2840, "language_code": "en", "sort_by": "search_volume"}]'
```

From the results, select the **top 10-15 most relevant** related keywords that:
- Are semantically related to the topic (not tangential)
- Have search_volume >= 10
- Would naturally appear in a comprehensive post about this topic

These are the terms to weave into the blog post naturally.

### 3. Pull "People Also Ask" questions

**API call:**
```bash
curl -s -X POST "https://api.dataforseo.com/v3/serp/google/organic/live/advanced" \
  -H "Content-Type: application/json" \
  -u "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" \
  -d '[{"keyword": "TARGET_KEYWORD", "location_code": 2840, "language_code": "en", "depth": 10}]'
```

Look for items with `type: "people_also_ask"` in the response. Extract the questions. These are:
- Questions Google thinks searchers want answered
- Perfect H2/H3 headings for the blog post
- Ready-made FAQ schema content

If the advanced endpoint doesn't return PAA, use WebSearch to find related questions.

### 4. Analyze competitor content structure

Read `competition.md` from the latest research run for this keyword. For the top 3 competitors:
- What H2 headings do they use?
- What subtopics do they cover?
- What do they MISS that we should add?

If competition.md doesn't have enough detail, call the SERP API and WebFetch the top 3 URLs to analyze their heading structure.

### 5. Design the heading structure

Based on competitor analysis + PAA questions + our differentiating angle, create a heading outline:

```
H1: [Title with primary keyword]
  H2: [Subtopic 1 — definition or context]
  H2: [Subtopic 2 — comparison or analysis]
    H3: [Detail under subtopic 2]
  H2: [Subtopic 3 — practical application for our audience]
  H2: [PAA question as heading]
  H2: [PAA question as heading]
  H2: [Our unique angle — what competitors miss]
  H2: FAQ
    H3: [Question 1]
    H3: [Question 2]
    H3: [Question 3]
```

### 6. Output the brief

Save to `/Users/jack0518/Blog-Post-SEO-/content-plan/blog-post-briefs/[keyword-slug].md`

## Output format

```markdown
# Blog Brief: [Post Title]

**Target keyword:** [exact keyword]
**URL slug:** /blog/[slug]
**Data:** Volume [X]/mo | KD [X] | Trend [X] | CPC $[X]

---

## Keyword Placement Checklist

| Placement | Keyword | Status |
|-----------|---------|--------|
| Title (H1) | Primary keyword, 1x | [ ] |
| URL slug | Primary keyword | [ ] |
| Meta description | Primary keyword, 1x | [ ] |
| First 100 words | Primary keyword, 1x | [ ] |
| H2 headings | Primary keyword, 1-2x | [ ] |
| Body text | Primary keyword, 3-5x per 1,000 words (natural, not stuffed) | [ ] |
| Last paragraph | Primary keyword, 1x | [ ] |
| Image alt text | Primary keyword or variant, 1x | [ ] |

## Related Keywords to Include

These terms tell Google your page comprehensively covers the topic. Weave them in naturally — don't force them.

| Related keyword | Search volume | Use where |
|----------------|---------------|-----------|
| [term 1] | [vol] | Body / heading |
| [term 2] | [vol] | Body |
| ... (10-15 terms) | | |

## Questions to Answer

From Google "People Also Ask" + Reddit threads. Each becomes an H2 or H3 heading + content section. Also use for FAQ schema.

1. [Question 1]
2. [Question 2]
3. [Question 3]
4. [Question 4]
5. [Question 5]

## Heading Structure

```
H1: [Title]
  H2: ...
  H2: ...
    H3: ...
  H2: ...
  H2: FAQ
    H3: [Q1]
    H3: [Q2]
    H3: [Q3]
```

## Competitor Gap

| What top 3 competitors cover | What they miss (our opportunity) |
|------------------------------|--------------------------------|
| [Topic A] | [Gap 1 — what we add] |
| [Topic B] | [Gap 2] |

## Internal Links

| Link TO this post (from existing pages) | Link FROM this post (to existing pages) |
|-----------------------------------------|-----------------------------------------|
| [Existing page 1] → this post | This post → [valuation tool] |
| [Existing page 2] → this post | This post → [related blog post] |

## CTA

Primary CTA: [what action the reader should take — e.g. "Get a free valuation"]
Placement: After the main content, before FAQ section

## KPIs — How to Measure Success

| KPI | Target | Timeline |
|-----|--------|----------|
| Google ranking | Page 1 (top 10) | [X weeks based on KD] |
| Organic traffic | [X visits/mo based on volume × CTR] | After ranking stabilizes |
| Time on page | > 3 minutes | Ongoing |
| Bounce rate | < 60% | Ongoing |
| CTA clicks | > 5% of visitors | Ongoing |
```

## Notes

- The brief is a BLUEPRINT, not the content itself. A writer (human or AI) uses this to write the actual post.
- Related keywords should appear NATURALLY. Never force a term where it doesn't fit.
- The heading structure is a starting point — the writer can adjust while keeping the keyword strategy intact.
- Cost per brief: ~$0.08-0.15 (1 related keywords call + 1 SERP call for PAA). Skip API calls if data already exists from previous pipeline runs.
