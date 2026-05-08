# Research Skills — Changelog

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versioning is [SemVer](https://semver.org/spec/v2.0.0.html) — major.minor.patch.

This file tracks stack-level releases. SKILL.md files describe current behavior; this file documents what changed and when.

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
