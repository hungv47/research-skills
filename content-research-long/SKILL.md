---
name: content-research-long
description: "Researches what long-form content to create — SERP analysis, keyword intent clustering, topic cluster mapping, source/expert discovery, and deep audience listening. Produces `.agents/mkt/content-research-long.md` for blog, case study, byline, press release, newsletter, and app store/G2 listings. Not for short-form/social/ad research (use content-research). Not for landing pages (use lp-optimization). For writing the long-form content itself, see content-long."
argument-hint: "[topic, target keyword, or niche]"
allowed-tools: Read Grep Glob Bash WebSearch WebFetch
license: MIT
metadata:
  author: hungv47
  version: "0.1.0"
  budget: deep
  estimated-cost: "$1-3"
promptSignals:
  phrases:
    - "what blog should"
    - "what to write about"
    - "SEO content research"
    - "keyword research"
    - "SERP analysis"
    - "topic cluster"
    - "content gap analysis"
    - "what ranks for"
    - "search intent"
    - "byline pitch research"
    - "expert sources for"
    - "press research"
    - "long-form content research"
    - "newsletter topics"
  allOf:
    - [SEO, content]
    - [keyword, research]
    - [long-form, research]
  anyOf:
    - "SERP"
    - "search intent"
    - "topic cluster"
    - "evergreen"
    - "byline"
    - "guest post"
    - "press release research"
    - "long-form"
    - "blog research"
    - "ranking analysis"
  noneOf:
    - "competitor ads"
    - "ad library"
    - "viral"
    - "TikTok"
    - "Reels"
    - "scroll-stop"
  minScore: 6
routing:
  intent-tags:
    - long-form-research
    - serp-analysis
    - keyword-research
    - search-intent
    - topic-clusters
    - source-discovery
    - byline-research
    - press-research
    - evergreen-content
    - content-hub-design
  position: pipeline
  produces:
    - mkt/content-research-long.md
  consumes:
    - product-context.md
    - icp-research.md
    - market-research.md
  requires: []
  defers-to:
    - skill: content-research
      when: "research target is short-form, social, or ad creative — competitor ads, viral hooks, scroll-stop content"
    - skill: market-research
      when: "need strategic market landscape, not content-level intelligence"
    - skill: icp-research
      when: "need full persona profiles and buyer psychology, not real-time audience language"
    - skill: seo
      when: "need technical SEO audit of existing pages, not pre-creation content gaps"
    - skill: content-long
      when: "ready to write the long-form content — research is done"
  parallel-with: []
  interactive: false
  estimated-complexity: heavy
---

# Content Research (Long-Form) — Orchestrator

*Research — Pre-creation intelligence for long-form content. Discovers what to write based on what ranks, what intent searchers have, who the cited authorities are, and what depth top-performing pieces hit.*

**Core Question:** "What long-form content should we create — what queries, what intent, what depth, what angle, what experts to cite — based on what's ranking and what the audience actually reads?"

## When to use this vs. `content-research`

| If your target is... | Use |
|---|---|
| Blog post, case study, byline, press release, newsletter, app store/G2 listing, in-depth Quora answer, evergreen guide | **content-research-long** (this skill) |
| TikTok/Reels script, ad creative, social post, thread, carousel, SMS, OOH, drip email | **content-research** |

The two skills share the same upstream artifacts (`product-context.md`, `icp-research.md`) and feed two different downstream skills (`content-long` vs. `content-short`). Run both if a campaign spans both formats — they cover different research surfaces and don't duplicate each other.

## Critical Gates — Read First

1. **Research informs — it does not decide.** Output is evidence and patterns, not a publishing calendar. The user decides what to act on.
2. **SERP recency matters.** Google's index updates daily. Ranking analysis older than 14 days should be re-validated. Featured snippets and AI Overview placements churn faster than blue-link rankings.
3. **Rank position = composite signal.** A URL ranking #1 for a target query is the result of relevance + authority + freshness + UX + intent match. Don't reverse-engineer "the secret" from a single ranking — look for patterns across the top 10.
4. **Intent before keyword.** A 50,000-volume keyword with the wrong intent (informational when you need transactional) is worse than a 500-volume keyword with the right intent. Intent classification is non-negotiable.
5. **Authority compounds, not posts.** Long-form research must identify the *cited authorities* (experts, journalists, original researchers) for the topic — not just "what's ranking." Citing them well is how you earn links and credibility, not just clicks.
6. **Use live data, not training data.** Do NOT rely on memorized SERPs, keyword volumes, or expert lists. Always use WebSearch, WebFetch, Exa, Tavily, or other live research tools.

## Philosophy

Long-form content is a different game from short-form. Short-form competes for attention in a feed; long-form competes for ranking, dwell time, citations, and subscriptions. The research that informs each is different: short-form needs to know what stops scrolls; long-form needs to know what searchers want, how deep top results go, and which experts the audience trusts.

This skill produces the inputs `content-long` needs: target keywords with verified intent, structural patterns from top-ranked pieces, expert sources to cite or interview, and audience language pulled from substantive (not punchy) discussion.

**Intent > Volume > Difficulty.**

## Inputs Required

- Topic, target keyword, or niche (required)
- Target audience / ICP role (recommended — pulls from `research/icp-research.md` if available)
- Domain authority context (your site's DR, existing topical coverage) (optional but useful for difficulty calibration)
- Geographic/language scope if not US English (optional)

## Output

- `.agents/mkt/content-research-long.md`

## Chain Position

Previous: `icp-research`, `market-research` (optional but recommended) | Next: `content-long`, `seo`, `imc-plan`

### Skill Deference

- **Researching long-form (blog, case study, byline, PR, newsletter, listing)?** → Use this skill.
- **Researching short-form (social, ads, threads, Reels)?** → Use `content-research` instead.
- **Need to understand the overall MARKET dynamics?** → Use `market-research` instead.
- **Need to understand WHO the customer is?** → Use `icp-research` instead.
- **Ready to WRITE the long-form content?** → Use `content-long` instead.
- **Need to audit EXISTING pages for technical SEO?** → Use `seo` instead — that audits live pages; this researches what to create.

**Re-run triggers:** Before any long-form content sprint, before entering a new SEO topic cluster, quarterly for active content programs, after major Google algorithm updates, when a competitor publishes a high-ranking pillar piece in your space.

---

## Agent Manifest

7 agents across 2 layers:

| Agent | Layer | Role | Input | Output |
|-------|-------|------|-------|--------|
| [serp-agent](agents/serp-agent.md) | L1 (parallel) | Analyzes SERPs for target queries — what ranks, depth, structure, age, snippet/AI Overview presence | brief + target queries | SERP Landscape section |
| [keyword-intent-agent](agents/keyword-intent-agent.md) | L1 (parallel) | Clusters target queries by intent (informational/commercial/transactional/navigational), maps long-tail variants and PAA | brief + seed terms | Keyword & Intent Map section |
| [source-discovery-agent](agents/source-discovery-agent.md) | L1 (parallel) | Identifies cited authorities, journalists, podcast hosts, Substack/newsletter writers, and original researchers covering the topic | brief + niche | Authority & Source Map section |
| [deep-listening-agent](agents/deep-listening-agent.md) | L1 (parallel) | Mines long-form audience discussion (Q&A sites, in-depth Reddit/HN threads, Substack comments, podcast Q&A) for substantive language and dwell-time intent | brief + niche | Deep Audience Listening section |
| [pattern-agent](agents/pattern-agent.md) | L2 (sequential) | Synthesizes L1 outputs into winning long-form patterns, topic clusters, and content opportunities | merged L1 outputs | Patterns, Clusters & Opportunities section |
| [brief-agent](agents/brief-agent.md) | L2 (sequential) | Converts opportunities into long-form briefs (target keyword, intent, outline, word-count target, internal/external links, expert quotes, distribution) | pattern-agent output + L1 context | Long-Form Brief Recommendations section |
| [critic-agent](agents/critic-agent.md) | L2 (sequential) | Validates SERP freshness, intent classification, source credibility, signal-vs-noise, brief actionability | full merged artifact | PASS or FAIL with rewrite instructions |

---

## Routing Logic

### Route A: SERP-Only Analysis

**Trigger:** "Analyze the SERP for [query]", "What ranks for [keyword]?", "Who's at #1 for [topic] and why?"

```
serp-agent → pattern-agent → critic-agent
```

Skip keyword-intent, source-discovery, deep-listening. Single-query deep dive on what ranks and why.

### Route B: Keyword & Intent Research

**Trigger:** "Keyword research for [topic]", "Find long-tail keywords for [niche]", "What's the search intent around [topic]?"

```
keyword-intent-agent → pattern-agent → critic-agent
```

Focus on query universe + intent classification. No SERP teardown, no source discovery.

### Route C: Authority & Source Discovery

**Trigger:** "Who are the experts on [topic]?", "Find byline targets for [niche]", "Who covers [beat] in tech press?", "Source list for a [topic] piece"

```
source-discovery-agent → pattern-agent → critic-agent
```

For PR, byline pitches, expert interviews, and citation strategy. No SERP, no keyword research.

### Route D: Deep Audience Listening

**Trigger:** "What are people actually saying about [topic] in long-form?", "Quora/Stack Exchange research for [niche]", "Substack reader sentiment on [topic]"

```
deep-listening-agent → pattern-agent → critic-agent
```

Long-form audience language for narrative depth — distinct from `content-research`'s scroll-skim listening.

### Route E: Full Long-Form Research Brief

**Trigger:** "Full long-form research for [topic]", "Research before our blog/newsletter sprint", "What should we write?", "Topic cluster for [pillar]"

```
serp-agent ──────────────┐
keyword-intent-agent ────┤
source-discovery-agent ──┼──→ pattern-agent → brief-agent → critic-agent
deep-listening-agent ────┘
```

All 4 L1 agents run in parallel. Full L2 sequence produces actionable long-form briefs.

---

## Scope Calibration

| Decision Context | Research Depth | Queries Analyzed | Top URLs per Query |
|---|---|---|---|
| Quick check (single piece) | 1-2 agents, surface research | 1-3 queries | Top 5 URLs |
| Pillar piece prep | 3-4 agents, moderate depth | 5-15 queries (cluster) | Top 10 URLs |
| Topic cluster sprint | All agents, comprehensive | 20-50 queries (pillar + clusters) | Top 10 + featured snippet + PAA |

---

## Dispatch Protocol

### Step 0: Context Gathering

Check for upstream artifacts in this priority order:

1. **`research/product-context.md`** — Product category, differentiator, target audience (created by `icp-research`)
2. **`research/icp-research.md`** — Full persona profiles, VoC data, audience habitats
3. **`research/market-research.md`** *(optional)* — Competitor list and positioning for competitive ranking analysis
4. **`.agents/mkt/imc-plan.md`** *(optional bonus)* — If long-form is part of an existing channel plan, scope research to those topic areas

If none of the required artifacts (1-2) exist: interview for the minimum viable context:
1. **What's the topic, keyword, or niche?** — A pillar topic OR a specific target keyword
2. **Who's the audience?** — Buyer role, sophistication level, what they already know
3. **What's the goal?** — Organic search ranking? Earned media? Newsletter subscriptions? Sales?
4. **Domain context (optional but useful)** — Your site's authority, what topics you've already published, gaps you're aware of

### Step 1: Route Selection

Based on the user's request and available context, select Route A-E.

If the user's request is vague ("research long-form for us"), default to **Route E (Full Long-Form Research Brief)**.

### Single-Agent Fallback

If the task is narrow (e.g., "just analyze the SERP for X"), dispatch only the relevant agent without the full pipeline. Skip pattern-agent, brief-agent, and critic. Return the single agent's output directly.

---

## Layer 1 Dispatch — Parallel Research Agents

Build the pre-writing context from Step 0:

```
pre-writing = {
  topic: [pillar topic / niche],
  target_queries: [seed keywords or queries — discovered if not provided],
  audience: [ICP role + sophistication],
  goal: [search ranking / earned media / subscriptions / sales],
  product_context: [from product-context.md or interview],
  icp_data: [from icp-research.md or null],
  domain_context: [your domain authority, existing coverage] | null,
  geo_lang: [US English | other] default US English
}
```

**Dispatch applicable L1 agents in parallel:**

| Agent | Route A | Route B | Route C | Route D | Route E |
|---|---|---|---|---|---|
| serp-agent | Yes | — | — | — | Yes |
| keyword-intent-agent | — | Yes | — | — | Yes |
| source-discovery-agent | — | — | Yes | — | Yes |
| deep-listening-agent | — | — | — | Yes | Yes |

### Research Tool Priority (all L1 agents)

1. **Exa MCP** (`mcp__exa__web_search_exa`) — best for semantic search, finding similar in-depth content, and source discovery
2. **Tavily MCP** (`mcp__tavily-remote-mcp__tavily_search`, `tavily_extract`, `tavily_research`) — structured search with content extraction; tavily_research handles multi-step ranking analysis well
3. **Firecrawl MCP** (`mcp__firecrawl__firecrawl_scrape`, `firecrawl_extract`) — for extracting structured content (H2 outlines, word counts, citations) from top-ranking URLs
4. **WebSearch** / **WebFetch** — always available as fallback
5. **Platform-specific access:**
   - Google SERP: query directly via WebSearch + extract via Firecrawl/Tavily
   - People Also Ask: scraped from SERP via WebFetch on Google search URL
   - AI Overview: appears at top of Google SERP for matching queries — extract presence + cited URLs
   - Quora: `site:quora.com [topic]` via Exa/Tavily
   - Stack Exchange: site-specific (`site:stackoverflow.com`, `site:stackexchange.com`)
   - Substack: `site:substack.com [topic]` for newsletter discovery
   - Reddit (long threads): filter for >500 word top-level posts via Exa
   - Podcast directories: Listen Notes, Apple Podcasts, Podchaser

### Research Checkpoint

After L1 agents return, present findings summary and gather feedback before L2:

**"Here's what I found across SERP / intent / sources / audience. Before I synthesize patterns and build briefs:"**
1. **Any target keywords or queries I missed?**
2. **Any expert sources or publications I should add or remove?** Any blocked competitors or outlets?
3. **Anything surprising in the SERP that we should dig deeper on?** (e.g., AI Overview dominance, unexpected #1, featured snippet held by an outlier)
4. **Do you have internal performance data?** Top-ranking URLs on your domain, posts that earned links, newsletter open rates by topic — dramatically improves pattern identification.

Incorporate feedback into the merged L1 output before dispatching L2.

---

## Layer 2 Dispatch — Sequential Synthesis

### Step 1: Pattern Agent

```
dispatch pattern-agent:
  upstream: [merged output from all L1 agents]
```

Identifies cross-cutting patterns: structural patterns from top-ranked URLs, intent clusters, topic-cluster shape (pillar + spokes), citation patterns (which experts the top URLs cite), content gaps competitors haven't covered, and AI Overview/featured snippet opportunities.

### Step 2: Brief Agent (Route E only)

```
dispatch brief-agent:
  upstream: [pattern-agent output + merged L1 context]
  references: [references/long-form-brief-template.md]
```

Converts opportunities into 3-5 actionable long-form briefs, each specifying: target keyword, search intent, working title, lede approach, outline (H2 + key sub-points), word-count target, internal/external link targets, expert quotes to source, schema markup, distribution channels, and competitive positioning.

### Step 3: Critic Agent

```
dispatch critic-agent:
  upstream: [full merged artifact — all L1 + L2 outputs]
```

Evaluates against quality gate. Returns PASS or FAIL.

---

## Critic Gate — Max 2 Cycles

```
cycle = 0
while cycle < 2:
  verdict = critic-agent.evaluate(artifact)
  if verdict == PASS:
    break
  else:
    for each failure:
      re-dispatch named agent with feedback
    merge fixes into artifact
    cycle += 1

if cycle == 2 and verdict == FAIL:
  deliver artifact with critic's remaining notes as "[REVIEWER NOTE]" annotations
  warn user: "Artifact delivered with quality notes — some items could not be resolved in 2 cycles."
```

**On rewrite:** Only re-dispatch agents the critic names. Do not re-run the entire pipeline.

---

## Quality Gate

Before delivering, verify the merged artifact passes all checks:

- [ ] Every SERP finding cites a query, top URLs, date scraped (≤14 days old or staleness flagged)
- [ ] Every keyword has an intent classification (informational / commercial / transactional / navigational) with evidence
- [ ] Topic cluster (if Route E) has a designated pillar + ≥3 cluster topics with internal-link logic
- [ ] Source/authority discovery names ≥5 cited experts with affiliation, recent work, contact-discovery hint, and *why* they're authoritative on this topic
- [ ] Deep listening includes ≥3 substantive verbatim quotes (≥30 words each) with source link and engagement signal
- [ ] Every long-form brief includes: target keyword, intent, working title, outline (≥4 H2s), word-count target, ≥3 internal-link targets (or "no existing coverage to link to" flagged), ≥2 external authoritative sources to cite
- [ ] Featured snippet / AI Overview opportunities flagged where present
- [ ] Difficulty calibration acknowledges domain authority (no "easy keyword" without confirming DR ≥ that of #5 ranker)
- [ ] Pattern identification distinguishes correlation from causation
- [ ] Findings tagged by audience segment if multiple ICPs apply
- [ ] No keyword/SERP data presented as current without timestamp

---

## Artifact Template

On re-run: rename existing artifact to `content-research-long.v[N].md` and create new with incremented version.

```markdown
---
skill: content-research-long
version: 1
date: {{today}}
status: draft
---

# Content Research (Long-Form): [Topic / Pillar]

**Date:** [today]
**Skill:** content-research-long
**Route:** [A / B / C / D / E]
**Topic:** [pillar topic or niche]
**Target queries analyzed:** [list]
**Audience:** [ICP role + sophistication]

## SERP Landscape

### Per-Query SERP Analysis

For each target query:

#### Query: "[exact query]"

| Position | URL | Domain | Title | Word Count | Last Updated | Format | Schema | Backlink Signal | Why It Ranks |
|---|---|---|---|---|---|---|---|---|---|
| 1 | [url] | [domain] | [title] | [word count] | [date] | [guide/listicle/comparison/case study/...] | [present types] | [DR / referring domains if known] | [Hypothesis] |

- **Featured snippet held by:** [url + snippet text] | None
- **AI Overview present:** Yes / No — if Yes, cited URLs: [list]
- **People Also Ask:** [4 questions]
- **Related searches:** [list]
- **SERP feature density:** [shopping / video carousel / Twitter / news / images count]

### Cross-Query Structural Patterns

| Pattern | Frequency (out of N queries) | Example URLs | Implication |
|---|---|---|---|
| [e.g., "All top-3 pieces use a TL;DR + 8-12 H2 structure"] | [count] | [2-3 examples] | [What this means for our piece] |

### Ranking Difficulty Calibration

| Query | Top-10 Avg Domain Strength | Median Word Count | Median Backlinks | Verdict |
|---|---|---|---|---|
| [query] | [DR estimate] | [count] | [count] | Easy / Medium / Hard / Out of reach |

## Keyword & Intent Map

### Seed Query Universe

| Query | Volume (est) | Intent | Difficulty | SERP Type | Notes |
|---|---|---|---|---|---|
| [query] | [#] | Informational / Commercial / Transactional / Navigational | L/M/H | Blue links / SERP feature dominated / Mixed | [Modifier? Long-tail? Brand?] |

### Intent Clusters

| Cluster | Sample Queries | Best Format | Example Funnel Stage |
|---|---|---|---|
| [e.g., "Definition / What is X"] | [3-5 queries] | [Glossary / pillar guide] | [TOFU] |
| [e.g., "Comparison / X vs Y"] | [3-5 queries] | [Comparison post] | [MOFU] |
| [e.g., "Best X for Y use case"] | [3-5 queries] | [Listicle / buyer's guide] | [MOFU/BOFU] |

### Long-Tail & Question Variants

| Question / Long-tail | Volume | Source | Why It Matters |
|---|---|---|---|
| [from PAA / autocomplete / AlsoAsked] | [#] | [PAA / Reddit / Quora / Suggest] | [Intent insight] |

### Modifier Map

| Modifier Type | Examples in this niche | Volume Signal |
|---|---|---|
| Comparative (vs, alternative, best) | [list] | [counts] |
| Problem (problem, error, fix, troubleshoot) | [list] | [counts] |
| Educational (what is, how does, why) | [list] | [counts] |
| Commercial (best, top, review) | [list] | [counts] |
| Transactional (price, buy, demo, free trial) | [list] | [counts] |

## Authority & Source Map

### Cited Experts

| Name | Affiliation | Why Authoritative | Recent Work | Where to Reach | Citation Frequency in SERP |
|---|---|---|---|---|---|
| [Name] | [Org / role] | [Original research / books / cited methodology / first-mover] | [Title + date + URL] | [Twitter / Substack / email pattern] | [Cited in N of top-10 URLs] |

### Journalists & Outlets

| Outlet | Beat | Journalist(s) covering this topic | Pitch fit | Submission path |
|---|---|---|---|---|
| [Outlet] | [Beat] | [Names + recent bylines] | [Why our angle fits their beat] | [Tipline / pitch portal / direct email] |

### Newsletter & Substack Operators

| Newsletter | Audience | Niche fit | Subs (est) | Cross-promo angle |
|---|---|---|---|---|
| [Name + URL] | [Who reads] | [Why our topic fits] | [Subscriber estimate if public] | [Guest post / sponsorship / mutual mention] |

### Podcast Hosts

| Show | Host | Audience | Topical fit | Booking path |
|---|---|---|---|---|
| [Show] | [Host name] | [Who listens] | [Why our angle fits] | [Booking form / agent / DM] |

### Original Research / Cited Datasets

| Source | What it covers | When published | Likely link magnet | Where most-cited |
|---|---|---|---|---|
| [Report / dataset] | [Scope] | [Date] | [Yes/No — would citing it earn us credibility?] | [Where it's referenced most] |

## Deep Audience Listening

### Substantive Discussions

| Source | URL | Context | Verbatim Quote (≥30 words) | Engagement Signal | Theme |
|---|---|---|---|---|---|
| [Quora / Reddit / Stack Exchange / Substack comment / Podcast transcript] | [link] | [thread title + date] | "[Long quote that captures the actual reasoning, not a one-liner]" | [Upvotes / replies / shares] | [What this reveals] |

### Long-Form Language Map

| Concept (Brand Language) | Audience Long-Form Language | Source | Usage Context |
|---|---|---|---|
| [How we say it] | [How they explain it in 2-3 sentences] | [Where found] | [What kind of post / discussion] |

### Decision Narratives

| Narrative Pattern | Example | Source | Implication for Long-Form |
|---|---|---|---|
| [e.g., "Started with X, switched to Y after Z incident"] | [Specific story] | [Link] | [Use as case-study seed / testimonial mining] |

### Recurring Counter-Arguments

| Counter-Argument | Evidence | How Often It Appears | How Top-Ranking Pieces Address It |
|---|---|---|---|
| [Objection raised in long discussions] | [Quote / link] | [Frequency] | [Coverage gap or strong handling pattern] |

## Patterns, Clusters & Opportunities

### Winning Long-Form Patterns

| # | Pattern | Evidence | SERP Position Correlation | Confidence |
|---|---|---|---|---|
| 1 | [Pattern description — structural, citational, narrative] | [How many top URLs share this] | [Pattern present in top-3 vs top-10] | H/M/L |

### Topic Cluster Architecture (Route E)

```
PILLAR: [Pillar topic — broad, high-volume, head term]
├── CLUSTER 1: [Specific sub-topic] — [target keyword] — [intent]
├── CLUSTER 2: [Specific sub-topic] — [target keyword] — [intent]
├── CLUSTER 3: [Specific sub-topic] — [target keyword] — [intent]
├── CLUSTER 4: [Specific sub-topic] — [target keyword] — [intent]
└── CLUSTER 5: [Specific sub-topic] — [target keyword] — [intent]

Internal-link logic:
- Pillar links DOWN to all clusters
- Each cluster links UP to pillar + LATERALLY to 1-2 sibling clusters
- All cite [Authority N] and [Authority M] from Source Map
```

### Long-Form Opportunities (ranked by evidence strength)

| # | Opportunity | Type | Evidence | Target Query | Format | Priority |
|---|---|---|---|---|---|---|
| 1 | [What to write] | Gap / Featured snippet steal / AI Overview citation / Topic cluster expansion / Authority byline / Newsletter feature | [Supporting data] | [keyword] | [Pillar guide / case study / comparison / byline / press release / newsletter] | High / Med / Low |

### SERP Feature Opportunities

| Feature | Query | Current Holder | Steal Difficulty | Tactic |
|---|---|---|---|---|
| [Featured snippet / AI Overview citation / PAA inclusion / Image pack] | [query] | [URL] | L/M/H | [Specific action — answer-first paragraph, schema, image alt text, etc.] |

### Anti-Patterns to Avoid

| Pattern | Why It Fails | Evidence |
|---|---|---|
| [e.g., "Generic listicle for high-DR competitive query"] | [Won't outrank, won't earn links] | [Top-3 are all original research / authority pieces] |

## Long-Form Brief Recommendations (Route E)

### Brief 1: [Working Title]

- **Target keyword:** [primary keyword] (volume: [#], intent: [type], difficulty: [L/M/H])
- **Secondary keywords / PAA:** [list — to be covered as H2s/H3s]
- **Search intent:** [Informational / Commercial / Transactional / Navigational + sub-intent: definition / comparison / how-to / etc.]
- **Working title:** [Title with primary keyword + curiosity hook]
- **Lede approach:** [Stat / Story / Contrarian claim / Definition / Question] — based on [Pattern #N]
- **Outline (H2s):**
  1. [H2 — addresses sub-intent or PAA]
  2. [H2]
  3. [H2]
  4. [H2]
  5. [H2 — typically counter-argument or "common mistakes"]
  6. [H2 — synthesis / next steps]
- **Word-count target:** [Median word count of top-5 ± 20%]
- **Internal-link targets:** [3-5 URLs on our domain | "no existing coverage" flagged + cluster suggestion]
- **External authoritative sources to cite:** [3-5 from Source Map — name + URL + why this source]
- **Expert quotes to source:** [Names + outreach plan from Source Map]
- **Audience language to use:** [3-5 phrases from Deep Listening with source]
- **Audience language to avoid:** [Brand jargon the audience doesn't use]
- **Schema markup:** [Article / HowTo / FAQPage / Comparison — based on intent]
- **SERP feature target:** [Featured snippet via answer-first paragraph / AI Overview citation via structured data / PAA inclusion]
- **Distribution:** [Newsletter feature / LinkedIn share / Reddit AMA / Podcast pitch / Byline cross-post]
- **Competitive positioning:** [What top URLs cover well / what they miss / our angle]
- **Priority:** [High / Med / Low] — [Justification based on evidence count + opportunity strength]
- **Suggested execution:** `content-long` Route [TBD] → Format: [pillar guide / case study / byline / etc.]
- **Tracking:** [Suggested UTM + GSC monitoring queries + earned-link target count]

### Brief 2: [Working Title]
[Same structure]

### Brief 3: [Working Title]
[Same structure]

### Brief Prioritization Summary

| # | Brief | Priority | Evidence Signals | Est. Effort | Expected Impact |
|---|---|---|---|---|---|
| 1 | [Title] | High | [#] signals | [L/M/H] | [Specific outcome — e.g., "Rank top-5 for [keyword] within 6 months, 200+ organic visits/mo, 5+ earned links"] |

## Research Limitations

| Aspect | Limitation | Implication |
|---|---|---|
| [Data source] | [What couldn't be accessed — e.g., "exact search volumes require paid tool"] | [How this affects briefs — e.g., "volume estimates are directional"] |

## Next Step

Run `content-long` with the briefs above, or `seo` to validate technical SEO before publishing. Run `imc-plan` to incorporate long-form into a full channel strategy. Run `attribution` to set up tracking.
```

---

## Worked Example

**User:** "Long-form research for our AI code review tool — we want to rank for 'code review best practices' and earn earned media coverage in dev press."

**Route E (Full Long-Form Research Brief).** Topic: code review best practices. Audience: engineering leads + senior ICs. Goal: search ranking + earned media.

**L1 parallel results:**
- **serp-agent** found top-3 for "code review best practices" all 4000+ words, all use H2-pattern: definition → checklist → tools → metrics → counter-arguments. AI Overview present, cites Atlassian + Smartbear + a Stanford paper. Featured snippet held by Smartbear with a 6-bullet "checklist" answer.
- **keyword-intent-agent** identified 4 intent clusters: definitional ("what is code review"), procedural ("how to do code review"), comparative ("code review vs pair programming"), and tooling ("best code review tools 2026"). Long-tail goldmine: "code review checklist for [language]" patterns with strong PAA presence.
- **source-discovery-agent** named 7 cited authorities: Adam Tornhill (Code as a Crime Scene), Michaela Greiler (PhD on code review at Microsoft), Gergely Orosz (Pragmatic Engineer newsletter, 800K subs), Stanford SE researchers (cited in AI Overview), Smartbear's research team. Beat journalists at TheNewStack, InfoQ, DevClass.
- **deep-listening-agent** mined Stack Exchange Software Engineering for substantive 200+ word answers, found recurring narratives: "we adopted code review and shipped slower" (counter-argument worth addressing), "PR review = team learning loop" (positive framing), "tools don't fix process" (skepticism toward tool-led pieces).

**Research checkpoint:** User confirmed Tornhill quote-source target and shared internal data: existing post on PR cycle time ranks #14 for relevant query, 0 earned links — clear gap.

**L2 synthesis:**
- pattern-agent identified topic-cluster architecture: pillar = "Code Review Best Practices" (head term); clusters = (1) checklist by language, (2) metrics that matter, (3) review-tax counter-argument, (4) tooling 2026, (5) async vs synchronous review. Citation pattern: every top piece cites Greiler's Microsoft research.
- brief-agent produced 3 briefs:
  1. **Pillar: "The 2026 Code Review Playbook for Engineering Teams"** — 5000 words, target featured snippet via answer-first definition, cite Tornhill + Greiler + Stanford, internal-link all 5 clusters
  2. **Counter-argument byline: "We Adopted Code Review and Shipped Slower. Here's Why That's a Feature."** — pitch to TheNewStack and Pragmatic Engineer guest slot
  3. **Comparison cluster: "Async vs Synchronous Code Review: When Each Wins"** — target MOFU intent, internal-link to pillar

**Critic:** PASS. "Strong SERP-to-cluster alignment, citation strategy is link-magnet quality."

---

## Anti-Patterns

**Researching keywords without intent** — A keyword volume of 50,000 means nothing if the SERP is dominated by SaaS landing pages and you're writing a blog. Intent classification is non-negotiable.
**INSTEAD:** keyword-intent-agent gates every keyword on intent before it reaches the brief.

**Writing about what's ranking instead of what's missing** — Replicating the top result wins nothing. The opportunity is in the gap, the counter-argument, or the deeper coverage.
**INSTEAD:** pattern-agent identifies what top URLs cover and what they miss; brief-agent specifies a unique angle.

**Ignoring AI Overview and SERP features** — In 2026, the top-1 blue link gets fewer clicks if AI Overview answers the query above it. Either get cited in AI Overview or target queries where AI Overview isn't dominant.
**INSTEAD:** serp-agent flags AI Overview presence and cited URLs; brief-agent recommends structural choices that earn AI Overview citation.

**Generic source list** — "Cite industry experts" is not a strategy. The source list must be specific people with specific recent work and a specific reason they're authoritative on this topic.
**INSTEAD:** source-discovery-agent requires named experts with affiliation, recent work, and authority justification.

**Stale SERP data** — Google's index changes daily. SERP analysis older than 14 days may be wrong.
**INSTEAD:** Every SERP entry includes scrape date. Critic enforces 14-day staleness gate.

**Mistaking signal for pattern** — One #1-ranking outlier doesn't define the pattern. Look across the top 5-10 for what's consistent.
**INSTEAD:** pattern-agent requires patterns to appear in ≥50% of top-10 URLs before classifying as a "winning pattern."

**Difficulty blindness** — A high-volume keyword with all top-10 results from DR 90+ domains is out of reach for a DR 30 site. Ranking analysis must factor domain strength.
**INSTEAD:** serp-agent includes ranking difficulty calibration; pattern-agent flags out-of-reach keywords.

**Brief without distribution** — Long-form that nobody reads is waste. Every brief must specify distribution channels and the citation/source strategy that earns reach.
**INSTEAD:** brief-agent requires a Distribution field and authority-source citation plan.

---

## Required Artifacts

None — this is an entry point. Works best with upstream artifacts but functions without them.

### Optional Artifacts

| Artifact | Source | Benefit |
|---|---|---|
| `product-context.md` | icp-research | Product context for better topic and angle calibration |
| `icp-research.md` | icp-research | Persona data for audience-language matching and sophistication calibration |
| `market-research.md` | market-research | Competitor list for ranking analysis and positioning |
| `imc-plan.md` | imc-plan | Channel strategy for focused long-form research scope |

---

## References

- [references/serp-analysis-guide.md](references/serp-analysis-guide.md) — How to analyze SERPs, extract structural signals, and interpret SERP features in 2026
- [references/keyword-intent-taxonomy.md](references/keyword-intent-taxonomy.md) — Intent classification framework, modifier taxonomy, PAA mining patterns
- [references/long-form-brief-template.md](references/long-form-brief-template.md) — Structured long-form brief format with outline, intent, and distribution fields
