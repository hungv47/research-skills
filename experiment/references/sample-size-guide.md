# Sample Size Guide

How to determine if you have enough traffic/users to run a meaningful test.

## Quick Reference Table

Minimum visitors per variant to detect a given relative lift at 95% confidence, 80% power.

**Confidence level context:** 95% confidence / 80% power is the standard for high-stakes decisions (pricing changes, major redesigns). For lower-stakes exploratory tests (social creative, email subject lines), 90% confidence reduces required sample by ~25% and is often sufficient. For critical, hard-to-reverse decisions, consider 99% confidence — which roughly doubles the required sample. Define acceptable risk before choosing.

| Baseline Rate | 10% Lift | 20% Lift | 50% Lift | 100% Lift |
|--------------|----------|----------|----------|-----------|
| 1% | ~160,000 | ~40,000 | ~6,400 | ~1,600 |
| 2% | ~80,000 | ~20,000 | ~3,200 | ~800 |
| 5% | ~32,000 | ~8,000 | ~1,300 | ~320 |
| 10% | ~16,000 | ~4,000 | ~640 | ~160 |
| 25% | ~6,400 | ~1,600 | ~260 | ~64 |
| 50% | ~3,200 | ~800 | ~130 | ~32 |

## How to Use

1. Find your current conversion rate (baseline) in the left column
2. Decide the minimum lift worth detecting (be realistic — 50%+ lifts are rare)
3. Read the sample size needed per variant
4. Divide by your daily traffic to get test duration
5. If duration > 2 weeks, consider a larger change (to detect bigger lift) or use Before-After instead

## When You Don't Have Enough Traffic

If sample size requires > 2 weeks:

| Alternative | When to Use |
|------------|------------|
| **Before-After** | Can't split traffic, measure same metric before and after change |
| **Pilot** | Test on small segment, look for qualitative signal + directional data |
| **Bigger swing** | Instead of 10% lift, make a bold change that should produce 50%+ lift |
| **Different metric** | Use a higher-volume upstream metric (clicks instead of purchases) |

## Common Mistakes

1. **Ending early** — Don't stop when results "look good." Wait for full sample size.
2. **Peeking** — Checking results daily inflates false positive rate. Set a check date and wait. Why this matters: at 95% confidence, peeking daily can inflate your actual false positive rate to 25-30%. Each check is an independent statistical test — if you check 20 times, the probability of seeing at least one "significant" result by chance compounds (1 - 0.95^20 ≈ 64%). Use sequential testing methods (like Bayesian or alpha-spending) if you must monitor, or commit to a single check date and don't look until then.
3. **Too many variants** — Each variant needs full sample size. 4 variants = 4x the traffic.
4. **Ignoring segments** — Overall results may mask segment-level differences. Check major segments.
5. **Novelty effect** — New things get clicks initially. Run tests long enough to normalize.
6. **Missing guardrails** — A test can "succeed" on the primary metric while breaking something else (support tickets spike, error rate climbs, refund rate doubles). Always define guardrail metrics — things that must NOT get worse — before launching.
