# Research Skills — Changelog

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versioning is [SemVer](https://semver.org/spec/v2.0.0.html) — major.minor.patch.

This file tracks stack-level releases. SKILL.md files describe current behavior; this file documents what changed and when.

---

## [5.0.0] - 2026-05-12

Stack-major cut coordinated across the 4-stack marketplace to mark the post-tier-discipline stable era. Funnel-planning gets a new default route shape; the stack-orchestrator declares itself fast-tier. No skill removed or renamed; no API breaking change. Major bump signals the alignment, not breakage.

### Changed
- `funnel-planner` defaults to Route B (`target-setter → sanity-check → critic`, 3-agent). Route A (full 6-agent flow) reserved for `--deep` flag or 3+ initiatives spanning 2+ funnel models.
- `funnel-planner` budget reclassified deep → standard, estimated cost $1-3 → $0.30-0.80.
- `orchestrate-research` budget reclassified standard → fast; body declares it is a pure router (no agent dispatch, no critic gate).

### Added
- `funnel-planner` Route C: single-pass bump-update path for ≤3-sentence asks with a single initiative and a prior `targets-*.md` on disk. Falls back to Route B if any condition fails.

Full review: `.agents/skill-artifacts/meta/records/2026-05-12-fresh-eyes-tier-discipline-phase-ab.md`

---

## [3.0.3] - 2026-05-11

`orchestrate-research` Step 1 starts from concrete disk state instead of asking the model to derive it. When you run `/orchestrate-research`, the skill now sees the actual artifact counts by domain, which top-level canonical folders exist (`research/`, `brand/`, `architecture/`), and the last 5 commits — all rendered inline before the manifest read kicks in.

### Changed
- **`skills/orchestrate-research/SKILL.md` §Step 1** — disk-snapshot block lifted from `orchestrate-meta`. Three `! \`<cmd>\`` interpolations (artifact-count-by-domain / canonical-folder check / git-log -5) substitute their output at slash-command invocation time, before the manifest read. The orchestrator starts from a deterministic snapshot instead of speculating about what's on disk.

### Notes
- Additive context. No behavioral change to routing, recommendations, or output schema. Existing invocations work unchanged.
- The block only renders when the skill is invoked as a slash command. If `SKILL.md` is read via the Read tool inside another skill, the bang-backtick lines pass through as literal syntax — by design.

---

## [3.0.2] - 2026-05-11

`short-form-research/pattern-extractor-agent.md` gains a clip-density characterization pass. Sourced from Oren John's clipping-and-live ecosystem breakdown (2026-03-17). Closes the upstream half of the format-fit critic loop shipped in 3.0.1 + marketing-skills 4.0.2.

### Changed
- **`short-form-research/agents/pattern-extractor-agent.md`** — added a **clip-density characterization** technique parallel to the existing synthesis-heuristic check. Synthesis asks where a format *came from*; clip-density asks what a format *pays out* — how many quotable moments per minute of source. For every archetype crossing the ≥3-occurrence threshold, the extractor now tags `[CLIP-DENSITY: high | med | low]` based on three structural triggers (every-interaction-quotable / multi-cut-point-long-form / single-moment-or-none). Insufficient source-length to judge tags `[CLIP-DENSITY: insufficient-source-length-to-judge]` rather than guessing. Two new anti-patterns: over-tagging-high (treating any conversational archetype as high-density), and single-outlier-driven judgments (clip-density is a structural property of the format, not an artifact of one strong moment).

### Why it matters downstream
The tag is consumed by `marketing-skills/short-form-brief`'s format-fit critic — high-density archetypes are format-fit candidates for campaigns running a paid-CPM clipping distribution layer; low-density archetypes get flagged as "produces views but won't survive clipping bounty network" so the brief skill can route around the format if clipping is the planned distribution. Cross-stack companion: `marketing-skills@4.1.2` ships the `campaign-plan/references/distribution-models/clipping-and-live.md` reference that this tag is scored against.

### Notes
- Additive technique to an existing agent. No contract change for synthesis-agent or critic-agent downstream consumers; tags are purely additive metadata in the existing per-platform Hook Archetypes section.

---

## [3.0.1] - 2026-05-10

`short-form-research/pattern-extractor-agent.md` gains a synthesis-heuristic detection pass. Sourced from Jibran's 60M-views / #1-AppStore-in-3-days breakdown.

### Changed
- **`short-form-research/agents/pattern-extractor-agent.md`** — added a **synthesis-heuristic detection** technique. Jibran's rule: viral formats are usually a synthesis of a *current* trend with a *previous* viral format from a different category, era, or product. For every archetype crossing the ≥3-occurrence threshold, the extractor now runs three checks — cross-category synthesis, cross-era synthesis (1–3 years prior), or combined-format synthesis. Candidates are tagged `[SYNTHESIS-CANDIDATE: cross-category | cross-era | combined]` for the downstream synthesis-agent's cross-platform table; insufficient cross-context exposure tags `[SYNTHESIS-CHECK: insufficient cross-context — flag for human]` rather than fabricating a source. New anti-patterns: fabricated synthesis attribution, over-tagging.

### Notes
- The companion copywriting reference enrichment ships in `marketing-skills@4.0.2` — separate stack.
- Additive technique to an existing agent. No contract change for synthesis-agent or critic-agent downstream consumers; tags are purely additive metadata in the existing per-platform Hook Archetypes section.

---

## [3.0.0] - 2026-05-08

### BREAKING
- Renamed `start-research` → `orchestrate-research`. The skill scans existing artifacts and continues mid-pipeline; the orchestration role now reads explicitly in the slash command. No backward-compat alias — single-rev cutover.
- Update any `/start-research` invocations in your workflows to `/orchestrate-research`.

---

## [2.3.1] - 2026-05-08

CLAUDE.md doc cleanup — align stack-level documentation with the new `.agents/skill-artifacts/` taxonomy shipped in v2.3.0 and across the umbrella as marketplace 1.5.0.

### Changed

- `research-skills/CLAUDE.md` Pipeline and Artifacts sections — every `.agents/diagnose.md`, `.agents/prioritize.md`, `.agents/targets.md`, `.agents/mkt/short-form-research.md` reference migrated to the new taxonomy:
  - `.agents/diagnose.md` → `.agents/skill-artifacts/meta/records/diagnose-*.md`
  - `.agents/prioritize.md` → `.agents/skill-artifacts/meta/sketches/prioritize-*.md`
  - `.agents/targets.md` → `.agents/skill-artifacts/meta/records/targets-*.md`
  - `.agents/mkt/short-form-research.md` → `.agents/skill-artifacts/research/short-form-research.md` (cross-stack relocation: short-form-research is a research-skill, output now lives under `research/` not `mkt/`)
- "Pipeline outputs write to `.agents/`" → "Pipeline outputs write under `.agents/skill-artifacts/`."

### Notes

Doc-only patch — no SKILL.md or skill-behavior changes.

---

## [2.3.0] - 2026-05-08

`short-form-eval` scaffold + T33 path migration.

### Added

- `short-form-eval` — Closes the feedback loop for short-form video. Scores published posts against the original brief and platform-intelligence references; produces signal-bearing pattern-log entries the gap-gate consumes. Scaffolded per panel verdict 2026-05-08 (manufactured-calibration approach — see `.agents/skill-artifacts/meta/decisions/2026-05-08-agents-panel-content-stack-direction.md`). 4 agents (eval-runner, hook-strength, pattern-extractor, critic) across 2 layers; standard budget; ~$0.50–1.50 per cycle. Provisional v0.1 rubric in `references/rubric.md`, **mandatory revision after cycle 2-3** against real variance — explicitly revisable, not locked. Cycle 1 weighting is 70% observation / 30% scoring to avoid overfitting on a single calibration pair.

### Changed

- T33 path migration: SKILL.md output declarations across `short-form-research`, `start-research`, `diagnose`, `prioritize`, `funnel-planner` updated to the new `.agents/skill-artifacts/...` taxonomy (canonical lifecycle taxonomy from `agent-skills/CLAUDE.md` §"Artifact Placement"). `lifecycle:` added to routing blocks: `pipeline` (short-form-research, short-form-eval, start-research), `snapshot` (diagnose, funnel-planner), `sketch` (prioritize). Body references and `produces:` paths aligned. Inline references to `.agents/diagnose.md`, `.agents/prioritize.md`, `.agents/targets.md` in `start-research`'s state-detection table updated to their new `.agents/skill-artifacts/meta/...` equivalents. `market-research` body line updated alongside.
- `market-research` and `icp-research` are canonical (top-level `research/...` paths) — `lifecycle: canonical` added to routing blocks, paths unchanged. `produces:` fields normalized to full `research/<file>.md` paths for clarity.

### Notes

The provisional rubric is the load-bearing design choice. Premature-rubric-lock was the panel's highest residual risk; mitigation is procedural — friction goes in the cycle-report Friction Notes channel, and the operator runs the revision pass at the cycle 2-3 boundary. Skill scaffolds and reports; it does not loop cycles autonomously. T27 (cycle 1 run) is operator-gated on T25 (calibration material decision).

---

## [2.2.0] - 2026-05-07

Manifest-aware state detection in `start-research`.

### Changed

- `start-research` SKILL.md — Step 1 (State Detection) now reads `.agents/manifest.json` first with a status-aware lookup table (`done`, `done_with_concerns`, `blocked`/`needs_context`, `stale`, `frontmatter_present: false`). Per-artifact staleness now flows from the manifest's `stale_after_days` field rather than a hard-coded 90-day check. Per-path filesystem scan demoted to fallback for fresh projects. Anti-pattern entry added: "Don't ignore the manifest." Added `side-effects: [manifest-sync]` to the skill's routing block.
- `CLAUDE.md` — added "Manifest Spec" section pointing producer skills (icp-research, market-research, diagnose, prioritize, funnel-planner, short-form-research) at the canonical contract in `meta-skills/references/manifest-spec.md` and the frontmatter obligations.

### Notes

This release lands the manifest-spec contract on the consumer side. Per-skill frontmatter retrofit (icp-research, market-research, etc.) follows in a later release — the spec's graceful fallback (`frontmatter_present: false`) means existing artifacts keep working until producers are migrated.

---

## [2.1.0] - 2026-05-06

Stack orchestrator added; declaration drift fixed.

### Added

- `start-research` — Stack orchestrator. Reads `research/`, `.agents/`, and `.agents/experience/*.md`, parses the user's free-form ask (or asks one bundled scoping question if empty), and proposes the next 1–3 skills in the research pipeline (`icp-research` → `market-research` / `diagnose` → `prioritize` → `funnel-planner`) with rationale + cost + duration. Never auto-invokes — always prints the `/skill-name` for the user to type. Persists a breadcrumb to `.agents/experience/research-workflow.md` so re-entry across sessions resumes the workflow. Standard budget, ~$0.10–0.30 per run. Pipeline catalog lives in `references/workflow-graph.md`.

### Fixed

- `short-form-research` was present on disk since v2.0.0 but missing from `.claude-plugin/plugin.json` `skills[]` — declaration restored. Skill now installs correctly via the Claude Code plugin marketplace path.

---

## [1.0.0] - 2026-05-05

Initial public release. Structured frameworks for audience research, market analysis, problem diagnosis → solution design → target setting.

### Added

**Skills (5)**

- `icp-research` — Builds ideal customer profiles + buyer personas. Auto-scans README/marketing/pricing first, interviews for gaps. Produces `research/product-context.md` (canonical cross-stack record consumed by 12+ downstream skills) and `research/icp-research.md` (audience profile with personas, VoC quotes, pain analysis, decision psychology, habitats). 7 agents across 2 layers; Quick ICP (Route A, single persona) or Full ICP (Route B, comprehensive).
- `market-research` — Analyzes market landscapes, competitive dynamics, TAM/SAM/SOM sizing, whitespace opportunities. Produces `research/market-research.md`. 7 agents across 2 layers (trends + sizing + competitor + consumer-landscape parallel; cross-analysis → opportunity → critic sequential).
- `diagnose` — Traces metric gaps to root causes via MECE logic tree, hypothesis validation, gap percentages. Produces `.agents/diagnose.md`. 6 agents; always cold-starts (skill IS the diagnostic interview).
- `prioritize` — Force-ranks initiatives with ICE scoring, kill criteria, ≤3 cut-line. Produces `.agents/prioritize.md`. **Hard-gated on `.agents/diagnose.md`** — no INTERVIEW substitute (initiative ranking only makes sense against validated root cause). 7 agents.
- `funnel-planner` — Sets numeric targets per prioritized initiative with improvement factors, kill criteria, stress tests. Produces `.agents/targets.md`. **Hard-gated on `.agents/prioritize.md`** — targets without ranked initiatives are arbitrary. 6 agents.

**Pipeline contract**

```
icp-research → product-context.md (foundation, 12+ consumers)
                   ↓
            ┌──────┴──────┐
            ↓             ↓
   market-research    diagnose ─→ prioritize ─→ funnel-planner
```

**Architectural patterns**

- **Pre-Dispatch protocol** — every skill follows the canonical spec at `meta-skills/references/pre-dispatch-protocol.md`. Cold Start / Warm Start flows; answers persist to `.agents/experience/{domain}.md`. Hard-gated skills skip cold-start — recommend upstream when gate fails.
- **Status protocol** — every skill emits `DONE / DONE_WITH_CONCERNS / BLOCKED / NEEDS_CONTEXT` with skill-specific exit conditions; artifact frontmatter mirrors.
- **Multi-agent orchestration** — Layer 1 (parallel) → Layer 2 (sequential) → Critic gate (PASS/FAIL with max 2 rewrite cycles).

**Cross-stack**

- `research/product-context.md` is the canonical cross-stack record — read by brand-system, copywriting, campaign-plan, lp-brief, lp-optimization, design-brief, system-architecture, user-flow, docs-writing, humanize, vn-tone, seo, cold-outreach.
- `.agents/prioritize.md` informs `system-architecture` (initiatives drive what to build).
- `.agents/diagnose.md` informs `market-research` (focus analysis on relevant market dimensions).
