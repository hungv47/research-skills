---
name: market-research
description: "Analyzes market landscapes, competitive dynamics, TAM/SAM/SOM sizing, and whitespace opportunities for a product or category. Produces `.agents/market-research.md`. Not for building customer personas (use icp-research) or planning marketing campaigns (use imc-plan)."
argument-hint: "[market or product category]"
license: MIT
metadata:
  author: hungv47
  version: "2.0.0"
routing:
  intent-tags:
    - market-research
    - competitive-analysis
    - market-sizing
    - tam-sam-som
    - whitespace-analysis
  position: pipeline
  produces:
    - market-research.md
  consumes:
    - product-context.md
  requires: []
  defers-to:
    - skill: icp-research
      when: "need audience personas, not market landscape"
    - skill: problem-analysis
      when: "diagnosing a specific metric decline, not surveying the market"
  parallel-with:
    - problem-analysis
  interactive: false
  estimated-complexity: heavy
---

# Market Research — Orchestrator

*Strategy — Entry point. Maps market landscape, competitors, and identifies gaps and opportunities.*

**Core Question:** "What does the market look like, and where are the gaps worth filling?"

## Critical Gates — Read First

1. **Markets move fast — any source older than 18 months must be flagged as historical context.** Do not present stale data as current intelligence.
2. **TAM/SAM/SOM without methodology is a guess, not research.** Every sizing estimate needs a stated method (top-down, bottom-up, or both), source, and confidence level.
3. **Adjacent competitors are the highest threat — do NOT skip the adjacent check.** The biggest competitive threat often comes from an adjacent category expanding into yours, not from a direct competitor.
4. **Do NOT rely on training data for market positions or pricing — use WebSearch.** Competitor data changes constantly. Always verify with live research.

## Philosophy

Research rigor scales with stakes. A weekend project needs a quick landscape scan; a Series A pivot needs deep competitive intelligence. This skill produces evidence, not opinions. Every claim cites a source. Every opportunity has supporting data.

**Correctness > Verifiability > Completeness > Style.**

## Inputs Required
- Product context (from `.agents/product-context.md` or willingness to answer questions)

## Output
- `.agents/market-research.md`

## Chain Position
Previous: none | Next: `solution-design`, `icp-research`

### Skill Deference
- **Need to understand the MARKET and WHERE the gaps are?** → Use this skill.
- **Need to understand WHO the customer is and what they feel?** → Use `icp-research` (from marketing-skills) instead — it builds personas and VoC, not market maps.
- **Have a METRIC that's underperforming?** → Use `problem-analysis` instead — it diagnoses root causes, not landscapes.
- **Already know the problem and need SOLUTIONS?** → Use `solution-design` instead.

**Re-run triggers:** When entering a new market, when a major competitor launches or pivots, when fundraising (investors expect current data), or quarterly for fast-moving markets.

---

## Agent Manifest

7 agents across 2 layers:

| Agent | Layer | Role | Input | Output |
|-------|-------|------|-------|--------|
| [trends-agent](agents/trends-agent.md) | L1 (parallel) | Market trends with direction, evidence, quantification, implication | brief + scope | Market Trends table + narrative |
| [sizing-agent](agents/sizing-agent.md) | L1 (parallel) | TAM/SAM/SOM with methods and confidence levels | brief + scope | Market Sizing table + math |
| [competitor-agent](agents/competitor-agent.md) | L1 (parallel) | Feature matrix, pricing, positioning map, community, adjacent check | brief + scope + known competitors | Competitive Landscape (6 sub-sections) |
| [consumer-landscape-agent](agents/consumer-landscape-agent.md) | L1 (parallel) | Hot topics, cultural moments, sentiment, unmet needs | brief + scope | User & Consumer Landscape |
| [cross-analysis-agent](agents/cross-analysis-agent.md) | L2 (sequential) | Synthesizes L1 outputs into gap identification across 4 dimensions | merged L1 outputs | Gaps & Opportunities (4 gap types) |
| [opportunity-agent](agents/opportunity-agent.md) | L2 (sequential) | Ranks top 3 opportunities with evidence, window, risk, "why now" | cross-analysis output + L1 context | Top 3 Opportunities (ranked) |
| [critic-agent](agents/critic-agent.md) | L2 (sequential) | Validates citations, confidence, methodology, adjacent coverage | full merged artifact | PASS or FAIL with rewrite instructions |

---

## Routing Logic

### Route A: Quick Validation
**Trigger:** "Quick check on this market," "Who are the competitors?", "Is this space crowded?"

```
trends-agent ──┐
               ├──→ cross-analysis-agent → critic-agent
competitor-agent┘
```

Skip sizing-agent (not required). Consumer-landscape-agent optional (include if time allows). Opportunity-agent skipped — cross-analysis identifies gaps directly for Quick scope.

### Route B: Product Positioning
**Trigger:** "Position our product," "Where do we fit?", "Competitive analysis for [product]"

```
trends-agent ────────────┐
sizing-agent (optional) ─┤
competitor-agent ────────┼──→ cross-analysis-agent → opportunity-agent → critic-agent
consumer-landscape-agent ┘
```

All 4 L1 agents run in parallel. Full L2 sequence.

### Route C: Fundraising / Market Entry
**Trigger:** "Market analysis for investors," "Series A research," "Entering [market]," "Full market research"

```
trends-agent ────────────┐
sizing-agent ────────────┤
competitor-agent ────────┼──→ cross-analysis-agent → opportunity-agent → critic-agent
consumer-landscape-agent ┘
```

All 4 L1 agents run in parallel with enhanced depth. Sizing-agent is required (not optional). Opportunity-agent uses quantitative 1-5 scoring. Full L2 sequence.

---

## Scope Calibration

| Decision Context | Research Depth | Competitor Depth | Time Investment |
|-----------------|---------------|-----------------|----------------|
| Quick validation | 3-5 competitors, top-level features | Surface (pricing, positioning) | Light |
| Product positioning | 5-8 competitors, detailed features | Deep (features, community, growth) | Medium |
| Fundraising / market entry | 8-12+ competitors, full landscape | Comprehensive (all dimensions) | Heavy |

**How scope affects agent depth:**

| Section | Quick | Positioning | Fundraising |
|---------|-------|-------------|-------------|
| Market Trends | 2-3 trends, skip narrative | 3-5 trends + narrative | 5+ trends + narrative |
| Market Sizing | Skip | Optional (SAM estimate) | Required (TAM/SAM/SOM) |
| Feature Comparison | Top 5 features + type labels | 5-8 features + type labels | 8+ features + type labels |
| Pricing | Entry price + model only | Full pricing table | Full table + hidden costs |
| Positioning Map | Skip | Include | Include + adjacent entrants |
| Community & Mindshare | Skip | Top 3 competitors | All competitors |
| Gaps & Opportunities | Top 3 opportunities | Full 4-dimension analysis | Full analysis + scoring from reference |

---

## Dispatch Protocol

### Step 0: Product Context

Check for `.agents/product-context.md`. If missing: **Strongly recommended:** run `icp-research` (from `hungv47/marketing-skills`) first to create `.agents/product-context.md`. This skill works without it but produces significantly better analysis with product context. If the user prefers not to, interview for the product's category, target market, and key differentiator at minimum.

**If product-context.md exists**, extract before dispatch:
- **Product category** → determines which competitors to include and which features to compare
- **Differentiator** → anchors positioning map axes and gap identification
- **Pricing model** → frames pricing comparison dimensions
- **Target segment** → filters competitors by segment overlap (direct vs. adjacent)

### Step 1: Scope Interview

If the user provides a vague request ("research this market", "who are the competitors"):

1. **What market/industry?** — Name the product category and adjacent categories. (Not "tech" — which segment?)
2. **Geographic scope?** — Global, specific regions, or specific countries?
3. **Timeframe?** — Current snapshot, or include trajectory (last 2-3 years)?
4. **Known competitors?** — List any the user already tracks. Starting points, not the full list.
5. **What decisions will this inform?** — Building? Positioning? Fundraising? Pivoting? The answer determines scope and route.

All 5 answers are necessary before dispatch — without scope, research sprawls and produces a Wikipedia article instead of actionable intelligence.

### Single-Agent Fallback

If the task is narrow enough for a single agent (e.g., "just list the competitors" or "what's the market size?"), dispatch only the relevant agent without the full pipeline. Skip cross-analysis, opportunity, and critic agents. Return the single agent's output directly.

---

## Layer 1 Dispatch — Parallel Research Agents

Build the pre-writing context object from Step 0 and Step 1, then dispatch:

```
pre-writing = {
  category: [product category],
  differentiator: [from product-context or interview],
  pricing_model: [from product-context or interview],
  target_segment: [from product-context or interview],
  scope: "Quick" | "Positioning" | "Fundraising",
  geography: [from interview],
  timeframe: [from interview],
  known_competitors: [from interview]
}
```

**Dispatch all applicable L1 agents in parallel:**

| Agent | Always | References to Include |
|-------|--------|----------------------|
| trends-agent | Yes (all routes) | — |
| sizing-agent | Route C required, Route B optional, Route A skip | `references/market-sizing-guide.md` |
| competitor-agent | Yes (all routes) | `references/competitor-analysis-framework.md` |
| consumer-landscape-agent | Route B + C required, Route A optional | — |

### Research Tool Priority (all L1 agents)

1. **Exa MCP** or **Perplexity MCP** (if installed) — best for market reports, competitor analysis, trend data
2. **Firecrawl** or **Defuddle** (if installed) — for scraping specific pages (pricing, features, G2, Crunchbase)
3. **WebSearch** — always available as fallback

### Research Checkpoint

After L1 agents return, present findings and gather feedback before proceeding to L2:

**"Here's the competitive landscape I've found. Before I identify gaps and opportunities:"**
1. **Are these the right competitors?** Any missing? Any to deprioritize?
2. **Do you have internal competitive intel?** Sales battle cards, win/loss data, customer feedback about competitors?
3. **Any surprising findings you want me to dig deeper on?**

If user provides internal data (battle cards, sales notes, support tickets), incorporate into the merged L1 output before dispatching L2.

---

## Layer 2 Dispatch — Sequential Analysis

After L1 agents return and checkpoint feedback is incorporated, dispatch L2 agents in strict sequence:

### Step 1: Cross-Analysis Agent

```
dispatch cross-analysis-agent:
  upstream: [merged output from all L1 agents]
  references: [references/gap-analysis-template.md]
```

Receives the full merged L1 output. Identifies gaps across 4 dimensions: underserved segments, feature gaps, emerging trend gaps, positioning whitespace.

### Step 2: Opportunity Agent

```
dispatch opportunity-agent:
  upstream: [cross-analysis-agent output + merged L1 context]
  references: [references/gap-analysis-template.md]
```

Receives cross-analysis output plus L1 context for evidence sourcing. Force-ranks top 3 opportunities.

### Step 3: Critic Agent

```
dispatch critic-agent:
  upstream: [full merged artifact — all L1 + L2 outputs assembled into artifact template]
  references: [all reference files]
```

Receives the complete artifact. Evaluates against quality gate checklist. Returns PASS or FAIL.

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

**On rewrite:** Only re-dispatch the agents the critic names. Do not re-run the entire pipeline. The critic provides specific feedback per agent — pass it in the `feedback` field.

---

## Quality Gate

Before delivering, verify the merged artifact passes all checks:

- [ ] Every claim cites a source with URL or publication name
- [ ] Competitor table includes >=3 competitors with quantified size/growth signals
- [ ] Feature comparison covers >=5 capabilities relevant to the product category
- [ ] Gaps & Opportunities section identifies >=3 distinct opportunities with evidence
- [ ] Each Top 3 opportunity includes: evidence source, estimated window, risk level, and "why now"
- [ ] Market trends include >=2 quantified data points (%, $, growth rates)
- [ ] No source older than 18 months presented as current without historical flag
- [ ] Confidence level stated with justification
- [ ] Adjacent competitors section populated — never skipped
- [ ] TAM/SAM/SOM (if present) shows methodology, not just numbers

---

## Artifact Template

On re-run: rename existing artifact to `market-research.v[N].md` and create new with incremented version.

```markdown
---
skill: market-research
version: 1
date: {{today}}
status: draft
---

# Market Research

## Scope

**Market:** [product category / industry]
**Geography:** [scope]
**Decision context:** [what this research informs]
**Date:** [today]

## Market Trends

| Trend | Direction | Evidence | Quantification | Implication |
|-------|-----------|----------|---------------|-------------|

**Narrative:** [2-3 paragraph market story connecting trends]

## Market Sizing (if applicable)

| Metric | Method | Estimate | Source | Confidence |
|--------|--------|----------|--------|-----------|

## User & Consumer Landscape

| Dimension | Findings | Source |
|-----------|----------|--------|

## Competitive Landscape

### Overview
| Competitor | Founded | Funding/Revenue | Team Size | Target Segment | Positioning | Threat |
|-----------|---------|----------------|-----------|---------------|-------------|--------|

### Adjacent Competitors
| Adjacent Category | Player | Why They Could Enter | Likelihood | Signal to Watch |
|------------------|--------|---------------------|-----------|----------------|

### Feature Comparison
| Capability | [Your Product] | Competitor A | Competitor B | Competitor C |
|-----------|---------------|-------------|-------------|-------------|

### Pricing
| Competitor | Free Tier | Entry Price | Mid Tier | Enterprise | Model |
|-----------|-----------|------------|----------|-----------|-------|

### Positioning Map
[2-axis positioning map: axes selected from buyer decision criteria]

### Community & Mindshare
| Competitor | Community Size | Activity Level | Sentiment | Share of Voice |
|-----------|---------------|---------------|-----------|---------------|

## Gaps & Opportunities

### Gap Analysis (4 dimensions)

| Dimension | Gap | Evidence | Demand Signal | Difficulty |
|-----------|-----|----------|---------------|-----------|
| Underserved Segment | [Segment] | [Evidence] | [Signal] | — |
| Feature Gap | [Gap] | [User need] | [Evidence] | S/M/L |
| Emerging Trend | [Trend] | [Incumbent response] | [Opportunity window] | — |
| Positioning White Space | [Position] | [Why empty] | [Risk] | — |

### Top 3 Opportunities

| # | Opportunity | Evidence Source | Window | Risk | Why Now |
|---|------------|---------------|--------|------|---------|

## Limitations & Confidence

| Aspect | Confidence | Justification |
|--------|-----------|---------------|

**Data gaps:** [What couldn't be found]

## Next Step

Run `solution-design` to turn top opportunities into prioritized initiatives, or `icp-research` to build personas for identified underserved segments.
```

---

## Worked Example

**User:** "Research the AI code review market."

### Step 0 — Product Context
Checked `.agents/product-context.md` — not found. Interview initiated.

### Step 1 — Scope Interview
- "What market?" → "AI-powered code review tools — automated PR review, code quality analysis"
- "Geography?" → "Global, English-speaking focus"
- "Timeframe?" → "Current snapshot with 2-year trajectory"
- "Known competitors?" → "CodeRabbit, Sourcery, maybe Codacy"
- "What decisions?" → "We're building a new product and need to know where the gaps are"
- **Scope calibration:** "Building a new product" → **Route B: Product positioning** (5-8 competitors, detailed features)

### Layer 1 Dispatch (parallel)

**trends-agent output (summarized):**
```
| Trend | Direction | Evidence | Quantification | Implication |
|-------|-----------|----------|---------------|-------------|
| AI dev tools adoption | Growing rapidly | GitHub Octoverse 2025 | 78% of developers use AI coding tools (+32% YoY) | Market tailwind for AI code review |
| Shift-left quality | Accelerating | GitLab DevSecOps Survey 2025 | 65% of orgs testing in CI, up from 48% | Code review moving earlier in pipeline |
| LLM cost decline | Declining costs | OpenAI, Anthropic pricing pages | GPT-4 class: ~$3/M tokens (was $30 in 2023) | AI review becoming economically viable at scale |
```

**competitor-agent output (summarized):**
```
5 direct competitors mapped (CodeRabbit, Sourcery, Codacy, Qodo, Ellipsis)
3 adjacent competitors identified (GitHub Copilot, SonarQube, Sourcegraph)
Feature matrix: 7 capabilities, Stakes/Diff labeled
Pricing: all 5 competitors with full table
```

**consumer-landscape-agent output (summarized):**
```
Hot topics: AI hallucination in code suggestions, false positive fatigue
Sentiment: Cautiously optimistic — devs want AI review but distrust accuracy
Unmet needs: Cross-repo understanding, org-specific style enforcement, test generation
```

### Research Checkpoint
- User confirmed competitor list: "Yeah, those are the main ones. I'd add Qodo and Ellipsis too."
- User shared internal sales notes: "We lose deals to CodeRabbit on speed, to Codacy on compliance"
- Incorporated internal intel into merged L1 output.

### Layer 2 Dispatch (sequential)

**cross-analysis-agent** → Identified 3 underserved segments, 5 feature gaps, 3 emerging trends, 2 positioning whitespaces.

**opportunity-agent** → Ranked top 3:
1. Multi-repo context awareness (Medium risk, 12-18 month window)
2. Self-hosted AI review for regulated industries (Low risk, 6-12 month window)
3. Review-to-test pipeline (High risk, 12-24 month window)

**critic-agent** → PASS on first cycle. Notes: "Adjacent competitor section is strong. Sizing was correctly skipped for Positioning scope."

### Final Artifact
Merged all outputs into `.agents/market-research.md` per artifact template.

---

## Anti-Patterns

**Describing without concluding** — Listing competitors and features without identifying gaps and opportunities produces a Wikipedia article, not strategic intelligence. The Gaps & Opportunities section is the point of this skill — everything before it is setup.
**INSTEAD:** Ensure cross-analysis-agent and opportunity-agent run. If they are skipped, the output is incomplete.

**Unsourced market sizing** — "The market is worth $5B" without citing a source or methodology is fiction.
**INSTEAD:** Every market size claim needs a source, methodology (top-down or bottom-up), and confidence level. See [references/market-sizing-guide.md](references/market-sizing-guide.md).

**Recency blindness** — Using a 2022 report to describe a 2026 market. In fast-moving categories, 18-month-old data is historical context, not current intelligence.
**INSTEAD:** Flag source dates prominently. Instruct agents to prioritize sources from the last 12 months.

**Feature-only competitor analysis** — Comparing features while ignoring positioning, community, growth trajectory, and pricing produces an incomplete picture.
**INSTEAD:** competitor-agent analyzes all 6 dimensions (overview, adjacent, features, pricing, positioning, community). Do not accept feature-only output.

**Confirmation bias in gap identification** — Reverse-engineering "gaps" from your product's features isn't research — it's rationalization.
**INSTEAD:** cross-analysis-agent identifies gaps from user complaints, switching reasons, and unmet needs FIRST, then checks product fit.

**Treating all competitors as equal threats** — A $40M-funded company with 200 employees targeting your exact segment is a different threat level than a side project on Product Hunt.
**INSTEAD:** competitor-agent assigns threat levels (Critical/High/Medium/Low/Watch) with justification per the framework.

**Ignoring adjacent competitors** — The biggest competitive threat often comes from an adjacent category expanding into yours.
**INSTEAD:** competitor-agent always researches adjacent players. critic-agent specifically checks this was not skipped.

**Positioning map with self-serving axes** — Axes should reflect dimensions that MATTER to buyers, not dimensions that make your product look good.
**INSTEAD:** competitor-agent validates axis choices against what users actually compare when switching.

---

## Required Artifacts
None — this is an entry point for the Strategy track.

### Optional Artifacts
| Artifact | Source | Benefit |
|----------|--------|---------|
| `product-context.md` | icp-research (from hungv47/marketing-skills) | Product context for better competitor selection and gap identification |
| `problem-analysis.md` | problem-analysis | Known root causes focus competitive analysis on relevant dimensions |

---

## References

- [references/competitor-analysis-framework.md](references/competitor-analysis-framework.md) — Structured competitor evaluation methodology
- [references/market-sizing-guide.md](references/market-sizing-guide.md) — TAM/SAM/SOM methods and search patterns
- [references/gap-analysis-template.md](references/gap-analysis-template.md) — Framework for opportunity identification and scoring
