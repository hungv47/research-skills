# Strategy Skills

Structured frameworks for market research, problem diagnosis, solution design, target setting, and experimentation. Domain-agnostic — works for marketing, product, engineering, or operational challenges.

## Installation

```bash
npx skills add hungv47/strategy-skills
```

## Skills

| Skill | Description |
|-------|-------------|
| `market-research` | Map market landscape, competitive dynamics, and identify gaps and opportunities |
| `problem-analysis` | Structured problem diagnosis, hypothesis development, and root cause analysis |
| `solution-design` | Brainstorm targeted solutions and rank with evidence-backed ICE scoring |
| `funnel-planner` | Set measurable targets with benchmarks and unit economics |
| `experiment` | Design minimum viable tests with clear decision rules |

## Pipeline

```
market-research ──┐
                  ├→ solution-design → funnel-planner → experiment
problem-analysis ─┘
```

Artifacts save to `.agents/`.

## Cross-Stack DAG

```
strategy: market-research ─┐
                           ├→ solution-design → funnel-planner → experiment
          problem-analysis ─┘
                                    ↓                  ↓
comms:    icp-research → imc-plan → content-create → attribution
               ↓              ↕ (reads solution-design, targets)
design:   brand-system → user-flow
                              ↓
prod:     plan-interviewer → system-architecture → task-breakdown
          code-cleanup (standalone)    technical-writer (standalone)
```

`product-description` or `icp-research` creates `.agents/product-context.md`, consumed by 12+ skills across all stacks.

## Cross-Stack Workflow

All strategy skills can read `.agents/product-context.md`, created by `product-description` or `icp-research` from [comms-skills](https://github.com/hungv47/comms-skills).

## Usage

- "Research this market" → `market-research`
- "Who are the competitors" → `market-research`
- "Where are the gaps" → `market-research`
- "Diagnose the problem" → `problem-analysis`
- "What's causing this?" → `problem-analysis`
- "Form hypotheses" → `problem-analysis`
- "Find root cause" → `problem-analysis`
- "Brainstorm solutions" → `solution-design`
- "Prioritize initiatives" → `solution-design`
- "Set targets" → `funnel-planner`
- "Design an experiment" → `experiment`
## License

MIT
