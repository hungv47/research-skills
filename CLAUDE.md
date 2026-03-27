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
Created by `icp-research` from the communication stack:
`npx skills add hungv47/comms-skills`

## Recommended Starting Point
Run `icp-research` (from comms-skills) first to create `.agents/product-context.md`, the canonical cross-stack artifact.

## Multi-Agent Skills

Some skills use a two-layer multi-agent orchestration pattern:

- `SKILL.md` = **orchestrator** — dispatch graph, routing logic, merge step, critic gate
- `agents/` = **sub-agent instruction files** — each with role, input/output contracts, domain knowledge, self-check
- `references/` = **shared data catalogs** — formula lists, template libraries read by multiple agents

### How it works
1. Orchestrator gathers context (problem statement, artifacts, brief)
2. **Layer 1** agents run in parallel — each produces one independent output
3. Orchestrator merges outputs
4. **Layer 2** agents run sequentially — each builds on the previous agent's output
5. **Critic agent** scores the output and returns PASS or FAIL (max 2 rewrite cycles)

### Skills using this pattern
- `market-research` — 7 agents (trends, sizing, competitor, consumer-landscape, cross-analysis, opportunity, critic). Layer 1 parallel (trends + sizing + competitor + consumer-landscape) → Layer 2 sequential (cross-analysis→opportunity→critic). Three routing modes: Quick (2 L1 agents), Positioning (all 4 L1), Fundraising (all 4 L1 + enhanced depth).
- `problem-analysis` — 6 agents (tree-builder, external-check, hypothesis, data-mapper, verdict, critic). Layer 1 parallel (tree-builder + external-check) → Layer 2 sequential (hypothesis→data-mapper→verdict→critic). Includes a Data Gathering Pause between data-mapper and verdict where the user provides evidence.
