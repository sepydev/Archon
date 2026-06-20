# Phase 5 — Context Builder

> Part of [Architecture Detective AI](../vision.md) — see [full phase plan](../project_phases_plan.md) and [MCP architecture](../mcp-architecture.md)
> Maps to: **Context Builder MCP** · Depends on: [Phase 4](phase-4-impact-analysis.md)

## Goal

Automatically assemble bounded, relevant investigation context for a target entity or question.

## Commands

```bash
arch context CustomerAssignment
```

## Features

- Combine relevant output from Phases 1–4 (graph + flow + impact) for one target
- Rank by relevance and cap to a size/token budget — never dump the full graph
- Builds AI-ready context packages

## Output

- Relevant models, forms, services, signals, tasks
- Related frontend components
- Execution flow

## Definition of Done

`arch context` for a known entity produces a bundle that a developer agrees contains "everything I'd need," within a defined size limit.

## Note on scope

"Combine everything" is not a real spec — without a relevance ranking and a size budget, context bundles for any non-trivial entity will balloon past what's useful (or fits in a prompt). This phase explicitly owns that tradeoff.
