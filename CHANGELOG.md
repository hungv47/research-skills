# Research Skills — Changelog

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versioning is [SemVer](https://semver.org/spec/v2.0.0.html) — major.minor.patch.

This file tracks stack-level releases. SKILL.md files describe current behavior; this file documents what changed and when.

---

## [2.0.0] - 2026-05-16

**Agent Skills 2.0 — fresh start.** The research-skills stack reset to 2.0.0 as part of the umbrella Agent Skills 2.0 release. Released as a pre-release tag on the `refactor/v2.0` branch. The `main` branch holds the legacy v1.x line for users who do not opt into the 2.0 trunk.

### Skills (8)

- `orchestrate-research` — router that reads project state and proposes the next skill with rationale
- `icp-research` — audience research (personas, VoC, habitat, pain analysis) → `research/icp-research.md` + `research/product-context.md`
- `market-research` — market landscape, competitive dynamics, TAM/SAM/SOM → `research/market-research.md`
- `diagnose` — problem-tree diagnosis (5-why + external check + hypothesis + verdict)
- `prioritize` — initiative generation + ICE scoring + cut-line + unconventional alternatives
- `funnel-planner` — funnel modeling + target setting + sanity check + stress test
- `short-form-research` — per-platform short-form video best-practice catalog (TikTok / Reels / Shorts default; +X / +LinkedIn opt-in)
- `short-form-eval` — post-publish short-form video evaluation (loop-native)

### Starting point

Run `icp-research` first to create `research/product-context.md` — the canonical cross-stack record consumed by 13+ downstream skills across marketing-skills + product-skills.

### Pipeline

```
icp-research → product-context.md (foundation)
market-research ──┐
                  ├→ prioritize → funnel-planner
diagnose ─┘
short-form-research → consumed by short-form-brief (marketing-skills)
short-form-eval → inside eval-loop, scores published shorts against research catalog
```
