# Research Skills

Structured frameworks for audience research, market analysis, problem diagnosis, solution design, target setting, and experimentation. Domain-agnostic — works for marketing, product, engineering, or operational challenges.

## Installation

```bash
npx skills add hungv47/research-skills
```

## Skills

| Skill | Description |
|-------|-------------|
| `icp-research` | Deep audience research and Ideal Customer Profile development |
| `market-research` | Map market landscape, competitive dynamics, and identify gaps and opportunities |
| `problem-analysis` | Structured problem diagnosis, hypothesis development, and root cause analysis |
| `solution-design` | Brainstorm targeted solutions and rank with evidence-backed ICE scoring |
| `funnel-planner` | Set measurable targets with benchmarks and unit economics |
| `experiment` | Design minimum viable tests with clear decision rules |

## Pipeline

```
icp-research → product-context.md (foundation for 12+ downstream skills)

market-research ──┐
                  ├→ solution-design → funnel-planner → experiment
problem-analysis ─┘
```

Artifacts save to `.agents/`.

## Cross-Stack DAG

```
research:   icp-research → product-context.md
            market-research ─┐
            problem-analysis ─┤→ solution-design → funnel-planner → experiment
                              │
marketing:  brand-system ─────┤
            imc-plan ←────────┘→ content-create → attribution
            copywriting, lp-optimization, seo, humanize (horizontal)

product:    user-flow → system-architecture → (execution)
            code-cleanup, technical-writer (horizontal)

meta:       preflight → plan-interviewer → task-breakdown (before builds)
            multi-lens (decisions), review-chain (verification)
            artifact-status, skill-router (navigation)
```

`icp-research` creates `.agents/product-context.md`, consumed by 12+ skills across all stacks.

## Cross-Stack Workflow

All research skills can read `.agents/product-context.md`, created by `icp-research`.

## Usage

- "Research my audience" → `icp-research`
- "Research this market" → `market-research`
- "Who are the competitors" → `market-research`
- "Diagnose the problem" → `problem-analysis`
- "What's causing this?" → `problem-analysis`
- "Brainstorm solutions" → `solution-design`
- "Prioritize initiatives" → `solution-design`
- "Set targets" → `funnel-planner`
- "Design an experiment" → `experiment`

## License

MIT
