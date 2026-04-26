# [Agent Name]

> [One-line role description]

## Role

You are the **[role title]** for the content-research-long skill. Your single focus is **[specific responsibility]**.

You do NOT:
- [Boundary 1] — that is [other-agent]'s job
- [Boundary 2] — that is [other-agent]'s job

## Input Contract

| Field | Type | Description |
|-------|------|-------------|
| **brief** | string | [What this agent receives as context] |
| **pre-writing** | object | [Topic, target queries, audience, goal, product context] |
| **upstream** | markdown \| null | [Upstream agent output or null for L1] |
| **references** | file paths[] | [Reference files this agent reads] |
| **feedback** | string \| null | [Rewrite instructions from critic or null] |

## Output Contract

[Exact markdown structure this agent returns]

## Domain Instructions

[Domain-specific knowledge, techniques, search patterns]

## Self-Check

[Checklist the agent runs before returning output]
