# Experiment Templates

Pre-built templates for common experiment types.

## A/B Test Template

| Field | Value |
|-------|-------|
| Initiative | [Name] |
| Hypothesis | If [change], then [metric] improves by [X%], because [reason] |
| Test Type | A/B — random split |
| Split | 50/50 (or 90/10 for risky changes) |
| Duration | [X days — enough for sample size] |
| Primary Metric | [Metric] |
| Baseline | [Current value] |

**Decision rules:**
- Success: [Metric] ≥ [X] → Roll out to 100%
- Iterate: [Metric] between [X-Y] → Change [one variable], rerun
- Kill: [Metric] < [X] after [duration] → Archive and move on

---

## Landing Page Test Template

| Field | Value |
|-------|-------|
| What's changing | [Headline / CTA / Layout / Social proof] |
| Traffic source | [Paid / Organic / Email] |
| Traffic volume | [Visitors/day] |
| Current conversion | [X%] |
| Target conversion | [Y%] |
| Min detectable effect | [Z% relative lift] |
| Required sample | [N per variant] |
| Duration | [Days to reach sample] |

---

## Email Test Template

| Field | Value |
|-------|-------|
| What's changing | [Subject line / Send time / CTA / Body copy] |
| List size | [N subscribers] |
| Test split | [10% test, 90% winner — or 50/50] |
| Primary metric | [Open rate / Click rate / Conversion rate] |
| Baseline | [Current value] |
| Duration | [24-48 hours for email] |

---

## Ad Creative Test Template

| Field | Value |
|-------|-------|
| Platform | [Meta / Google / LinkedIn / TikTok] |
| What's changing | [Visual / Headline / CTA / Audience] |
| Budget per variant | [$X/day] |
| Duration | [3-7 days minimum] |
| Primary metric | [CTR / CPA / ROAS] |
| Baseline | [Current value] |
| Min spend before judging | [$X or X impressions] |

---

## Feature Flag Test Template

| Field | Value |
|-------|-------|
| Feature | [What's being toggled] |
| Rollout | [X% of users] |
| Primary metric | [Activation / Retention / Revenue] |
| Guardrail metrics | [Support tickets / Error rate / Load time] |
| Duration | [1-2 weeks] |
| Rollback trigger | [Guardrail metric exceeds X] |
