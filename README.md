# Research Skills

Structured frameworks for audience research, market analysis, problem diagnosis, solution design, target setting, and experimentation.

## Install

```bash
npx skills add hungv47/research-skills
```

## Pipeline

<picture>
  <img src="./assets/pipeline.svg" alt="Research pipeline: icp-research → market-research + problem-analysis → solution-design → funnel-planner → experiment" width="100%">
</picture>

## Skills

### `icp-research` — build ideal customer profiles

Analyzes demographics, pain points, jobs-to-be-done, and market segmentation to build detailed buyer personas.

**Use when:**
- You're entering a new market and need to understand who you're building for
- You want to ground all downstream skills in real audience data
- You need the `product-context.md` artifact that 12+ skills depend on

**Not for:** competitive positioning (use `solution-design`) or campaign planning (use `imc-plan`)

**Produces:** `.agents/product-context.md` (cross-stack), `.agents/mkt/icp-research.md`

---

### `market-research` — map the competitive landscape

Analyzes competitive dynamics, TAM/SAM/SOM sizing, whitespace opportunities, and market trends.

**Use when:**
- You need to size a market opportunity before committing resources
- You want to understand who the competitors are and where the gaps exist
- You're evaluating whether to enter a market

**Not for:** audience personas (use `icp-research`) or diagnosing a metric decline (use `problem-analysis`)

**Produces:** `.agents/market-research.md`

---

### `problem-analysis` — diagnose root causes

Builds logic trees, forms testable hypotheses, maps data requirements, and identifies root causes with evidence.

**Use when:**
- A key metric dropped and you need to figure out *why*
- You're facing a complex problem and want structured diagnosis before jumping to solutions
- You need to distinguish symptoms from causes

**Not for:** code bugs (use `code-cleanup`) or brainstorming solutions to a known problem (use `solution-design`)

**Produces:** `.agents/problem-analysis.md`

---

### `solution-design` — prioritize what to build

Generates strategic options, scores trade-offs with evidence-backed ICE scoring, and recommends a path forward.

**Use when:**
- The problem is already clear and you need to decide *what* to pursue
- You have multiple possible directions and need a structured way to choose
- You want initiatives ranked by impact, confidence, and effort

**Not for:** diagnosing what the problem is (use `problem-analysis`) or engineering task lists (use `task-breakdown`)

**Produces:** `.agents/solution-design.md`

---

### `funnel-planner` — set numeric targets

Models business funnels backward from revenue goals to required traffic, conversion rates, and unit economics. Supports PLG and SLG growth motions.

**Use when:**
- You need to answer "how much traffic / how many leads do we need to hit $X?"
- You're setting quarterly targets with realistic assumptions
- You want to stress-test whether your growth model actually works

**Not for:** mapping activities to existing KPIs (use `attribution`) or testing specific variants (use `experiment`)

**Produces:** `.agents/targets.md`

---

### `experiment` — design validation tests

Designs minimum viable tests with hypotheses, success metrics, sample sizes, and clear decision rules.

**Use when:**
- You have a strategic initiative and want to validate before full commitment
- You need a test design with proper sample sizing and guardrail metrics
- You want clear "ship / kill / iterate" decision criteria before running the test

**Not for:** strategic prioritization (use `solution-design`) or market sizing (use `market-research`)

**Produces:** `.agents/experiment-[name].md`

---

## Cross-Stack

All research skills can read `.agents/product-context.md`, the foundation artifact created by `icp-research`. Run it first to establish context for all downstream skills.

Research artifacts feed into marketing (`imc-plan`, `attribution`) and product (`system-architecture`) stacks.

## License

MIT
