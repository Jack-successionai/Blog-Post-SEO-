---
name: seo-next-post
description: >
  Plan the next blog post using accumulated SEO data, learnings, and the content playbook.
  Reads all persistent files + keyword research data, identifies the best next topic,
  and produces a complete content brief optimized by everything we've learned so far.
  Use when: "plan next post", "what should we write next", "seo next post", "next blog post",
  "content brief", "what content to write".
allowed-tools: Bash, Read, Write, Edit, Grep, Glob, WebSearch, WebFetch
---

# Next Post Planning

Use all accumulated SEO intelligence to plan the optimal next blog post.

## Setup

`$PROJECT_ROOT`: Resolve via `git rev-parse --show-toplevel` at start of every run. All paths below are relative to this root. If any persistent file is missing, fail with a clear message naming the missing file.

## Persistent Files (READ all before starting)

1. **Performance log:** `$PROJECT_ROOT/docs/seo-performance-log.md`
2. **Learnings:** `$PROJECT_ROOT/docs/seo-learnings.md`
3. **Playbook:** `$PROJECT_ROOT/docs/content-playbook.md`
4. **Backlink strategy:** `$PROJECT_ROOT/docs/backlink-strategy.md`
5. **Content strategy:** `$PROJECT_ROOT/content-seo-strategy.md`

## Data Sources

6. **Existing posts:** `$PROJECT_ROOT/landing/lib/blog.ts` — current blog posts with keywords
7. **Keyword research:** Latest `$PROJECT_ROOT/keyword-research/YYYY-MM-DD/` directory:
   - `scored.json` — keyword scores
   - `competition.md` — SERP analysis
   - `recommendations.md` — previous recommendations

## Steps

### 1. Load everything

Read ALL files listed above. You need the full picture:
- What posts exist and what keywords they target (blog.ts)
- What keywords have been researched and scored (keyword-research/)
- What's performing well and what's not (performance log)
- What content patterns work (learnings)
- What rules to follow (playbook)
- What the broader strategy says (content strategy + backlink strategy)

### 2. Identify content gaps

Cross-reference:
- **Scored keywords with no content:** From scored.json, find WRITE NOW / HIGH PRI keywords where content_status = NO CONTENT
- **Unexpected ranking keywords:** From performance log, find queries driving traffic that no post specifically targets — these are expansion opportunities
- **Position 5-15 keywords:** Could a new supporting post help an existing post rank higher? (topical authority)
- **Funnel balance:** Check what stages existing posts cover. If we have 3 awareness posts and 0 tax/structure posts, prioritize tax/structure.
- **Internal link opportunities:** Would this new post create valuable links to/from existing content?

### 3. Apply learnings filter

For each candidate keyword, check against learnings:
- Does it match patterns that work? (e.g., if Canadian tax content ranks fast, prioritize Canadian tax keywords)
- Does it avoid patterns that don't work? (e.g., if generic "business tips" content underperforms, skip those)
- Can we embed a tool? (if tool posts outperform, prioritize keywords where a tool makes sense)

### 4. Apply playbook rules

Check each candidate against the playbook:
- KD < 30? (if playbook says prioritize low KD)
- Volume > 50 total? (minimum threshold)
- Mixed funnel stage from recent posts?
- Canadian-specific available?

### 5. Rank candidates

Score each candidate on:

| Factor | Weight | Source |
|--------|--------|--------|
| Keyword score (from scored.json) | 30% | keyword-research |
| Content gap urgency | 20% | blog.ts + performance |
| Matches winning patterns (learnings) | 20% | seo-learnings.md |
| Internal link potential | 15% | existing posts |
| Tool/asset opportunity | 15% | available tools |

### 6. Select top pick + 2 runners-up

Pick the #1 recommendation and 2 alternatives.

### 7. Generate content brief for #1 pick

```markdown
## Content Brief: [Post Title]

### Target
- **Primary keyword:** [keyword] (Volume: [CA+US], KD: [X])
- **Secondary keywords:** [2-3 keywords to weave in]
- **Target position:** [X] within [Y weeks] (based on KD)
- **Funnel stage:** [Awareness / Valuation / Active Intent / Tax & Structure]

### Why This Post, Why Now
[2-3 sentences explaining why this is the best next post based on data]
- Performance data says: [what the data shows]
- Learnings say: [what patterns this aligns with]
- Gap analysis says: [what coverage this fills]

### Competitive Landscape
- **Who ranks now:** [top 3 competitors + their type]
- **Their weakness:** [what they miss]
- **Our angle:** [how we differentiate]

### Recommended Structure
Based on playbook rules and what's worked:

1. **[H2]** — [what to cover]
2. **[H2]** — [what to cover]
3. ...

### Must-Include Elements
Based on confirmed/emerging learnings:
- [ ] Worked example with real dollar amounts (if applicable)
- [ ] Embedded tool: [which one, or "create new X calculator"]
- [ ] FAQ schema: [3-5 suggested questions]
- [ ] In-body links to: [specific existing posts]
- [ ] relatedPosts: [2 suggestions]
- [ ] relatedTools: [suggestions]

### Estimated Impact
- **Conservative (pos 3-5):** [X monthly visits]
- **Optimistic (pos 1-2):** [X monthly visits]
- **Internal link boost:** This post creates links to [existing posts], potentially improving their rankings

### What We'll Learn
After publishing, these hypotheses will be tested:
- [Hypothesis 1 — e.g., "Canadian regulatory content ranks within 4 weeks"]
- [Hypothesis 2 — e.g., "Posts with calculators get 2x time-on-page"]
```

### 8. Output runners-up

For each of the 2 alternatives, provide:
- Keyword + volume + KD
- One-line reason it's a good pick
- Why it ranked below #1

### 9. Ask for decision

End with:
```
Recommendation: Write "[Post Title]" first.

Alternatives:
1. [Runner-up 1 title] — [one-line reason]
2. [Runner-up 2 title] — [one-line reason]

Which one do you want to go with? Or should I run /keyword-discover first for fresh data?
```

## When No Keyword Research Exists

If there's no `scored.json` or it's older than 14 days:

1. Still read the persistent files
2. Use WebSearch to check what keywords related to existing content are trending
3. Check Google's "People Also Ask" and autocomplete for related queries
4. Provide best-guess recommendations but flag: "Recommendations based on web search — run `/keyword-discover` for data-backed scoring"

## When Performance Data is Sparse

If the performance log only has 1-2 snapshots:

1. Note that learnings are still emerging
2. Weight keyword scores more heavily (70%) since we don't have enough performance data
3. Flag which hypotheses this post will help test
4. Suggest what metrics to watch after publishing

## Important Rules

- **Read ALL files before recommending.** The power of this skill is the accumulated context. Don't skip files.
- **Never recommend a keyword we already cover well.** Check blog.ts carefully.
- **Show your reasoning.** The user should see WHY this is the #1 pick — what data supports it.
- **Connect to the flywheel.** Every recommendation should explain what we'll learn from publishing it.
- **Be honest about data gaps.** If we don't have enough performance data to back a pattern, say so.
- **Respect the playbook.** If it says KD < 30 first, don't recommend a KD 45 keyword unless you have a strong data-backed reason.
