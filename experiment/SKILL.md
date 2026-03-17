---
name: experiment
description: "Design experiments, create A/B tests, validate hypotheses with minimum viable tests, test this idea, how do I validate this — experimentation, A/B testing, test design, decision rules. Works across any domain: landing pages, pricing, onboarding flows, ad creative, feature flags."
license: MIT
metadata:
  author: hungv47
  version: "1.1.0"
---

# Experiment Design

*Strategy — Step 4 of 4. Designs minimum viable tests with clear success/iterate/kill rules.*

**Core Question:** "What's the cheapest way to learn if this works?"

## Inputs Required
- An initiative with hypothesis and target metric (from `.agents/solution-design.md` + `.agents/targets.md`)
- OR: User describes what they want to test

## Output
- `.agents/experiment-[name].md`

## Quality Gate
Before delivering, verify:
- [ ] Executable in ≤2 weeks
- [ ] Costs <10% of full initiative budget (or $0 for internal-only changes)
- [ ] Success threshold is a specific number (not "significant improvement" or "we'll know it when we see it")
- [ ] Sample size is sufficient (checked against table below) — or test type adjusted if insufficient

## Chain Position
Previous: `funnel-planner` | Next: implement winning variant, or re-diagnose with `problem-analysis` if inconclusive

---

## Before Starting

### Step 0: Product Context

Check for `.agents/product-context.md`. If missing: **INTERVIEW.** Ask the user 8 product questions (what, who, problem, differentiator, proof points, pricing, objections, voice) and save to `.agents/product-context.md`. Or recommend running `icp-research (from hungv47/comms-skills)` to bootstrap it.

### Required Artifacts
| Artifact | Source | If Missing |
|----------|--------|------------|
| `solution-design.md` | solution-design | **INTERVIEW.** Ask what to test. |
| `targets.md` | funnel-planner | **INTERVIEW.** Ask for baseline metrics. |

### Optional Artifacts
| Artifact | Source | Benefit |
|----------|--------|---------|
| `product-context.md` | icp-research (from hungv47/comms-skills) | Test design context |

### Initiative & Target Review
Read `.agents/solution-design.md` and `.agents/targets.md` if they exist. If not, interview for:
1. "What are you trying to test? What's your hypothesis?"
2. "What metric are you measuring? What's the current baseline?"
3. "What would success look like? What would make you stop?"

A hypothesis, a target metric, and a baseline number are the minimum required inputs — without them, success criteria become subjective.

---

## Step 1: Choose Test Type

| Type | When | Rigor |
|------|------|-------|
| **A/B Test** | Can split traffic randomly; enough volume | High — controls for time |
| **Before-After** | Can't split (infrastructure change, whole-site change) | Medium — time effects confound |
| **Cohort** | Testing on a specific segment first | Medium — segment differences |
| **Pilot** | New capability; need qualitative signal first | Low — directional only |

If unsure: check sample size (Step 3). Not enough volume for A/B in 2 weeks → use Before-After or Pilot.

---

## Step 2: Define Decision Rules

Define specific numeric thresholds — vague thresholds cause endless "let's give it one more week" cycles.

| Outcome | Threshold | Action |
|---------|-----------|--------|
| **Success** | [metric] ≥ [number] | Scale: roll out fully |
| **Iterate** | [metric] between [X] and [Y] | Change ONE variable: [specify which]. Rerun for [duration]. Max 2 iterations. |
| **Kill** | [metric] < [number] after [full duration] | Archive learnings. Re-diagnose using `problem-analysis`. |

---

## Step 3: Sample Size Check

For A/B tests — minimum visitors PER VARIANT:

| Baseline Rate | 20% Relative Lift | 50% Lift |
|--------------|-------------------|----------|
| 1% | ~40,000 | ~6,400 |
| 5% | ~8,000 | ~1,300 |
| 10% | ~4,000 | ~640 |
| 25% | ~1,600 | ~260 |

**Quick math:** [Your daily traffic] × [test days] ÷ 2 = visitors per variant. Compare against table.

If insufficient for A/B: use Before-After, or make a bigger change (aim for 50%+ lift instead of 20%).

See [references/sample-size-guide.md](references/sample-size-guide.md) for full tables.

---

## Artifact Template

On re-run: rename existing artifact to `experiment-[name].v[N].md` and create new with incremented version.

```markdown
---
skill: experiment
version: 1
date: {{today}}
status: draft
---

# Experiment: [Name]

**Initiative:** [from prioritized list]

## Design

| Field | Value |
|-------|-------|
| Hypothesis | If [action], then [metric] changes from [baseline] to [target], because [reason] |
| Test Type | A/B / Before-After / Cohort / Pilot |
| Duration | [≤2 weeks] |
| Budget | [$X or $0] |
| Primary Metric | [metric name] |
| Baseline | [current value] |

## Decision Rules

| Outcome | Threshold | Action |
|---------|-----------|--------|
| Success | [metric] ≥ [X] | [Scale how] |
| Iterate | [metric] between [X-Y] | Change: [one specific variable]. Rerun: [duration]. |
| Kill | [metric] < [X] after [duration] | Archive. Next: [what to try instead] |

## Sample Size Check

- Daily traffic: [N]
- Test duration: [days]
- Visitors per variant: [N × days ÷ 2]
- Required for [X%] lift at baseline [Y%]: [from table]
- **Sufficient?** Yes / No → [if no, adjusted approach]

## Results

[Filled in after experiment concludes]
- **Outcome:** Success / Iterate / Kill
- **Primary metric result:** [actual value]
- **Duration:** [actual days run]
- **Key finding:** [one sentence — what did we learn?]

## Post-Mortem

[Filled in for Kill decisions]
- **What we tested:** [summary]
- **Why it failed:** [root cause of failure]
- **What we learned:** [insight for future initiatives]
- **Next action:** Run `solution-design` for alternatives / Run `problem-analysis` to re-diagnose

## Next Step

[When to check results. What to do with each outcome.]
```

---

## Worked Example 1: A/B Test

```markdown
# Experiment: Paid Targeting Lookalike Test

**Date:** 2026-03-13
**Skill:** experiment
**Initiative:** Restore + Refine Paid Targeting

## Design

| Field | Value |
|-------|-------|
| Hypothesis | If we use a 1% converters lookalike audience, then paid signup rate increases from 1.2% to ≥3%, because the lookalike matches our ICP better than current broad targeting |
| Test Type | A/B — split paid budget 50/50 between current and lookalike |
| Duration | 10 days |
| Budget | $500 (redirecting existing $50/day spend) |
| Primary Metric | Paid visitor-to-signup rate |
| Baseline | 1.2% |

## Decision Rules

| Outcome | Threshold | Action |
|---------|-----------|--------|
| Success | ≥2.5% signup rate | Roll out lookalike to 100% of paid budget |
| Iterate | 1.5-2.5% | Narrow lookalike from 1% to 0.5%, add interest-based exclusions. Rerun 10 days. |
| Kill | <1.5% after 10 days | Targeting isn't the primary issue. Test homepage messaging next. |

## Sample Size Check

- Daily paid traffic: ~300 visitors
- 10 days: 3,000 total → 1,500 per variant
- At 1.2% baseline, need ~1,300 per variant for 50% lift detection
- **Sufficient?** Yes — 1,500 > 1,300. Can detect a 50%+ relative lift (1.2% → 1.8%+).
```

---

## Worked Example 2: Before-After

```markdown
# Experiment: Homepage Social Proof Restoration

**Date:** 2026-03-13
**Skill:** experiment
**Initiative:** Restore Homepage Social Proof

## Design

| Field | Value |
|-------|-------|
| Hypothesis | If we restore customer logos + testimonial to homepage hero, then bounce rate drops from 52% to ≤42%, because the redesign removed trust signals that reduced visitor confidence |
| Test Type | Before-After — change affects all visitors |
| Duration | 7 days after deployment |
| Budget | $0 (copy/design work only) |
| Primary Metric | Homepage bounce rate |
| Baseline | 52% |

## Decision Rules

| Outcome | Threshold | Action |
|---------|-----------|--------|
| Success | Bounce ≤ 42% | Keep changes. Explore further trust signal additions. |
| Iterate | 42-48% bounce | Add more proof: case study snippet, user count, or G2 rating badge. Rerun 7 days. |
| Kill | >48% after 7 days | Social proof alone isn't enough. Run full Homepage A/B (old vs new) from parked list. |

## Sample Size Check

- Daily homepage visitors: ~800
- 7 days: 5,600 visitors
- Before-After with 52% baseline: 5,600 is ample to detect a 10-point bounce rate change
- **Sufficient?** Yes
```

---

## Loop-Back Protocol

When an experiment concludes, follow the appropriate path:

### Kill Decision
1. Append learnings to the experiment file under `## Post-Mortem`
2. Check: Is the root cause (`.agents/problem-analysis.md`) still valid?
   - **Yes → Route to solution design:** Run `solution-design` with constraint: "[Failed approach] did not work because [reason]. Generate alternatives that avoid this failure mode."
   - **No → Route to problem analysis:** Problem may have shifted. Re-diagnose using `problem-analysis`.
3. Update `.agents/solution-design.md` — move killed initiative to "Kill" status with reason.

### Iterate Decision
1. Change exactly ONE variable from the original test
2. Maximum 2 iterations allowed before triggering Kill protocol
3. Document iteration rationale in experiment file under `## Iterations`

### Scale Decision
1. Document final results in experiment file under `## Results`
2. Move to full execution — no further testing needed for this initiative
3. Map the scaled initiative to KPIs and track attribution

---

## After the Experiment

| Result | Do This |
|--------|---------|
| **Success** | Scale. Set full targets via `funnel-planner`. Celebrate briefly, then test the next initiative. |
| **Iterate** | Change ONE variable. Document what and why. Max 2 iterations before escalating to kill/pivot. |
| **Kill** | Archive in the experiment file: what you tested, what happened, what you learned. Return to `solution-design` with the new learning. |

---

## References

- [references/experiment-templates.md](references/experiment-templates.md) — Templates for landing page, email, ad creative, feature flag tests
- [references/sample-size-guide.md](references/sample-size-guide.md) — Full sample size tables and common mistakes
