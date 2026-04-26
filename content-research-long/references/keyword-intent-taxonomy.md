# Keyword Intent Taxonomy

Framework for classifying search intent, expanding keyword universes, and mapping modifiers to funnel stages and content formats.

## The 4 Primary Intents

| Intent | Description | Keyword Tells | Best Formats | Funnel Stage |
|---|---|---|---|---|
| **Informational** | User wants to learn or understand | "what is", "how to", "why", "guide", "tutorial", "explained" | Pillar guide, glossary, definition post, in-depth tutorial, FAQ | TOFU |
| **Commercial Investigation** | User comparing options before deciding | "best", "vs", "alternative to", "top X", "review", "comparison" | Comparison post, listicle, buyer's guide, case study | MOFU |
| **Transactional** | User ready to act | "buy", "price", "cost", "demo", "free trial", "signup", "discount" | Product page, pricing page, sales-led landing | BOFU |
| **Navigational** | User looking for a specific brand/site | "[Brand]", "[Brand] login", "[Brand] docs" | Brand homepage, docs, knowledge base | Existing audience |

## Sub-Intents (Drill Down for Format Choice)

### Informational Sub-Intents

| Sub-Intent | Keyword Tells | Best Format |
|---|---|---|
| Definition | "what is X", "X meaning", "X definition" | Glossary entry / pillar definition section |
| How-to / procedural | "how to X", "how do I X", "X step by step" | Step-by-step guide with numbered list |
| Why / reasoning | "why X", "why does X happen" | Explainer with cause-effect structure |
| When | "when to X", "when should I X" | Decision-framework guide |
| Where | "where to X" | Location/source guide |
| Conceptual deep-dive | "understanding X", "X explained" | Long-form pillar with multi-layered explanation |
| Troubleshooting | "X not working", "X error", "fix X" | Troubleshooting guide with specific symptoms→fixes |
| Comparison (informational) | "X vs Y" (when both are concepts, not products) | Comparison post explaining trade-offs |

### Commercial Sub-Intents

| Sub-Intent | Keyword Tells | Best Format |
|---|---|---|
| Direct product comparison | "X vs Y" (specific products) | Head-to-head comparison post |
| Alternative search | "alternatives to X", "X alternative" | Alternatives listicle |
| Listicle / ranking | "best X", "top 10 X", "best X for Y" | Listicle with selection criteria |
| Use-case fit | "X for [audience]", "X for [industry]" | Use-case-specific buyer's guide |
| Review / opinion | "X review", "is X worth it" | Review post (first-person) |
| Pricing investigation | "X pricing", "X cost", "is X expensive" | Pricing breakdown post |
| Feature investigation | "does X have Y", "X features" | Feature deep-dive |

### Transactional Sub-Intents

| Sub-Intent | Keyword Tells | Best Format |
|---|---|---|
| Pricing | "X pricing", "X cost" | Pricing page (not blog) |
| Trial / demo | "X demo", "X free trial" | Product page or demo signup |
| Direct purchase | "buy X", "X discount", "X coupon" | Product/checkout page |
| Onboarding | "X setup", "X getting started", "X tutorial" (when product-specific) | Docs / onboarding guide |

### Navigational Sub-Intents

| Sub-Intent | Keyword Tells | Best Format |
|---|---|---|
| Brand homepage | "[Brand]" alone | Homepage |
| Login | "[Brand] login" | Login page |
| Docs | "[Brand] docs", "[Brand] api" | Documentation |
| Support | "[Brand] support", "[Brand] contact" | Support center |

## Modifier Taxonomy

Modifiers reveal funnel stage, sophistication, and intent more reliably than head terms.

| Modifier Type | Examples | Funnel Implication | Sophistication |
|---|---|---|---|
| **Comparative** | vs, alternative to, best, top, compared to | Commercial / MOFU | Mid-to-high |
| **Problem** | problem, error, fix, troubleshoot, broken, not working, issue | Informational / urgent | Varies (often high — they're hands-on) |
| **Educational** | what is, how does, why, when to, guide, tutorial, explained | Informational / TOFU | Often beginner-to-mid |
| **Commercial** | best, top, review, comparison, X for Y | Commercial / MOFU | Mid |
| **Transactional** | price, cost, buy, demo, free trial, signup, discount | Transactional / BOFU | Decision-ready |
| **Temporal** | 2026, latest, current, new, upcoming, this year | Freshness intent | Any |
| **Audience-specific** | for [role], for [industry], for [team size], for [use case] | Segmentation signal | Mid-to-high (knows their context) |
| **Quantitative** | how much, how many, statistics, data, benchmark | Data-seeking | Mid-to-high |
| **Subjective** | best, worst, easy, hard, simple, complex | Opinion-seeking | Beginner-to-mid |

## Query Expansion Sources

| Source | What It Reveals | How to Access |
|---|---|---|
| **Google Autocomplete** | Most common query continuations | Type seed query into Google + add letters a-z |
| **People Also Ask** | Confirmed question demand | SERP features (pull via serp-agent) |
| **Related searches** | Adjacent queries Google associates | Bottom of Google SERP |
| **AlsoAsked** | PAA hierarchy explorer | alsoasked.com |
| **Reddit/Quora question titles** | How real audiences phrase questions | `site:reddit.com [topic] ?` via Exa |
| **Answer The Public** | Question + preposition expansion | answerthepublic.com |
| **YouTube autocomplete** | Video-intent queries | YouTube search bar |
| **Google Trends** | Rising queries (emerging intent) | trends.google.com |
| **Google News** | Topical / current-event variants | news.google.com search |

## Volume Estimation Without Paid Tools

| Proxy | Strength as Volume Signal |
|---|---|
| Autocomplete priority (top 1-3 in Suggest) | Strong — Google ranks Suggest by volume |
| PAA inclusion | Strong — PAA requires meaningful query volume |
| Trends comparison (relative) | Strong for relative volume (compare 2-5 queries) |
| AI Overview presence | Moderate — typically requires meaningful volume |
| Featured snippet presence | Moderate |
| Total SERP result count | Weak — competitive density signal, not volume |
| Reddit/Quora question frequency | Moderate — reveals audience demand, may not match search demand exactly |

Always flag estimates as "directional" without paid keyword tool data.

## Intent Classification Decision Tree

```
Is the query branded?
├── Yes → Navigational (route to brand pages, not blog)
└── No → Continue
        │
        Does the query include transactional modifiers (buy, price, demo)?
        ├── Yes → Transactional (route to product/pricing, not blog)
        └── No → Continue
                │
                Does the query include commercial modifiers (best, vs, alternative)?
                ├── Yes → Commercial Investigation (blog post: comparison/listicle/buyer's guide)
                └── No → Continue
                        │
                        Does the query ask a question (what/how/why/when/where)?
                        ├── Yes → Informational (blog post: guide/tutorial/explainer)
                        └── No → Check SERP — Google's interpretation is ground truth
```

## SERP-Intent Alignment Verification

Always cross-check your classification against what Google shows:

| Your Classification | If SERP is dominated by... | Then... |
|---|---|---|
| Informational | Blog posts, guides | ✓ Aligned |
| Informational | Product pages, pricing | ✗ Misaligned — Google reads as transactional |
| Commercial | Listicles, comparisons | ✓ Aligned |
| Commercial | Reddit, forums | ✗ Misaligned — community/discussion intent |
| Transactional | Product pages, pricing | ✓ Aligned |
| Transactional | Blog explainers | ✗ Misaligned — Google reads as informational |

When misaligned, either reclassify or pick a different target query.

## Anti-Patterns

- **Volume worship** — A 50,000-volume keyword with wrong intent is worse than a 500-volume keyword with right intent.
- **Single-modifier coverage** — Targeting only "best X" misses the entire informational funnel.
- **Ignoring SERP feedback** — If Google disagrees with your classification, Google wins. Always cross-check.
- **No long-tail expansion** — Head terms are competitive and ambiguous. The win is in the long tail.
- **Brand mixing** — Branded queries are navigational; non-branded are informational/commercial. Don't blend in clusters.
- **Modifier blindness** — Two queries with the same head term but different modifiers have different intents. "Code review" (informational TOFU), "code review tools" (commercial MOFU), "best code review tool 2026" (commercial MOFU+), "Codacy pricing" (transactional BOFU) — all different.
