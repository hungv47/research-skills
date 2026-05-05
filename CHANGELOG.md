# Research Skills — Changelog

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versioning is [SemVer](https://semver.org/spec/v2.0.0.html) — major.minor.patch.

This file tracks stack-level releases. SKILL.md files describe current behavior; this file documents what changed and when.

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
