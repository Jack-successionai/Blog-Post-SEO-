# Blog Post Layout Template

Design layout and SEO structure for all Succession AI blog posts. Every post on `successionai.org/blog/` follows this template.

---

## Page Architecture

```
app/blog/[slug]/page.tsx
  |
  |-- metadata export (Next.js Metadata)
  |-- structuredData (JSON-LD: Article + FAQPage)
  |
  |-- <BlogLayout post={post} relatedPosts={[...]}>
  |     |
  |     |-- [StatRow]           (optional, for impact numbers)
  |     |-- TableOfContents     (always present)
  |     |-- Intro paragraphs    (keyword in first 100 words)
  |     |-- [Callout]           (optional, for disclaimers)
  |     |-- <section id="...">  (one per H2, matches TOC)
  |     |     |-- <h2>
  |     |     |-- <h3> (sub-sections)
  |     |     |-- <p>, <ul>, <ol>
  |     |     |-- [Callout]
  |     |     |-- [tables]
  |     |-- BlogCTA             (mid-article conversion)
  |     |-- <section id="faq">  (FAQ for schema)
  |     |-- <section>           (conclusion with keyword)
  |     |-- [BlogCTA]           (optional second CTA)
  |     |
  |     BlogLayout auto-adds:
  |       - Breadcrumb nav (Back to Blog)
  |       - Post header (category, date, read time, title, description)
  |       - Author card (linked to LinkedIn)
  |       - Hero image (2:1 aspect ratio)
  |       - Related posts grid (2 cards)
  |       - Legal disclaimer footer
  </BlogLayout>
```

---

## File Setup

### 1. Blog entry in `lib/blog.ts`

```typescript
{
    slug: 'your-url-slug',
    title: 'Your Post Title With Primary Keyword',
    description: 'Meta description with primary keyword. Under 155 characters for search results.',
    ogDescription: 'Shorter OG description for social sharing. Value-prop focused.',
    publishedDate: '2026-MM-DD',
    readTime: 'X min read',
    category: 'Guide',        // 'Guide' | 'Blog' | 'Resource'
    keywords: [
        'primary keyword',
        'secondary keyword',
        // 4-6 keywords total
    ],
    heroImage: '/images/blog-your-image.jpg',
    heroImageAlt: 'Descriptive alt text with keyword variant',
    featured: true,            // optional, shows on blog index
}
```

### 2. Page file structure

```typescript
import { Metadata } from 'next'
import { getBlogPost } from '@/lib/blog'
import { BlogLayout } from '@/components/blog/blog-layout'
import { TableOfContents } from '@/components/blog/table-of-contents'
import { Callout } from '@/components/blog/callout'
import { BlogCTA } from '@/components/blog/blog-cta'
// Optional:
import { StatRow } from '@/components/blog/stat-row'
import { StepCard, StepConnector } from '@/components/blog/step-card'
import { ComparisonTable } from '@/components/blog/comparison-table'

const post = getBlogPost('your-slug')!

export const metadata: Metadata = { /* ... */ }
const structuredData = [ /* Article + FAQPage schemas */ ]

export default function YourPage() {
    return (
        <>
            {structuredData.map((data, i) => (
                <script key={i} type="application/ld+json"
                    dangerouslySetInnerHTML={{ __html: JSON.stringify(data) }} />
            ))}
            <BlogLayout post={post} relatedPosts={[...]}>
                {/* content */}
            </BlogLayout>
        </>
    )
}
```

---

## Component Library

### TableOfContents
Anchor-linked navigation at the top of every post.

```tsx
<TableOfContents
    items={[
        { id: 'section-id', label: 'Section Label' },
        // One entry per H2 section
    ]}
/>
```

**Design:** Border-top and border-bottom, uppercase "In this article" label, stone-600 links with hover transition. Connects to `<section id="...">` anchors.

### Callout
Info or warning box for key takeaways, disclaimers, or emphasis.

```tsx
<Callout title="Title text">
    <p>Content text here.</p>
</Callout>

<Callout title="Warning title" variant="warning">
    <p>Warning content with amber accent.</p>
</Callout>
```

**Design:** Left border accent (stone-300 for info, amber-500 for warning), subtle background fill. Resets to sans-serif font inside.

### BlogCTA
Dark hero card for conversion. Place 1-2 per post (before FAQ + optionally at end).

```tsx
<BlogCTA
    title="Compelling question?"
    description="Value proposition for clicking."
    href="/valuation"
    buttonText="Action Text"
/>
```

**Design:** Stone-900 background, white text, centered layout, ArrowRight icon on button. Detects external vs internal links.

### StatRow
Three-stat display for opening impact. Use sparingly.

```tsx
<StatRow
    stats={[
        { value: '91%', label: 'of owners have no succession plan' },
        { value: '70%', label: 'of businesses never sell' },
        { value: '$5T+', label: 'in enterprise value at risk' },
    ]}
/>
```

**Design:** 3-column grid (1-col mobile), stone-900 top border on each stat, serif number.

### StepCard + StepConnector
Numbered progression for frameworks or processes.

```tsx
<StepCard number={1} title="Step Title" outcome="What you get from this step.">
    <p>Explanation of this step.</p>
</StepCard>
<StepConnector />
<StepCard number={2} title="Next Step" outcome="Outcome text.">
    <p>Next step explanation.</p>
</StepCard>
```

**Design:** Numbered circle on left, vertical connector line, bordered outcome box.

### ComparisonTable
Before/after comparison. Headers are hardcoded: "Without a Plan" / "With Succession Planning".

```tsx
<ComparisonTable
    rows={[
        { label: 'Row Label', without: 'Bad state', with: 'Good state' },
    ]}
/>
```

**Design:** Red header for "without", green for "with". Stone borders.

### Custom HTML Tables
For any comparison not fitting ComparisonTable, use plain HTML tables:

```tsx
<div data-blog-component className="my-6 overflow-x-auto">
    <table className="w-full text-base border-collapse">
        <thead>
            <tr className="border-b-2 border-stone-900">
                <th className="py-3 pr-4 text-left font-semibold text-stone-900">Header</th>
                <th className="py-3 pl-4 text-right font-semibold text-stone-900">Value</th>
            </tr>
        </thead>
        <tbody>
            <tr className="border-b border-stone-200">
                <td className="py-3 pr-4 text-stone-600">Label</td>
                <td className="py-3 pl-4 text-right text-stone-800">$100,000</td>
            </tr>
        </tbody>
    </table>
</div>
```

### Formula/Code Blocks
For calculations or formulas:

```tsx
<div data-blog-component className="my-6 rounded-lg bg-stone-50 border border-stone-200 px-6 py-5">
    <p className="font-mono text-base text-stone-800 leading-relaxed">
        EBITDA = Net Income<br />
        &nbsp;&nbsp;+ Interest<br />
        &nbsp;&nbsp;+ Taxes<br />
        &nbsp;&nbsp;+ Depreciation<br />
        &nbsp;&nbsp;+ Amortization
    </p>
</div>
```

---

## Design System

### Typography
| Element | Font | Size | Color |
|---------|------|------|-------|
| H1 (title) | Serif (Lora) | 2.75rem desktop, 1.875rem mobile | stone-900 |
| H2 | Serif (Lora) | 1.875rem desktop, 1.5rem mobile | stone-900 |
| H3 | Sans | 1.125rem | stone-800 |
| Body | Serif (Lora) | 1.0625rem (17px) | stone-700 |
| Meta/labels | Sans | text-sm, uppercase tracking-wide | stone-400 |
| Strong | Sans | font-semibold | stone-900 |
| Links | Serif | text-base, underline | stone-900, stone-300 decoration |

### Spacing
- Content max-width: **680px** (editorial readability standard)
- H2 top margin: 2.5rem + 1.75rem padding with border-top separator
- H3 top margin: 1.5rem
- Paragraph bottom margin: 1.125rem
- Line height: **1.8** for body, 1.6 for components
- Section padding: px-6 mobile, px-8 desktop

### Colors
- **Primary:** stone-900 (text, borders, accents)
- **Body text:** stone-700
- **Secondary text:** stone-500, stone-600
- **Muted text:** stone-400 (labels, meta)
- **Backgrounds:** stone-50 (callouts, formulas), stone-900 (CTA)
- **Warning:** amber-500 (callout border), amber-50 (callout bg)
- **Comparison:** red-800 (negative), green-800 (positive)

### Layout
- Hero image: 2:1 aspect ratio, rounded-lg, object-cover
- Related posts: 2-column grid, 3:2 image ratio, hover scale effect
- All tables: overflow-x-auto wrapper for mobile
- `data-blog-component` attribute: resets serif prose to sans inside components

---

## SEO Structure Checklist

Every blog post must include these for search ranking:

### Metadata (Next.js)
- [x] Title with primary keyword
- [x] Meta description with primary keyword, under 155 chars
- [x] Keywords array (4-6 terms)
- [x] Open Graph: title, description, type: 'article', url, publishedTime, image
- [x] Twitter card: summary_large_image
- [x] Canonical URL

### Structured Data (JSON-LD)
- [x] **Article schema:** headline, description, image, datePublished, author, publisher
- [x] **FAQPage schema:** 3 questions minimum (from FAQ section)
- [x] **BreadcrumbList:** Auto-generated by BlogLayout

### Content SEO
- [x] Primary keyword in H1 title
- [x] Primary keyword in URL slug
- [x] Primary keyword in first 100 words
- [x] Primary keyword in 1-2 H2 headings
- [x] Primary keyword 3-5x per 1,000 words (natural, not stuffed)
- [x] Primary keyword in last paragraph
- [x] Primary keyword in image alt text
- [x] Tier 2 keywords (competitor-verified related terms) woven throughout
- [x] Tier 3 keywords (contextual terms) where natural
- [x] Internal links to other pages on the site (2-3 minimum)

### Section Structure for SEO
```
H1: Title with Primary Keyword
  H2: The Short Answer                    (skimmer-friendly, competitors skip this)
  H2: Definition / What Is [Topic]        (core informational intent)
  H2: Practical Criteria / Requirements   (eligibility, rules)
  H2: Worked Example with Numbers         (our differentiator -- shows real $$ impact)
  H2: How-To / Process Steps              (actionable content)
  H2: Common Mistakes / Traps             (anxiety-driven search intent)
  H2: Planning / Preparation              (connects to our service)
  H2: FAQ                                 (PAA questions, schema markup)
  H2: Next Step / CTA                     (conversion, keyword in last paragraph)
```

### Technical
- [x] All sections wrapped in `<section id="...">` matching TOC anchors
- [x] `scroll-margin-top: 5rem` on H2s (from global CSS, prevents header overlap)
- [x] Images use Next.js `<Image>` with proper width/height and alt text
- [x] No text-xs anywhere (minimum is text-sm for helper text)
- [x] No emojis anywhere in content

---

## Content Flow Best Practices

1. **Open with pain/question** -- not a definition. Hook the reader in 2 sentences.
2. **"Short Answer" section** -- give the answer upfront. Competitors make you scroll. We don't.
3. **Worked examples with real numbers** -- this is our biggest differentiator. Show the exact dollar impact.
4. **Tables for comparison** -- visual, scannable, shareable. Google can feature these.
5. **Callouts for key takeaways** -- break up walls of text, highlight critical info.
6. **BlogCTA before FAQ** -- the reader is most engaged after absorbing the content.
7. **FAQ matches Google's "People Also Ask"** -- 3 questions minimum for FAQPage schema.
8. **Last paragraph includes keyword + links to valuation tool** -- SEO + conversion in one.
9. **2 related posts** -- keeps readers on the site, reduces bounce rate.
