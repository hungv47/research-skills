---
name: content-research
description: "Researches what content to create by analyzing competitor ads, audience communities, trending topics, and top-performing content across platforms. Produces `.agents/mkt/content-research.md`. Not for market landscape analysis (use market-research) or audience personas (use icp-research). For writing the content itself, see content-create."
argument-hint: "[niche, competitor, or topic]"
allowed-tools: Read Grep Glob Bash WebSearch WebFetch
license: MIT
metadata:
  author: hungv47
  version: "1.0.0"
  budget: deep
  estimated-cost: "$1-3"
routing:
  intent-tags:
    - content-research
    - ad-research
    - competitor-ads
    - content-intelligence
    - trend-detection
    - audience-listening
    - swipe-file
    - viral-content
    - ad-library
    - content-gaps
  position: pipeline
  produces:
    - mkt/content-research.md
  consumes:
    - product-context.md
    - mkt/icp-research.md
  requires: []
  defers-to:
    - skill: market-research
      when: "need strategic market landscape and competitive dynamics, not content-level intelligence"
    - skill: icp-research
      when: "need full persona profiles and buyer psychology, not real-time audience language"
    - skill: content-create
      when: "ready to write the content — research is done"
    - skill: seo
      when: "need technical SEO audit of existing pages, not pre-creation content gaps"
  parallel-with: []
  interactive: false
  estimated-complexity: heavy
---

# Content Research — Orchestrator

*Research — Pre-creation intelligence. Discovers what content to create by analyzing what's winning in the market right now.*

**Core Question:** "What content should we create, based on what's actually working for competitors and resonating with the audience?"

## Critical Gates — Read First

1. **Research informs — it does not decide.** Output is evidence and patterns, not a content calendar. The user decides what to act on.
2. **Recency is everything.** Content research older than 30 days is stale. Ad creative data older than 14 days should be flagged. Social trends older than 7 days may have already peaked.
3. **Longevity = performance proxy.** An ad running for 60+ days is almost certainly profitable. An ad that ran for 3 days and stopped probably failed. Use run duration as the primary performance signal when engagement metrics aren't available.
4. **Platform-native, not cross-posted.** Research what works ON each platform. A winning LinkedIn post structure is useless on TikTok. Always tag findings by platform.
5. **Use live data, not training data.** Do NOT rely on memorized knowledge for competitor content, ad creatives, or trending topics. Always use WebSearch, WebFetch, Exa, Tavily, or other research tools.

## Philosophy

The best content teams don't create in a vacuum — they research before they write. This skill bridges the gap between strategic research (market-research, icp-research) and content execution (content-create, copywriting). It answers: "What's working, what's missing, and what should we make?"

**Evidence > Intuition > Imitation.**

## Inputs Required

- Niche, industry, or product category (required)
- Competitor names or domains (optional — will discover if not provided)
- Target platforms/channels (optional — will research all if not specified)

## Output

- `.agents/mkt/content-research.md`

## Chain Position

Previous: `icp-research`, `market-research` (optional but recommended) | Next: `imc-plan`, `content-create`, `copywriting`

### Skill Deference

- **Need to know what CONTENT to create based on what's working?** → Use this skill.
- **Need to understand the overall MARKET and competitive dynamics?** → Use `market-research` instead — it maps markets, not content.
- **Need to understand WHO the customer is?** → Use `icp-research` instead — it builds personas, not content briefs.
- **Ready to WRITE the content?** → Use `content-create` instead — it executes, this skill researches.
- **Need to audit EXISTING pages for SEO?** → Use `seo` instead — it audits what's live, this researches what to create.

**Re-run triggers:** Before any content campaign, before entering a new channel, monthly for active content programs, when competitor creative strategy shifts noticeably.

---

## Agent Manifest

7 agents across 2 layers:

| Agent | Layer | Role | Input | Output |
|-------|-------|------|-------|--------|
| [ad-intel-agent](agents/ad-intel-agent.md) | L1 (parallel) | Researches competitor ads across platforms — hooks, offers, formats, run duration | brief + competitors + platforms | Ad Creative Intelligence section |
| [listening-agent](agents/listening-agent.md) | L1 (parallel) | Mines communities, reviews, forums for authentic audience language and pain points | brief + niche + communities | Audience Listening section |
| [trend-agent](agents/trend-agent.md) | L1 (parallel) | Identifies emerging topics and trending formats across platforms | brief + niche + platforms | Trending Topics section |
| [competitive-content-agent](agents/competitive-content-agent.md) | L1 (parallel) | Analyzes competitor organic content — top performers, gaps, formats, frequency | brief + competitors | Competitive Content Audit section |
| [pattern-agent](agents/pattern-agent.md) | L2 (sequential) | Synthesizes L1 outputs into winning patterns and content opportunities | merged L1 outputs | Patterns & Opportunities section |
| [brief-agent](agents/brief-agent.md) | L2 (sequential) | Converts opportunities into actionable content briefs with platform specs | pattern-agent output + L1 context | Content Brief Recommendations section |
| [critic-agent](agents/critic-agent.md) | L2 (sequential) | Validates data freshness, source quality, signal vs. noise, actionability | full merged artifact | PASS or FAIL with rewrite instructions |

---

## Routing Logic

### Route A: Ad Creative Intel

**Trigger:** "What ads are competitors running?", "Research the Meta ad library for [niche]", "Swipe file for [category]", "What creatives are working?"

```
ad-intel-agent → pattern-agent → critic-agent
```

Skip listening, trend, and competitive-content agents. Focus on paid creative intelligence only.

### Route B: Audience Listening

**Trigger:** "What is the audience saying?", "What questions do they ask?", "Reddit research for [topic]", "Find authentic language for [niche]"

```
listening-agent → pattern-agent → critic-agent
```

Focus on community mining and audience language extraction.

### Route C: Trend Research

**Trigger:** "What's trending in [niche]?", "Emerging topics for [category]", "What should we post about?"

```
trend-agent ──────────┐
                      ├──→ pattern-agent → critic-agent
competitive-content-agent┘
```

Combines trend detection with competitive content analysis to find trending topics competitors haven't covered yet.

### Route D: Competitive Content Audit

**Trigger:** "What content are competitors publishing?", "Content gap analysis", "What are they ranking for that we're not?"

```
competitive-content-agent → pattern-agent → critic-agent
```

Focus on competitor organic content — blog posts, videos, social, newsletters.

### Route E: Full Research Brief

**Trigger:** "Full content research for [niche]", "Research before our next content sprint", "What should we create next?"

```
ad-intel-agent ───────────┐
listening-agent ──────────┤
trend-agent ──────────────┼──→ pattern-agent → brief-agent → critic-agent
competitive-content-agent ┘
```

All 4 L1 agents run in parallel. Full L2 sequence produces actionable content briefs.

---

## Scope Calibration

| Decision Context | Research Depth | Competitor Count | Platforms |
|-----------------|---------------|-----------------|-----------|
| Quick check (single channel) | 1-2 agents, surface research | 2-3 competitors | 1-2 platforms |
| Campaign prep | 3-4 agents, moderate depth | 3-5 competitors | 2-4 platforms |
| Full content sprint | All agents, comprehensive | 5-8 competitors | All relevant platforms |

---

## Dispatch Protocol

### Step 0: Context Gathering

Check for upstream artifacts in this priority order:

1. **`.agents/product-context.md`** — Product category, differentiator, target audience (created by `icp-research`)
2. **`.agents/mkt/icp-research.md`** — Full persona profiles, VoC data, audience habitats
3. **`.agents/mkt/imc-plan.md`** *(optional bonus — not upstream)* — If it already exists from a prior run, use channel strategy to focus research on relevant platforms. Content-research is typically run before imc-plan, so this will often be null.

If none of the required artifacts (1-2) exist: interview for the minimum viable context:
1. **What's the product/niche?** — Category, differentiator, and who it's for
2. **Who are the competitors?** — At least 2-3 names or domains. If unknown, the competitive-content-agent and ad-intel-agent will discover them.
3. **Which platforms matter?** — Where does the audience spend time? If unknown, research all major platforms.
4. **What's the goal?** — Brand awareness? Lead gen? Sales? This determines which content types to prioritize.

### Step 1: Route Selection

Based on the user's request and available context, select Route A-E.

If the user's request is vague ("research content for us"), default to **Route E (Full Research Brief)**.

### Single-Agent Fallback

If the task is narrow enough for a single agent (e.g., "what are competitors running on Meta?"), dispatch only the relevant agent without the full pipeline. Skip pattern-agent, brief-agent, and critic. Return the single agent's output directly.

---

## Layer 1 Dispatch — Parallel Research Agents

Build the pre-writing context from Step 0:

```
pre-writing = {
  niche: [product category / industry],
  competitors: [list of competitor names/domains],
  platforms: [list of target platforms],
  goal: [awareness / leads / sales / engagement],
  product_context: [from product-context.md or interview],
  icp_data: [from icp-research.md or null],
  existing_channels: [from imc-plan.md if it exists, or null — optional bonus context]
}
```

**Dispatch applicable L1 agents in parallel:**

| Agent | Route A | Route B | Route C | Route D | Route E |
|-------|---------|---------|---------|---------|---------|
| ad-intel-agent | Yes | — | — | — | Yes |
| listening-agent | — | Yes | — | — | Yes |
| trend-agent | — | — | Yes | — | Yes |
| competitive-content-agent | — | — | Yes | Yes | Yes |

### Research Tool Priority (all L1 agents)

1. **Exa MCP** (`mcp__exa__web_search_exa`) — best for semantic content search, finding similar content
2. **Tavily MCP** (`mcp__tavily-remote-mcp__tavily_search`, `tavily_extract`) — structured web search with content extraction
3. **WebSearch** / **WebFetch** — always available as fallback
4. **Platform-specific access:**
   - Meta Ad Library: `facebook.com/ads/library` (free, no API key required — WebFetch)
   - TikTok Creative Center: `ads.tiktok.com/business/creativecenter` (browse top ads)
   - LinkedIn Ad Library: `linkedin.com/ad-library` (search by company)
   - Reddit: direct API access or Exa/Tavily search with `site:reddit.com`
   - YouTube: YouTube Data API or Exa search with `site:youtube.com`

### Research Checkpoint

After L1 agents return, present findings summary and gather feedback before L2:

**"Here's what I found across [platforms]. Before I identify patterns and build briefs:"**
1. **Any competitors I missed?** Names or domains to add.
2. **Any platforms I should skip or focus on?** Priority channels.
3. **Anything surprising that I should dig deeper on?**
4. **Do you have internal performance data?** Past content metrics, best-performing posts, email open rates — this dramatically improves pattern identification.

Incorporate feedback into the merged L1 output before dispatching L2.

---

## Layer 2 Dispatch — Sequential Synthesis

### Step 1: Pattern Agent

```
dispatch pattern-agent:
  upstream: [merged output from all L1 agents]
```

Receives all L1 outputs. Identifies cross-platform patterns, winning formulas, content gaps, and opportunity areas.

### Step 2: Brief Agent (Route E only)

```
dispatch brief-agent:
  upstream: [pattern-agent output + merged L1 context]
  references: [references/content-brief-template.md]
```

Converts patterns into 3-5 actionable content briefs, each specifying: topic, angle, format, platform, hook approach, evidence, and priority.

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

- [ ] Every finding cites a source with URL, date accessed, or platform reference
- [ ] No data older than 30 days presented as current without staleness flag
- [ ] Ad creative analysis includes run duration as performance proxy (not just engagement metrics)
- [ ] Audience language findings include direct quotes or paraphrases (not analyst interpretation)
- [ ] Trend signals appear on 2+ platforms before being classified as a "trend" (single-platform = "signal")
- [ ] Content gaps are evidenced by competitor absence + audience demand (not just "nobody's doing this")
- [ ] Every content brief (if Route E) includes: topic, angle, format, platform, hook approach, evidence
- [ ] Platform-specific findings tagged by platform (never "this works everywhere")
- [ ] Competitive findings include 3+ competitors analyzed
- [ ] Pattern identification distinguishes between correlation and causation

---

## Artifact Template

On re-run: rename existing artifact to `content-research.v[N].md` and create new with incremented version.

```markdown
---
skill: content-research
version: 1
date: {{today}}
status: draft
---

# Content Research: [Niche / Category]

**Date:** [today]
**Skill:** content-research
**Route:** [A / B / C / D / E]
**Competitors analyzed:** [list]
**Platforms covered:** [list]

## Ad Creative Intelligence

### Active Campaigns Overview

| Competitor | Platform | Format | Hook Type | Offer/CTA | Est. Run Duration | Status |
|-----------|----------|--------|-----------|-----------|-------------------|--------|
| [Name] | [Meta/TikTok/LinkedIn/YouTube] | [Video/Image/Carousel] | [Question/Bold claim/Social proof/How-to/UGC] | [Offer text] | [Days active] | Active/Paused |

### Winning Creative Patterns

| Pattern | Frequency | Platforms | Example | Why It Works |
|---------|-----------|-----------|---------|-------------|
| [Hook type / format / angle] | [How many competitors use it] | [Where] | [Specific example] | [Evidence-based reasoning] |

### Long-Running Ads (60+ days — high-confidence performers)

| Competitor | Platform | Hook | Key Message | Run Duration | Takeaway |
|-----------|----------|------|-------------|-------------|----------|

### Hook Analysis

| Hook Type | Count | Best Example | Platform | Why It Works |
|-----------|-------|-------------|----------|-------------|

### Offer & CTA Patterns

| CTA Type | Competitors Using | Platform | Destination |
|----------|------------------|----------|-------------|

## Audience Listening

### Top Questions & Pain Points

| Question / Pain Point | Source | Engagement Signal | Frequency | Verbatim Quote |
|----------------------|--------|-------------------|-----------|---------------|
| [What the audience asks/feels] | [Subreddit / forum / review site + link] | [Upvotes / replies / stars] | [How often this appears] | "[Exact words used]" |

### Authentic Language Map

| Concept (Brand Language) | Audience Language | Source | Usage Context |
|-------------------------|-------------------|--------|--------------|
| [How we say it] | [How they say it] | [Where found] | [When they use this phrase] |

### Emotional Triggers

| Trigger | Evidence | Strength | Content Angle |
|---------|----------|----------|--------------|

### Community Landscape

| Community | Platform | Size | Activity | Relevance | Key Themes |
|-----------|----------|------|----------|-----------|-----------|

## Trending Topics

### Cross-Platform Trends (appearing on 2+ platforms)

| Topic | Platforms | Velocity | Peak Window | Our Angle |
|-------|----------|----------|-------------|-----------|
| [Topic] | [Reddit + TikTok + YouTube...] | [Rising/Peaking/Declining] | [Estimated days remaining] | [How we'd cover this differently] |

### Platform-Specific Signals (single platform)

| Platform | Signal | Evidence | Risk Level |
|----------|--------|----------|------------|
| [Platform] | [Trending topic/format/sound] | [URL or data point] | [Low: established / Med: emerging / High: may peak soon] |

### Emerging Formats

| Format | Platform | Adoption Stage | Example | Opportunity |
|--------|----------|---------------|---------|------------|

### Trending Keywords & Hashtags

| Keyword/Hashtag | Platform | Volume/Growth | Relevance |
|----------------|----------|--------------|-----------|

## Competitive Content Audit

### Competitor Content Mix

| Competitor | Platform | Content Types | Frequency | Audience Size | Top Performer | Engagement |
|-----------|----------|--------------|-----------|--------------|---------------|-----------|

### Content Gap Matrix

| Topic / Angle | Us | Competitor A | Competitor B | Competitor C | Demand Signal |
|--------------|-----|-------------|-------------|-------------|--------------|
| [Topic] | [Covered/Missing] | [Covered/Missing] | [Covered/Missing] | [Covered/Missing] | [Search vol / community interest] |

### Top-Performing Competitor Content (last 90 days)

| Competitor | Content | Platform | Format | Engagement | Why It Worked |
|-----------|---------|----------|--------|-----------|---------------|

### Publishing Patterns

| Competitor | Frequency | Best Day/Time | Formats Used | Content Pillars |
|-----------|-----------|---------------|-------------|----------------|

### Content Quality Assessment

| Competitor | Strengths | Weaknesses | Unique Approach |
|-----------|-----------|-----------|----------------|

## Patterns & Opportunities

### Winning Patterns (cross-cutting)

| # | Pattern | Evidence | Platforms | Confidence |
|---|---------|----------|-----------|-----------|
| 1 | [Pattern description] | [N competitors use this, M community signals] | [Where] | H / M / L |

### Content Opportunities (ranked by evidence strength)

| # | Opportunity | Type | Evidence | Platform | Priority |
|---|------------|------|----------|----------|----------|
| 1 | [What to create] | [Gap / Trend / Audience need / Format] | [Supporting data] | [Where to publish] | High / Med / Low |

### Anti-Patterns to Avoid

| Pattern | Why It Fails | Evidence |
|---------|-------------|----------|

### Signal Strength Map

| Signal Source | Data Point | Supports Opportunity # |
|--------------|-----------|----------------------|

## Content Briefs (Route E only)

### Brief 1: [Title]

- **Topic:** [Specific topic]
- **Angle:** [Unique angle — what makes ours different from what exists]
- **Format:** [Blog / Video / Carousel / Thread / Ad / etc.]
- **Platform:** [Primary platform + secondary distribution]
- **Hook approach:** [Question / Bold claim / Social proof / How-to / Story / Data] — based on [winning pattern #N]
- **Key message:** [Core idea in one sentence]
- **Audience language to use:** [3-5 specific phrases from listening research]
- **Audience language to avoid:** [Brand jargon the audience doesn't use]
- **Evidence:** [Why this will work — cite specific research findings]
- **Competitive positioning:** [What competitors do/don't cover on this topic]
- **Priority:** [High / Med / Low] — [Justification]
- **Suggested execution:** `content-create` Route [A/B/D] → Channel: [specific channel]
- **Tracking:** [Suggested UTM structure or attribution approach]

### Brief 2: [Title]
[Same structure]

### Brief 3: [Title]
[Same structure]

### Brief Prioritization Summary

| # | Brief | Priority | Evidence Signals | Est. Effort | Impact |
|---|-------|----------|-----------------|-------------|--------|

## Research Limitations

| Aspect | Limitation | Implication |
|--------|-----------|-------------|
| [Data source] | [What couldn't be accessed] | [How this affects findings] |

## Next Step

Run `content-create` with the content briefs above, or `imc-plan` to incorporate these findings into a full channel strategy. Run `attribution` to set up tracking before publishing.
```

---

## Worked Example

**User:** "Research content opportunities for our AI code review tool before our next content sprint."

**Route E (Full Research Brief)** with 5 competitors (CodeRabbit, Sourcery, Codacy, Qodo, Ellipsis). Product context: AI code review, multi-repo awareness differentiator, targeting engineering teams.

**L1 parallel results:** ad-intel found CodeRabbit's 78-day carousel ("5 things your code reviewer misses") as high-confidence winner. Listening mined "review tax" and "PR bottleneck" as authentic audience phrases from r/ExperiencedDevs. Trends detected "AI dev tools backlash" cross-platform and LinkedIn "hot take" format 3x outperformance. Competitive audit found multi-repo content gap and zero user-led (vs. founder-led) video testimonials.

**Research checkpoint:** User confirmed multi-repo as sweet spot, shared internal data (top blog post: "reducing PR cycle time", 8K views).

**L2 synthesis:** pattern-agent identified 5 cross-cutting patterns. brief-agent produced 3 briefs:
1. **"The Review Tax Calculator"** — interactive tool + blog, using "review tax" audience language
2. **"Hot Take: Most AI Code Review Tools Miss 80% of What Matters"** — contrarian LinkedIn post on multi-repo gap
3. **"Engineering Lead Video Testimonial Series"** — 60-sec clips, LinkedIn + YouTube Shorts

**Critic:** PASS. "Strong evidence-to-brief connection."

---

## Anti-Patterns

**Researching without a niche** — "Research trending content" with no product category produces a BuzzFeed listicle, not actionable intelligence. Always anchor research to a specific niche and audience.
**INSTEAD:** Require niche/category before dispatch. If the user doesn't provide one, pull from `product-context.md` or interview.

**Copying competitor content** — Research identifies what's working so you can create something BETTER with a unique angle, not so you can clone it.
**INSTEAD:** pattern-agent identifies the underlying principle (why it works), not a template to copy. brief-agent always specifies a unique angle.

**Confusing signals with trends** — A single viral post is a signal. A pattern across 2+ platforms is a trend. Treating signals as trends leads to chasing noise.
**INSTEAD:** trend-agent requires 2+ platform appearances before classifying anything as a "trend." Single-platform findings are labeled "signals."

**Ignoring ad longevity** — Engagement metrics (likes, shares) are vanity. Run duration is the real performance proxy — nobody keeps running unprofitable ads.
**INSTEAD:** ad-intel-agent prioritizes run duration over engagement metrics. 60+ day ads get a dedicated "high-confidence performers" section.

**Stale research treated as current** — Content landscapes change weekly. A 60-day-old competitive analysis may be completely wrong.
**INSTEAD:** Every finding includes a date. The 30-day staleness gate is enforced by the critic.

**Platform-blind recommendations** — "Post this on social media" is not actionable. Each platform has different winning formats, audiences, and algorithms.
**INSTEAD:** Every finding and every brief is tagged by specific platform. No cross-platform generalizations without evidence.

**Research without action** — A beautiful research document that doesn't lead to content creation is waste.
**INSTEAD:** Route E always produces content briefs. Every brief includes a suggested `content-create` route.

---

## Required Artifacts

None — this is an entry point. Works best with upstream artifacts but functions without them.

### Optional Artifacts

| Artifact | Source | Benefit |
|----------|--------|---------|
| `product-context.md` | icp-research | Product context for better competitor identification and angle development |
| `icp-research.md` | icp-research | Persona data for audience-language matching and community identification |
| `imc-plan.md` | imc-plan | Channel strategy for focused platform research |
| `market-research.md` | market-research | Competitor list and positioning for competitive content analysis |

---

## References

- [references/content-brief-template.md](references/content-brief-template.md) — Structured content brief format with platform-specific fields
- [references/ad-library-guide.md](references/ad-library-guide.md) — How to access and interpret ad libraries across platforms
- [references/platform-research-patterns.md](references/platform-research-patterns.md) — Search queries and data extraction patterns per platform
