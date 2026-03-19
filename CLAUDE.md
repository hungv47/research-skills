# Strategy Skills

Structured frameworks: market research, problem diagnosis → solution design → target setting → experimentation.

## Pipeline
market-research ──┐
                  ├→ solution-design → funnel-planner → experiment
problem-analysis ─┘

## Artifacts
Skills write to `.agents/`:
- `.agents/market-research.md`
- `.agents/problem-analysis.md`
- `.agents/solution-design.md`
- `.agents/targets.md`
- `.agents/experiment-[name].md`

## Cross-Stack (Optional)
All strategy skills can read `.agents/product-context.md` for business context.
Created by `product-description` or `icp-research` from the communication stack:
`npx skills add hungv47/comms-skills`

## Recommended Starting Point
Run `product-description` or `icp-research` (from comms-skills) first to create `.agents/product-context.md` — the canonical cross-stack artifact.