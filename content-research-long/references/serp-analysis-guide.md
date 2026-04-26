# SERP Analysis Guide

How to analyze Google SERPs for long-form content research in 2026 — what to extract, how to interpret SERP features, and how to calibrate ranking difficulty.

## What's Different in 2026

Google's SERP has evolved significantly. The blue-link list is no longer the dominant feature for many queries:

- **AI Overview** appears at the top for most informational queries, citing 2-5 sources. Click-through to organic blue links drops 30-60% when AI Overview is present. Being cited *in* AI Overview is often more valuable than ranking #1.
- **Featured snippets** still appear but compete with AI Overview. They're typically held by position 1-5 URLs that match a specific format (paragraph / list / table).
- **People Also Ask** has expanded — typically 4 questions visible, expanding to 10+ as users interact. PAA inclusions drive significant click-through and signal answer-worthiness.
- **Image and video carousels** dominate visual queries.
- **Discussions and forums** modules appear for community-intent queries (often surfacing Reddit and Stack Exchange directly in the SERP).

Always check what SERP features are present before assessing the opportunity.

## Extraction Checklist (Per Query)

For every target query, capture:

1. **Date scraped** (must be ≤14 days old to be considered current)
2. **Geo / language** of the search
3. **AI Overview** presence — yes/no, full text, cited URLs
4. **Featured snippet** — present? Format (paragraph / list / table / video)? Held by which URL? Snippet text?
5. **PAA** — top 4 questions (capture exact wording)
6. **Other SERP features** — image pack, video carousel, news box, shopping, Twitter/X, sitelinks, knowledge panel
7. **Top 10 organic URLs** with: position, URL, domain, title, word count, last-updated date, format type, schema markup, hypothesis on why it ranks
8. **Related searches** at the bottom of the SERP

## Interpreting SERP Features

### AI Overview Strategy

When AI Overview is present:
- **Identify cited URLs** — Google selects for citation-worthiness, not raw ranking. Cited URLs typically have: clear schema, original data, authoritative voice, structured "what / why / how" sections.
- **The cited URLs are your competitive set**, not necessarily the top-ranked ones.
- **AI Overview citations have outsized value** — they appear above all organic results.
- **To earn AI Overview citation:** structured data (Article + FAQPage schema), original research/data points, authority signals (cited expert quotes, About page credibility), specificity (concrete examples, not generalities), freshness (recent dates), clear "answer" sections.

When AI Overview is absent: that's an opportunity — the query may shift to AI Overview later, and being the answer-shaped content positions you to be cited.

### Featured Snippet Strategy

| Snippet Format | Winning Page Pattern |
|---|---|
| **Paragraph** | H2 phrased as the query → 40-60 word answer paragraph immediately following |
| **List (ordered)** | H2 phrased as the query → numbered list of 4-8 items immediately following |
| **List (unordered)** | H2 phrased as the query → bullet list of 4-8 items immediately following |
| **Table** | H2 phrased as the query → HTML table with clear `<th>` headers immediately following |
| **Video** | YouTube video with the query in the title and timestamp answering the question |

To steal a featured snippet:
1. Identify the exact format Google has chosen.
2. Replicate the format precisely on your page (same H2 wording, same structure type).
3. Make your answer cleaner / more authoritative / more specific.
4. Add schema markup (Article, FAQPage, HowTo as applicable).

### People Also Ask Strategy

PAA questions are confirmed search demand. They become H2/H3 in your long-form piece.

To get included in PAA for a question:
1. Phrase an H2 or H3 *exactly* as the PAA question (or very close).
2. Immediately follow with a 40-80 word direct answer.
3. Use FAQPage schema markup.

### Discussions / Forums Module

When Google surfaces a "Discussions and forums" module, it's signaling community intent. A polished blog post may not rank in the main SERP.

Strategies:
- **Rank in the discussions module** by participating substantively in the named forums (Reddit, Stack Exchange).
- **Pivot the query** to a less community-intent variant where blog content can rank.
- **Build the case study or contrarian angle** that earns Reddit AMAs and discussion linkbacks.

## Top-10 URL Analysis

For each top-10 URL, extract:

### Structural Signals
- Word count
- H2 count and pattern
- Sub-H3 density per H2
- TL;DR or summary at top? (Yes/No)
- FAQ section at bottom? (Yes/No)
- Comparison table? (Yes/No)
- Original data / research? (Yes/No)
- Multimedia density (images, videos, charts per 1000 words)
- Internal link count
- External authoritative link count

### Authority Signals
- Domain strength (DR/DA estimate or proxy)
- Author byline + author authority (do they have a wikipedia page, books, recognized credentials?)
- Cited experts (look for in-text "[Name] said" patterns)
- Linked-from sources (use a backlink tool if available, otherwise estimate from domain prominence)

### Freshness Signals
- Last-updated date (from meta or visible byline)
- Date in URL (often a strong signal for "[topic] 2026" queries)
- Recent commentary on emerging events

### Schema Signals
- View page source for `application/ld+json` blocks
- Identify schema types present: Article, FAQPage, HowTo, Comparison, Review, BreadcrumbList, etc.
- Schema is part of why URLs win SERP features — always check.

## "Why It Ranks" Hypothesis Framework

Bad: "It ranks because it's well-written."
Good: "It ranks because of: (1) authority — the domain is DR 85 and the author is a recognized industry voice; (2) depth — 4500 words covering all 12 PAA questions; (3) freshness — updated 30 days ago with current 2026 references; (4) schema — Article + FAQPage."

Always cite 2-3 specific signals. Vague hypotheses fail the critic gate.

## Ranking Difficulty Calibration

| Top-10 Profile | Difficulty for DR 30 | Difficulty for DR 60 | Difficulty for DR 80+ |
|---|---|---|---|
| All top-10 DR 80+ news/major SaaS | Out of reach | Hard | Medium |
| Mixed DR 50-90 | Hard | Medium | Easy |
| Mixed DR 30-70 | Medium | Easy | Easy |
| Mostly DR <50 | Easy | Easy | Easy |

When out of reach: pivot to long-tail flank queries (more specific modifiers, narrower audience, less competitive).

## Tool Stack

| Tool | Use For |
|---|---|
| WebSearch | Direct Google SERP queries |
| Firecrawl scrape/extract | Extracting H2 outline, word count, schema from top-ranking URLs |
| Tavily extract / research | Alternative content extraction; multi-step ranking research |
| Exa web_search | Semantic search when SERP results are sparse |
| WebFetch | Direct URL access for verification |
| Free domain check | DR/DA proxies — Ahrefs/SEMrush free tools, Moz free, Open PageRank |

## Anti-Patterns

- **Single-URL hypothesizing** — Patterns require ≥5 of top-10 sharing them.
- **Word-count fetishism** — Match depth to intent, not to a number.
- **Ignoring AI Overview** — In 2026, this is often the most valuable SERP feature.
- **Stale data** — SERPs change daily. Always include scrape date.
- **Schema blindness** — Many SERP features are schema-driven. Always check.
- **Domain-blind difficulty assessment** — Difficulty depends on the user's domain strength relative to top-10.
