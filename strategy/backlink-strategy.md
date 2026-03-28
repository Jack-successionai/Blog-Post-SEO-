# Backlink Strategy: Widget Partnerships + Internal Linking

## Core Model

Package the existing business valuation widget as a **free embeddable tool** for brokers, accountants, and advisory firms. They get lead generation; we get a tracked "Powered by Succession AI" backlink on every embed.

### Why This Works
- The widget already exists and is production-ready (4 variants: v1/v2, EN/FR)
- One `<script>` tag to embed — zero friction for partners
- Every embed = a dofollow backlink from a relevant domain (financial services, advisory)
- Partners get real value: qualified business-owner leads from the widget submissions
- Lead source is tracked via `?partner=slug` param so we can attribute and report

---

## Widget Partnership Model

### How It Works

1. Partner adds one script tag to their site:
   ```html
   <script src="https://successionai.org/api/widget/embed.js?partner=firmname"></script>
   ```
2. Widget renders with "Powered by Succession AI" footer linking to `successionai.org?ref=widget-firmname`
3. When a visitor completes the valuation form, the submission includes `source: partner_firmname`
4. Partner receives the lead data; we get a permanent backlink

### What Partners Get
- Free business valuation widget for their website
- Qualified leads from business owners considering a sale
- Co-branded or white-label options (future)
- Monthly lead report (future)

### What We Get
- Dofollow backlink from a relevant, authoritative domain
- Referral traffic from business-owner audiences
- Brand visibility in the advisory/broker ecosystem
- Attribution data for each partner's performance

---

## Target Partner Types

| Type | Why They're Good Partners | Volume Potential |
|------|--------------------------|------------------|
| Business brokers | Direct access to sellers; high domain relevance | Medium (dozens in Canada) |
| Accounting firms | Trusted advisors to business owners; strong domains | High (hundreds) |
| Business lawyers | Estate/corporate lawyers advising on exits | Medium |
| Exit planning coaches | Niche audience of business owners planning exits | Low-medium |
| Industry associations | Member directories, resource pages; high DA | Low (but high quality) |
| Financial advisors | Wealth management for business owners | High |

**Priority order:** Business brokers > Accounting firms > Industry associations > Lawyers > Coaches > Financial advisors

---

## Outreach Playbook

### Tier 1 — Small Firms (Cold Outreach via Apollo)

Individual brokers, small accounting practices, solo exit-planning coaches. Owner makes the decision. Cold email can work here because the value prop is immediate and tangible (free leads).

**Identify:** Apollo.io search — Canada, business brokerage / accounting / corporate law, 2-50 employees, Owner/Partner title.

**Qualify:**
- [ ] Has a website (not just a directory listing)
- [ ] Serves business owners who might sell
- [ ] Domain Authority > 15

**Pitch (email):**

> Hi [First Name],
>
> I noticed [firm name] works with business owners looking to sell — we do too, from the technology side.
>
> We built a free business valuation widget that gives owners an instant SDE-based estimate. Several advisory firms embed it on their websites to generate qualified seller leads.
>
> It's one line of code to add, completely free, and the leads go directly to you. Here's a quick demo: [link to widget demo page]
>
> Would you be open to trying it on your site? Happy to set it up for you — takes about 5 minutes.
>
> Best,
> Avril

**Follow-up (3 days later):**

> Quick follow-up on the free valuation widget. Here's what it looks like embedded: [screenshot or demo link]
>
> No cost, no commitment — if the leads aren't useful, you can remove it anytime.

### Tier 2 — Mid-Size Firms (Warm Intro / LinkedIn)

Regional accounting firms, multi-broker shops, wealth management practices. Need to reach a decision-maker, but still relatively flat orgs. LinkedIn warm outreach or intro through shared connections works better than cold email.

**Approach:**
1. Connect on LinkedIn with a short note referencing their work with business owners
2. Share a relevant blog post (LCGE calculator, SDE vs EBITDA) as a value-add — not a pitch
3. After engagement, propose the widget partnership as a mutual-benefit tool
4. Offer a 15-min call to demo the widget and discuss lead sharing

### Tier 3 — Institutions (Relationship / Formal Partnership)

CPA Canada, provincial law societies, BDC, industry associations, large advisory firms. Cold email will NOT work here. These require formal partnership proposals, often through business development or marketing departments.

**Approach:**
1. **Identify the right contact** — partnerships/BD team, not individual practitioners
2. **Build credibility first** — contribute guest content, attend their events, get quoted in their publications
3. **Propose a formal partnership** — position the widget as a member benefit or resource page tool
4. **Package it properly** — one-pager with: what it is, how it helps their members, embed demo, data privacy/security assurance
5. **Be patient** — these take weeks/months, not days

**What to offer institutions:**
- Co-branded widget with their logo
- Aggregate (anonymized) data insights for their members
- Guest blog post or webinar collaboration
- Listed as a "Technology Partner" on our site

**Examples of institutional targets:**
- CPA Canada / provincial CPA bodies (resource pages for members)
- Canadian Federation of Independent Business (CFIB)
- BDC (Business Development Bank of Canada)
- Provincial law society practice resources
- Industry-specific associations (e.g., Canadian Franchise Association)

### Onboarding (All Tiers)

Once they say yes:
1. Send them the embed code with their partner slug: `?partner=firmname`
2. Offer to help install (many will need hand-holding)
3. Confirm the widget renders correctly on their site
4. Verify the backlink is live and dofollow
5. Set up a monthly lead report (manual for now, automate later)

---

## Internal Linking Guidelines

### Principles
- Every blog post should link to 1-2 other posts and 1-2 tools where contextually relevant
- Links should be natural — a reader should want to click, not feel forced
- Use descriptive anchor text (not "click here")
- `relatedPosts` in BlogLayout: curate 2 posts, prioritize newer/higher-value content
- `relatedTools` in BlogLayout: link to `/valuation` and `/tools/lcge-calculator` where relevant

### Current Internal Link Map

| Post | Links To (in-body) | relatedPosts | relatedTools |
|------|-------------------|--------------|--------------|
| SDE vs EBITDA | LCGE blog post | LCGE, Selling Guide | Valuation, LCGE Calculator |
| Selling Your Business | SDE vs EBITDA | LCGE, SDE vs EBITDA | Valuation, LCGE Calculator |
| Succession Planning Guide | — | LCGE, Selling Guide | Valuation |
| Succession Complete Guide | LCGE blog post | LCGE, SDE vs EBITDA | Valuation, LCGE Calculator |
| LCGE Post | (embedded calculator) | SDE vs EBITDA | Valuation |

### When Adding New Posts
1. Add 1-2 in-body links to existing posts
2. Set `relatedPosts` with 2 relevant existing posts
3. Add `relatedTools` with relevant tool links
4. Update 1-2 existing posts to link back to the new post

---

## Metrics to Track

| Metric | Source | Cadence |
|--------|--------|---------|
| Number of partner embeds live | Manual audit | Monthly |
| Referring domains from widget backlinks | Google Search Console | Monthly |
| Referral traffic from widget backlinks | GA4 | Monthly |
| Leads per partner (source field) | Backend/CRM | Monthly |
| Domain Authority trend | Ahrefs/Moz | Quarterly |
| Keyword ranking changes | Google Search Console | Weekly |
| Internal link click-through | GA4 events | Monthly |

---

## Timeline

- **Phase 1-2 (Done):** Partner attribution + tracked backlinks in widget
- **Phase 3 (Done):** Internal linking across all blog posts
- **Phase 4 (Done):** This strategy document
- **Phase 5 (Next):** Tier 1 outreach via Apollo — start with 3-5 small broker/accounting firms
- **Phase 6 (Parallel):** Tier 2/3 relationship building — LinkedIn warm outreach, identify institutional contacts
- **Ongoing:** Add new partners, monitor metrics, iterate on pitch
