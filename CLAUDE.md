# Research Skills

Structured frameworks: audience research, market analysis, problem diagnosis → solution design → target setting.

## Pipeline
icp-research → product-context.md (foundation)

market-research ──┐
                  ├→ solution-design → funnel-planner
problem-analysis ─┘

## Artifacts
Pipeline outputs write to `.agents/`; canonical audience/market records live in the top-level `research/` folder:
- `research/product-context.md` (cross-stack canonical record — created by icp-research, consumed by 12+ skills)
- `research/icp-research.md` (canonical audience record from icp-research)
- `research/market-research.md` (canonical market record from market-research)
- `.agents/problem-analysis.md`
- `.agents/solution-design.md`
- `.agents/targets.md`

## Cross-Stack (Optional)
All research skills can read `research/product-context.md` for business context.
Created by `icp-research` — run it first to establish the foundation artifact.

## Recommended Starting Point
Run `icp-research` first to create `research/product-context.md`, the canonical cross-stack artifact.

## Multi-Agent Skills

All 5 skills use a two-layer multi-agent orchestration pattern:

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
- `icp-research` — 7 agents (persona, voc-collector, habitat, pain-analysis, decision-psychology, synthesis, critic). Layer 1 parallel (persona + VoC + habitat) → Layer 2 sequential (pain→psychology→synthesis→critic).
- `market-research` — 7 agents (trends, sizing, competitor, consumer-landscape, cross-analysis, opportunity, critic). Layer 1 parallel (trends + sizing + competitor + consumer-landscape) → Layer 2 sequential (cross-analysis→opportunity→critic).
- `problem-analysis` — 6 agents (tree-builder, external-check, hypothesis, data-mapper, verdict, critic). Layer 1 parallel (tree-builder + external-check) → Layer 2 sequential (hypothesis→data-mapper→verdict→critic).
- `solution-design` — 7 agents (research, initiative-generator, unconventional, ranking, ice-scoring, cut-line, critic). Layer 1 (research) → Layer 1.5 parallel (initiative-generator + unconventional) → Layer 2 sequential (ranking→ice-scoring→cut-line→critic).
- `funnel-planner` — 6 agents (model-selection, baseline-collector, target-setter, sanity-check, stress-test, critic). Layer 1 parallel (model-selection + baseline-collector) → Layer 2 sequential (target-setter→sanity-check→stress-test→critic).
