---
name: start-research
description: "Stack orchestrator for research-skills. Reads what's already done in `research/` and `.agents/`, parses your intent, and proposes the next 1–3 skills in the research pipeline (icp-research → market-research / diagnose → prioritize → funnel-planner). Use when you don't know which research skill to invoke, or want a guided run through the full audience/market/strategy workflow. Not for executing the work itself — it routes to the skill that does. Not for cross-stack workflows (use start-meta or invoke skills directly)."
argument-hint: "[free-form ask, or empty to be guided]"
allowed-tools: Read Grep Glob Bash
user-invocable: true
license: MIT
metadata:
  author: hungv47
  version: "1.0.0"
  budget: standard
  estimated-cost: "$0.10-0.30"
promptSignals:
  phrases:
    - "where do i start with research"
    - "i want to research"
    - "help me plan research"
    - "what skill should i use for research"
    - "start research"
    - "begin research"
    - "research workflow"
  allOf:
    - [where, start, research]
    - [what, skill, research]
  anyOf:
    - "research workflow"
    - "research pipeline"
    - "guide me through research"
  noneOf:
    - "code review"
    - "marketing campaign"
    - "landing page"
  minScore: 5
routing:
  intent-tags:
    - research-orchestration
    - workflow-routing
    - stack-entry-point
    - research-guide
  position: orchestrator
  produces:
    - .agents/experience/research-workflow.md
  consumes:
    - research/product-context.md
    - research/icp-research.md
    - research/market-research.md
    - .agents/diagnose.md
    - .agents/prioritize.md
    - .agents/targets.md
    - .agents/experience/*.md
  requires: []
  defers-to:
    - skill: icp-research
      when: "no audience clarity yet — entry point of the research pipeline"
    - skill: market-research
      when: "audience clear, market landscape unclear"
    - skill: diagnose
      when: "known problem or metric decline to root-cause"
    - skill: prioritize
      when: "options landscape needs ranking"
    - skill: funnel-planner
      when: "revenue model needs numeric targets"
  parallel-with: []
  interactive: true
  estimated-complexity: low
---

# Start Research

*Meta — Stack orchestrator. The entry point for the research-skills stack when you don't know what to invoke.*

**Core Job:** read what's been done, infer where you are in the research pipeline, propose the next skill.

**Core Question:** "Given what's already in `research/` and `.agents/`, plus what you just asked, what's the highest-leverage research skill to run next?"

This skill does NOT execute research work. It is a router and progress-tracker. The actual research is done by the skill it routes you to (icp-research, market-research, diagnose, prioritize, funnel-planner).

---

## When To Use

- You just installed the research-skills plugin and don't know what to type.
- You're mid-project and forget which skill is next.
- You have a vague research need ("understand my market", "figure out who buys this", "why are conversions tanking") and want a guided routing.
- You want to resume a workflow across sessions — re-running `/start-research` re-reads state and resumes from the next step.

## When NOT To Use

- You already know which skill to run — just run it directly. This skill is overhead for users who know the stack.
- You want cross-stack guidance (e.g., "research → marketing → product"). Use `/start-meta` or compose conversationally.
- You want to execute research yourself rather than route. This skill only proposes and hands off.

---

## How It Works

1. **State detection** — silently read `research/`, `.agents/`, `.agents/experience/*.md` to build a picture of what's been done.
2. **Intention analysis** — parse the user's free-form ask (or, if empty, ask one scoping question). Map intent to a position in the research pipeline.
3. **Routing decision** — propose the next 1–3 skills with rationale. Show what each consumes and produces, expected cost, expected duration.
4. **User confirmation** — user picks one (or none, or a different skill). On confirmation, hand off — recommend the user invoke `/<skill-name>` with the appropriate args.

The skill never auto-invokes another skill. It always pauses for explicit user confirmation. This is the anti-runaway guard.

---

## Step 1: State Detection

Silently scan in this order. Do not announce findings until Step 4 — keep the read invisible.

| Path | What it tells you |
|---|---|
| `research/product-context.md` | ICP foundation exists. Audience is at least sketched. |
| `research/icp-research.md` | Full ICP research is done. |
| `research/market-research.md` | Market landscape mapped. |
| `.agents/diagnose.md` | A specific problem has been diagnosed. |
| `.agents/prioritize.md` | Initiative ranking exists. |
| `.agents/targets.md` | Funnel targets are set. |
| `.agents/experience/audience.md` | Cold-start audience answers persisted. |
| `.agents/experience/business.md` | Business model context persisted. |
| `.agents/experience/research-workflow.md` | Prior `/start-research` session left a breadcrumb. |

Build a **state map** — a small internal table:

```
icp-foundation:    done | partial | missing
market-landscape:  done | partial | missing
problem-diagnosis: done | partial | missing | n/a
prioritization:    done | partial | missing | n/a
funnel-targets:    done | partial | missing | n/a
```

**Stale check:** if an artifact is older than 90 days OR product-context.md mentions a different product than the current `CLAUDE.md` describes, mark it as stale and surface that in the proposal.

---

## Step 2: Intention Analysis

Parse the user's argument (the free-form ask). Match against intent buckets:

| User says | Intent | Pipeline position |
|---|---|---|
| "understand my customer", "who buys this", "personas", "voice of customer" | audience-research | icp-research |
| "market sizing", "TAM/SAM/SOM", "competitors", "market landscape", "whitespace" | market-mapping | market-research |
| "why is X dropping", "root cause", "metric decline", "diagnose this problem" | problem-diagnosis | diagnose |
| "what should we build", "prioritize features", "ICE score", "options ranking" | option-ranking | prioritize |
| "revenue targets", "funnel math", "how much traffic do I need", "unit economics" | target-setting | funnel-planner |

**If the ask is empty or ambiguous**, do NOT silently default to icp-research. Ask one bundled scoping question:

> "What are you trying to learn? Pick one or describe in your words:
>
> 1. Who's actually buying this / who should be (audience)
> 2. What the market and competitors look like (landscape)
> 3. Why a specific metric is moving the wrong way (diagnose)
> 4. Which initiative to bet on next (prioritize)
> 5. Whether the revenue math actually works (targets)"

Wait for answer. Do not proceed without one.

---

## Step 3: Routing Decision

Combine state map + intent. Apply routing rules in order — first match wins.

**Routing rules:**

1. **No icp foundation + any audience-or-strategy intent** → propose `icp-research` first. Rationale: 12+ skills downstream consume `research/product-context.md`. Skipping it produces hollow output.
2. **icp done + market intent** → propose `market-research`. Rationale: with audience defined, market landscape becomes targeted (which segment owns which competitor, etc.).
3. **icp done + diagnose intent** → propose `diagnose`. Note that `diagnose` requires a *specific* problem statement; if user says "things feel off," push back and ask for a metric.
4. **icp + (market or diagnose) done + prioritize intent** → propose `prioritize`. Rationale: it consumes both upstream artifacts.
5. **prioritize done + targets intent** → propose `funnel-planner`. Rationale: it consumes prioritize.md.
6. **icp done + user intent ambiguous** → propose **2 options** (market-research and diagnose) with one-line rationale each. Let user pick.
7. **No clear intent + everything done** → say so. "All five core artifacts exist and look fresh. You're past the standard research pipeline. Did you mean to refresh one, or do you need a different stack?" — suggest `/start-marketing` or `/start-product`.

**Edge cases:**
- **Stale icp** (older than 90 days, or product description in `CLAUDE.md` doesn't match): warn but don't force a rerun. "Your icp is 4 months old. Refresh it before market-research, or proceed with current?"
- **Wants to skip** ("I just want prioritize"): respect it. Run prioritize. But note in output: "Without market-research, prioritize will rely on whatever context you put in the prompt. Output quality drops accordingly."

---

## Step 4: Present + Confirm

Output format — keep it scannable, no walls of text:

```
## Where you are

- ICP foundation: ✅ done (research/icp-research.md, 2 weeks old)
- Market landscape: ❌ missing
- Problem diagnosis: ⊘ n/a (no specific problem in your ask)
- Prioritization: ❌ missing
- Funnel targets: ❌ missing

## What you asked

"I want to understand my market" → market-mapping intent.

## Recommended next: market-research

Why: ICP foundation is in place; market-research consumes it and produces
the landscape your prioritize step will need.

Cost: ~$1–3 · Duration: ~10 min · Produces: research/market-research.md

Run it?  →  /market-research
```

If multiple options apply (rule 6), show 2–3 with rationale per option, ask the user which.

---

## Step 5: Persist + Hand Off

After the user confirms, write a breadcrumb to `.agents/experience/research-workflow.md`:

```markdown
## Session 2026-05-06

- Read state: icp ✅, market ❌, diagnose ⊘, prioritize ❌, funnel ❌
- User intent: market-mapping
- Recommended: market-research
- User confirmed: yes
```

Keep this file append-only. Future `/start-research` invocations read it to detect "user already did X two sessions ago, don't re-propose it."

Then present the hand-off line:

> Run `/market-research` next. After it completes, re-run `/start-research` to plan the next step.

That's it. Skill exits. The user invokes the next skill themselves — this skill never auto-invokes.

---

## Pipeline Reference

For the canonical pipeline definition, decision rules, and per-skill catalog, see [`./references/workflow-graph.md`](./references/workflow-graph.md).

---

## Anti-Patterns

- **Don't summarize all 6 skills** in the output. Propose 1–3 specific ones for THIS user's state.
- **Don't auto-invoke** the recommended skill. The user must explicitly type `/skill-name`. This is the anti-runaway guard.
- **Don't re-derive the pipeline** from scratch each invocation. Read it from `references/workflow-graph.md`.
- **Don't lecture about the pipeline** — show the user only what's relevant to where they are.
- **Don't silently start over** when a workflow breadcrumb exists. Read it. Acknowledge it. Resume from there.

---

## Output

- **Inline only** — this skill prints to the conversation, doesn't produce a saved artifact.
- **Side effect:** appends one entry to `.agents/experience/research-workflow.md`.

## Status

Always ends with one of:
- `DONE` — recommendation given, user confirmed, hand-off line printed.
- `BLOCKED` — couldn't read state (no project context, no `CLAUDE.md`, no `.agents/`). Ask user where the project lives.
- `NEEDS_CONTEXT` — user's ask was empty AND no state exists. Ask the scoping question, return after answer.
