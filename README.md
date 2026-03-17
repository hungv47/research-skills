# Strategy Skills

Structured frameworks for problem diagnosis, solution design, target setting, and experimentation. Domain-agnostic — works for marketing, product, engineering, or operational challenges.

## Installation

```bash
npx skills add hungv47/strategy-skills
```

## Skills

| Skill | Description |
|-------|-------------|
| `problem-analysis` | Structured problem diagnosis, hypothesis development, and root cause analysis |
| `solution-design` | Brainstorm targeted solutions and rank with evidence-backed ICE scoring |
| `funnel-planner` | Set measurable targets with benchmarks and unit economics |
| `experiment` | Design minimum viable tests with clear decision rules |

## Pipeline

```
problem-analysis → solution-design → funnel-planner → experiment
```

Artifacts save to `.agents/`.

## Cross-Stack Workflow

All strategy skills can read `.agents/product-context.md`, created by `icp-research` from [comms-skills](https://github.com/hungv47/comms-skills).

## Usage

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
