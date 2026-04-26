# Long-Form Brief Template

Structured format for actionable long-form content briefs that bridge content-research-long findings to `content-long` execution.

## Brief Structure

Each brief must contain ALL of the following fields. Missing fields produce vague content.

```markdown
### Brief [N]: [Working Title]

- **Target keyword:** [primary keyword] (volume: [#], intent: [type], difficulty: [L/M/H])
- **Secondary keywords / PAA to cover:** [list — these become H2s/H3s]
- **Search intent:** [Informational / Commercial / Transactional / Navigational + sub-intent]
- **Working title:** [Title with primary keyword + curiosity hook OR contrarian framing OR specific outcome]
- **Lede approach:** [Stat / Story / Contrarian claim / Definition / Question / Decision narrative] — based on [Pattern #N]
- **Outline (H2s):**
  1. [H2 — addresses sub-intent or PAA question]
  2. [H2]
  3. [H2]
  4. [H2]
  5. [H2 — counter-argument or "common mistakes"]
  6. [H2 — synthesis / next steps / FAQ]
- **Word-count target:** [Median word count of top-5 ranking URLs ± 20%, with reasoning]
- **Internal-link targets:**
  - [URL — anchor text suggestion]
  - [URL — anchor text suggestion]
  - [URL — anchor text suggestion]
  - OR: "no existing coverage to link to — flag for cluster expansion: [topics]"
- **External authoritative sources to cite:**
  - [Authority + work URL + why this source — from Source Map]
  - [Authority + work URL + why this source]
  - [Authority + work URL + why this source]
- **Expert quotes to source (active outreach):**
  - [Authority + reach path + suggested ask] — Tier 2/3 from Source Map
- **Audience language to use (3-5 phrases):**
  - "[Phrase]" (source URL, engagement signal) — from Deep Listening
- **Audience language to avoid:** [Brand jargon the audience doesn't use]
- **Counter-arguments to address:**
  - [Counter-argument from Deep Listening] — addressed in H2 #[N]
- **Schema markup:** [Article / HowTo / FAQPage / Comparison / Review]
- **SERP feature target:**
  - Featured snippet via [paragraph / list / table format]
  - AI Overview citation via [structured data + authority signals]
  - PAA inclusion for [specific PAA question]
- **Multimedia plan:** [Original images / data viz / video / interactive element]
- **Distribution plan:**
  - Newsletter feature: [Specific newsletter] — [pitch angle]
  - LinkedIn share: [Author / executive] — [hook for the share]
  - Reddit / community: [Specific subreddit + posting approach]
  - Podcast pitch: [Specific show] — [pitch angle]
  - Byline cross-post: [Outlet] — [pitch angle]
  - Authority outreach: [Person → ask: cite, share, quote, intro]
- **Competitive positioning:**
  - What top URLs cover well: [list]
  - What top URLs miss: [list — these become differentiators]
  - Our angle: [specific positioning]
- **Priority:** [High / Med / Low] — [Justification with opportunity score + evidence count]
- **Suggested execution:** `content-long` Route [TBD] → Format: [pillar guide / case study / byline / etc.]
- **Tracking:**
  - UTM: [?utm_source=X&utm_medium=Y&utm_campaign=brief-slug]
  - GSC monitoring: [primary keyword + 3-5 long-tails]
  - Earned-link target: [N links / N referring domains within X months]
  - Newsletter mentions target: [N within X months]
```

## Field Guidelines

### Working Title

Bad: "Code Review Best Practices"
Good: "The 2026 Code Review Playbook for Engineering Teams" (outcome + audience + freshness)

Title patterns:
- **Outcome + audience** — "The [Topic] Playbook for [Audience]"
- **Stat + curiosity** — "78% of [X] Miss This. Here's What Actually Works."
- **Contrarian + reasoning** — "We [Did X] and [Bad Outcome]. Here's Why That's a Feature."
- **Comparison + specificity** — "[X] vs [Y]: When Each Wins"
- **Definitive guide** — "The 2026 Guide to [Topic]"
- **Year-stamped + authority** — "[Topic] in 2026: What [Audience] Actually Do"

### Lede Approach

Match lede to intent and pattern:
- **Stat** — when original/cited research is available
- **Story** — when decision narrative from deep-listening is strong
- **Contrarian claim** — when counter-argument from deep-listening is recurring and unaddressed by competitors
- **Definition** — for definitional informational intent
- **Question** — when PAA reveals a single dominant question
- **Decision narrative** — for case studies and bylines

### Outline Construction

Build outline from:
1. **PAA questions** → H2/H3 (these are confirmed search demand)
2. **Top-ranking URL H2 patterns** (from serp-agent) → cross-validate structure
3. **Counter-arguments from deep-listening** → differentiating H2 (your edge)
4. **Source Map authorities** → outline placement for citations

Outline patterns by intent:

**Informational (definitional / how-to):**
1. TL;DR / quick definition
2. What it is (depth)
3. Why it matters
4. How to do it (step-by-step or principles)
5. Common mistakes (counter-arguments)
6. When NOT to do it
7. Tools / further reading
8. FAQ (PAA harvest)

**Commercial (comparison / best-of):**
1. TL;DR / quick recommendation
2. Selection criteria
3. Top option 1 (deep dive)
4. Top option 2 (deep dive)
5. Top option N
6. Comparison table
7. How to choose for your use case
8. FAQ

**Counter-argument / contrarian:**
1. The conventional wisdom (state it fairly)
2. The story / data that contradicts it
3. Why the conventional wisdom fails
4. What actually works
5. Edge cases
6. Implications

**Case study:**
1. The protagonist (specific company / team / person)
2. The starting situation (with metrics)
3. The challenge / decision
4. The approach (in detail)
5. The outcome (with metrics)
6. Lessons learned (generalizable)
7. How others can apply

### Word-Count Target

Pull from serp-agent's top-5 median ± 20%. Don't pad to hit a number. If outline is substantive, length follows.

### Internal-Link Targets

- Aim for 3-5 internal links per piece
- Use the secondary keyword the linked page targets as anchor text (not "click here")
- Link from contextually relevant H2s, not the conclusion
- If no existing coverage exists, flag for future cluster expansion

### External Citation Logic

- ≥3 external authoritative sources per brief
- Mix tiers: Tier 1 (canonical), Tier 2 (recent applied), Tier 3 (community signals)
- Anchor original research / datasets where available
- Avoid Wikipedia as primary citation — find the underlying source

### Expert Outreach Targets

- 1+ Tier 2/3 authority per brief from Source Map
- Specify reach path (URL, email pattern, booking form)
- Specify the ask (interview / quote / cite-and-share / co-byline)

### Audience Language

Pull directly from deep-listening output. Include exact phrase, source URL, engagement signal.

Bad: "Users are frustrated with slow code reviews"
Good:
- Use: "review tax" (Stack Exchange Software Engineering, accepted answer, 230 votes)
- Use: "PR bottleneck" (Pragmatic Engineer comments, 15 substantive replies)
- Avoid: "automated code quality analysis" (brand jargon — 0 organic usage)

### Counter-Arguments

Every brief should address ≥1 counter-argument from deep-listening. This is the differentiator vs. coverage that ignores objections.

If deep-listening surfaced none, note "no recurring counter-argument surfaced — opportunity to lead the discourse on [topic]."

### Schema Markup by Intent

| Intent | Schema |
|---|---|
| Informational (definitional / how-to) | Article + HowTo (if procedural) + FAQPage (if FAQ section) |
| Commercial (comparison) | Article + Review (if reviewing) + ItemList (if listicle) |
| Commercial (vs) | Article + Comparison-style structured data |
| Case study | Article + Review or CaseStudy structured data |
| Press / byline | Article + Author |

### SERP Feature Targets

Per featured snippet format observed in serp-agent:
- **Paragraph** → H2 phrased as query + 40-60 word answer
- **List** → H2 phrased as query + ordered/unordered list of 4-8 items
- **Table** → H2 phrased as query + HTML table

For AI Overview:
- Structured data (Article + FAQPage)
- Original data points
- Authority signals (cited expert quotes)
- Specificity (concrete examples)
- Freshness signals (recent dates)

For PAA:
- H2/H3 phrased exactly as PAA question
- 40-80 word direct answer
- FAQPage schema

### Distribution Plan

Every brief covers ≥3 distribution channels with named targets from Source Map:
- Newsletter feature (specific newsletter)
- LinkedIn share (specific author/executive)
- Community (specific subreddit / Discord / Slack)
- Podcast pitch (specific show)
- Byline cross-post (specific outlet)
- Authority outreach (specific person)

"Publish and hope" is not a distribution plan.

### Tracking

- UTM structure for all distribution links
- GSC queries to monitor (primary keyword + long-tails)
- Earned-link target with timeframe
- Newsletter mention target with timeframe

## Prioritization Summary Table

After all briefs:

```markdown
### Brief Prioritization Summary

| # | Brief | Priority | Evidence Signals | Est. Effort | Expected Impact |
|---|---|---|---|---|---|
| 1 | [Title] | High | [#] signals (SERP gap + AI Overview opportunity + audience demand + authority alignment) | [L/M/H] | "Rank top-5 for [keyword] within 6 months, 200+ organic visits/mo, 5+ earned links, 1 newsletter feature" |
```

## Cluster Sequencing Recommendation

If Route E with topic cluster, recommend publishing order:

```markdown
### Cluster Sequencing Recommendation

Recommended publishing order:
1. [Cluster piece — easy win, builds internal-link target for pillar]
2. [Cluster piece — earns links via outreach to Tier-3 authority]
3. [Pillar piece — links to all published cluster pieces, leverages cluster credibility]
4. [Remaining cluster pieces — link to pillar, complete the cluster]

Reasoning: [Cluster-first builds the internal-link foundation that helps the pillar rank, AND distributes outreach load over time]
```

## Common Mistakes

1. **Vague briefs** — "Write a comprehensive guide" is not a brief. Spell out outline, sources, angle.
2. **Missing audience language** — If the brief doesn't include phrases from deep-listening with sources, it's not research-informed.
3. **No evidence trace** — Every spec should trace to an L1 finding or pattern. "Seems like a good idea" doesn't qualify.
4. **No competitive positioning** — If the brief doesn't say what we do *differently*, we're writing a worse copy.
5. **Missing distribution** — A brief without a distribution plan = a piece that won't get read.
6. **Citation theater** — Listing authorities to cite without specifying *where* in the outline = empty signaling.
7. **Counter-arguments ignored** — If deep-listening surfaced an objection and the brief doesn't address it, the differentiator is wasted.
8. **Tracking afterthought** — "Add tracking after publish" leads to no tracking. Specify upfront.
9. **Outline cribbed from one URL** — If your outline matches one top-ranking piece, you're writing a worse copy. Synthesize across top-5.
10. **Word-count padding** — Hitting a number ≠ depth. Match length to outline substance.
