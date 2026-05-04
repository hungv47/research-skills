# Research Skills

Structured frameworks for audience research, market analysis, problem diagnosis, solution design, and target setting. 5 skills.

## Install

Installs via the [`skills` CLI](https://skills.sh). Requires Node.js 18+. Auto-detects Claude Code, Cursor, Codex, Windsurf, Gemini CLI, or VS Code.

```bash
# Install the full research stack
npx skills add hungv47/research-skills

# Cherry-pick a single skill (any skill in the stack — these are just examples)
npx skills add hungv47/research-skills --skill icp-research
npx skills add hungv47/research-skills --skill problem-analysis

# List available skills without installing
npx skills add hungv47/research-skills --list

# Target a specific editor
npx skills add hungv47/research-skills --agent claude-code

# Install globally (available in every project)
npx skills add hungv47/research-skills -g
```

See the [root README](https://github.com/hungv47/agent-skills#install) for the full install reference.

### Alternative: Claude Code plugin

For Claude Code users who prefer the native plugin system:

```
/plugin marketplace add hungv47/agent-skills
/plugin install research-skills@agent-skills
```

Skills are then namespaced — call them as `/research-skills:icp-research`, `/research-skills:market-research`, etc. **`npx skills add` is recommended for most users** (editor-agnostic, no namespace prefix, per-skill cherry-pick). Plugin path is Claude Code only.

## Pipeline

```
icp-research → market-research + problem-analysis → solution-design → funnel-planner
```

## Skills

### `icp-research` — build ideal customer profiles

Analyzes demographics, pain points, jobs-to-be-done, and market segmentation to build detailed buyer personas.

**Use when:**
- You're entering a new market and need to understand who you're building for
- You want to ground all downstream skills in real audience data
- You need the `product-context.md` artifact that 12+ skills depend on

**Not for:** competitive positioning (use `solution-design`) or campaign planning (use `imc-plan`)

**Produces:** `research/product-context.md` (cross-stack), `research/icp-research.md`

---

### `market-research` — map the competitive landscape

Analyzes competitive dynamics, TAM/SAM/SOM sizing, whitespace opportunities, and market trends.

**Use when:**
- You need to size a market opportunity before committing resources
- You want to understand who the competitors are and where the gaps exist
- You're evaluating whether to enter a market

**Not for:** audience personas (use `icp-research`) or diagnosing a metric decline (use `problem-analysis`)

**Produces:** `research/market-research.md`

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

**Not for:** validating ideas with experiments (use `experiment` if you have one) — funnel-planner sets the math, doesn't measure it

**Produces:** `.agents/targets.md`

---

## Cross-Stack

All research skills can read `research/product-context.md`, the foundation artifact created by `icp-research`. Run it first to establish context for all downstream skills.

Research artifacts feed into marketing (`imc-plan`, `lp-brief`, `copywriting`) and product (`system-architecture`) stacks.

## License

MIT
