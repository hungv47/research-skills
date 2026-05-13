# Research Skills

Structured frameworks: audience research, market analysis, problem diagnosis → solution design → target setting.

## Pipeline
icp-research → product-context.md (foundation)

market-research ──┐
                  ├→ prioritize → funnel-planner
diagnose ─┘

short-form-research → skills-resources/research/short-form-research.md (consumed by short-form-brief in marketing-skills)

## Artifacts
Pipeline outputs write under `skills-resources/`; canonical audience/market records live in the top-level `research/` folder:
- `research/product-context.md` (cross-stack canonical record — created by icp-research, consumed by 13+ skills)
- `research/icp-research.md` (canonical audience record from icp-research)
- `research/market-research.md` (canonical market record from market-research)
- `skills-resources/meta/records/diagnose-*.md`
- `skills-resources/meta/sketches/prioritize-*.md`
- `skills-resources/meta/records/targets-*.md`
- `skills-resources/research/short-form-research.md` (per-platform best-practice catalog — pipeline output, consumed by short-form-brief)

## Cross-Stack (Optional)
All research skills can read `research/product-context.md` for business context.
Created by `icp-research` — run it first to establish the foundation artifact.

## Recommended Starting Point
Run `icp-research` first to create `research/product-context.md`, the canonical cross-stack artifact.

## Pre-Dispatch Protocol

All 6 skills follow the canonical Pre-Dispatch protocol (`meta-skills/references/pre-dispatch-protocol.md`). Cold Start (3-5 bundled questions, one round-trip) when context is missing; Warm Start (summary + optional probe) when artifacts/experience cover what's needed. Answers persist to `skills-resources/experience/{domain}.md` so subsequent skills never re-ask. `prioritize` and `funnel-planner` are hard-gated — no cold-start; recommend upstream (diagnose / prioritize) when gate fails. `short-form-research` writes to `skills-resources/experience/content.md`.

## Complexity Routing

Every skill declares a `budget` tier in frontmatter: `fast`, `standard`, or `deep`. The harness reads the tier and adjusts execution before dispatch:

| Budget | Execution |
|--------|-----------|
| **fast** | Single-agent, no sub-agent dispatch, no critic gate. Respond directly. |
| **standard** | Reduced orchestration — essential agents only, one critic pass. |
| **deep** | Full orchestration as documented — all agents, all layers, full critic gate. |

**Auto-downgrade** (before dispatch): ≤3 sentences AND no prior artifacts AND not deep → fast; single-topic clear-scope → cap at standard; multi-artifact / cross-domain / ambiguous → full tier.

**Override — bidirectional.** Auto-downgrade is heuristic; operator intent wins.

- **Upward (force deeper):** "run this thoroughly", "full analysis", "deep mode" → use the documented tier even on small inputs.
- **Downward (`--fast`):** `--fast` flag on the slash command, OR phrases "fast mode" / "quick pass" / "skip the orchestration" in the same turn → force single-agent execution regardless of tier. No sub-agents, no critic gate, no rewrite loops, no warm-start Pre-Dispatch interrogation. Skill produces its core deliverable in one pass and ends with "Ran in --fast mode; rerun without the flag for full critique."

**`--fast` does NOT skip Cold Start.** When no context is resolvable from artifacts or `skills-resources/experience/`, the skill still asks its bundled cold-start questions. `--fast` only bypasses multi-agent orchestration *after* context is resolved — it does not authorize hallucinating against missing audience/business/brand decisions.

**Safety gates supersede `--fast`.** Hard-gated skills (mandatory Pre-Dispatch hard blocks — `prioritize` and `funnel-planner` are hard-gated per the Pre-Dispatch section above) enforce gates regardless of `--fast`. The contract is "skip the heavy lift, not the guardrails."

Conflict rules: `--fast` on a `fast`-tier skill is a no-op. `--fast` + "run thoroughly" → `--fast` wins (explicit flag > upward phrase). `--fast` + `--deep` → `--fast` wins (downward bias on conflicting explicit flags). Budget is the default — never a ceiling, never a floor.

**Stack-specific tail:** `funnel-planner` has a built-in **route-level** auto-downgrade to Route C for trivially-shaped single-initiative bumps even without `--fast`. This is route selection (independent of the tier-level override above) — see its SKILL.md "Routing Logic" section.

## Manifest Spec

State detection across all research skills (especially `orchestrate-research`) reads `skills-resources/manifest.json` — a derived index of artifact metadata (producer, date, status, schema version, staleness, summary). The manifest is rebuilt from artifact frontmatter by `meta-skills/scripts/manifest-sync.ts`; skills don't write to it directly. See [`../meta-skills/references/manifest-spec.md`](../meta-skills/references/manifest-spec.md) for the full contract. Skills that produce artifacts (icp-research, market-research, diagnose, prioritize, funnel-planner, short-form-research) must write the required frontmatter fields (`skill`, `version`, `date`, `status`) and call sync as their last step.

## Multi-Agent Skills

All 6 skills use a two-layer multi-agent orchestration pattern:

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
- `diagnose` — 6 agents (tree-builder, external-check, hypothesis, data-mapper, verdict, critic). Layer 1 parallel (tree-builder + external-check) → Layer 2 sequential (hypothesis→data-mapper→verdict→critic).
- `prioritize` — 7 agents (research, initiative-generator, unconventional, ranking, ice-scoring, cut-line, critic). Layer 1 (research) → Layer 1.5 parallel (initiative-generator + unconventional) → Layer 2 sequential (ranking→ice-scoring→cut-line→critic).
- `funnel-planner` — 6 agents (model-selection, baseline-collector, target-setter, sanity-check, stress-test, critic). Layer 1 parallel (model-selection + baseline-collector) → Layer 2 sequential (target-setter→sanity-check→stress-test→critic).
- `short-form-research` — 6 agents (platform-scout × N parallel, audience-fit, pattern-extractor, audio-trend conditional, synthesis, critic). Layer 1 parallel (platform-scout × N + audience-fit) → Layer 2 sequential (pattern-extractor→audio-trend→synthesis→critic). Default 3 platforms (TikTok+Reels+Shorts), max 5 with `--all`. Single-market per artifact.
