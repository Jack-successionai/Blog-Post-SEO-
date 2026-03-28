# Content Playbook

The rulebook for writing blog posts and tool pages. Updated by `/seo-analyze` when learnings warrant a rule change. Read by `/seo-next-post` and during content creation.

Every rule has a source — either data-backed or a starting assumption. Rules without data backing are marked [ASSUMPTION] and should be validated or removed within 60 days.

---

## Post Structure

1. **Title:** Include primary keyword naturally. Use specific numbers or dollar amounts when possible (e.g., "$1.25M Tax Break" not "Tax Break"). [ASSUMPTION]

2. **Lead/standfirst:** 1-2 sentences, italic, in the BlogLayout subtitle. Should create urgency or curiosity. [ASSUMPTION]

3. **1-Minute Overview:** Start every long post (8+ min read) with a quick summary section so readers get immediate value. [ASSUMPTION — based on LCGE post structure]

4. **Worked examples:** Include at least one concrete example with real dollar amounts. Don't say "significant savings" — say "$312,500 in tax savings." [EMERGING — LCGE post validation]

5. **Interactive tool:** If a calculator or tool is relevant, embed it in the post using `data-blog-component` wrapper. Also link to the standalone tool page. [EMERGING]

6. **FAQ section:** 3-5 questions with FAQ schema (JSON-LD). Target "People Also Ask" queries. [ASSUMPTION]

7. **CTA placement:** One mid-post CTA (after key insight) + BlogLayout handles end-of-post tools/posts. [ASSUMPTION]

---

## SEO Checklist (Every Post)

- [ ] Primary keyword in title, H1, first 100 words, meta description
- [ ] 2-3 secondary keywords woven naturally into H2s and body
- [ ] FAQ schema with 3-5 questions
- [ ] Article structured data (JSON-LD)
- [ ] Open Graph + Twitter Card meta
- [ ] Canonical URL set
- [ ] Hero image with descriptive alt text
- [ ] `relatedPosts` — 2 curated posts
- [ ] `relatedTools` — link to valuation widget + any relevant tool pages
- [ ] 1-2 in-body links to existing posts (contextual, not forced)
- [ ] Update 1-2 existing posts to link back to the new post
- [ ] Add entry to `blog.ts`
- [ ] Sitemap auto-includes (verify after build)

---

## Keyword Selection Rules

1. **Target KD < 30 first.** Build domain authority on easy wins before attacking harder keywords. [ASSUMPTION]
2. **Prioritize Canada-specific keywords.** Lower competition, more relevant to our audience. [EMERGING]
3. **Mix funnel stages.** Don't write 5 awareness posts in a row. Alternate between: awareness → valuation → active intent → tax/structure. [ASSUMPTION]
4. **Volume floor: 50+ total (CA+US).** Below this, the keyword isn't worth a dedicated post. [ASSUMPTION]

---

## Content Differentiation

What makes our posts different from competitors:

1. **Founder-focused, not advisor-focused.** We write for the business owner, not their accountant. Plain language, not jargon. [CONFIRMED — core brand positioning]
2. **$500K-$50M range.** We're specific about the business size we serve. Content reflects this. [CONFIRMED — core positioning]
3. **Actionable, not theoretical.** Every post should leave the reader with something they can do today. [ASSUMPTION]
4. **Canadian context.** Tax rules, legal frameworks, and examples are Canadian. US content exists as secondary. [CONFIRMED — market focus]

---

## Format Guidelines

- **Read time:** 6-12 minutes (1500-3000 words). Longer only if the depth is justified. [ASSUMPTION]
- **Tone:** Professional but approachable. Think Financial Times editorial, not academic paper. Serif headings, clean layout. [CONFIRMED — design system]
- **No jargon without explanation.** If using terms like SDE, EBITDA, LCGE — explain them on first use or link to a post that does. [EMERGING — LCGE calculator feedback]

---

## Changelog

| Date | Change | Source |
|------|--------|--------|
| 2026-03-27 | Initial playbook created | Seeded from LCGE post validation + competitor analysis |
