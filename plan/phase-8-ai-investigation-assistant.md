# Phase 8 — AI Investigation Assistant

> Part of [Architecture Detective AI](../vision.md) — see [full phase plan](../project_phases_plan.md) and [MCP architecture](../mcp-architecture.md)
> Maps to: **Composition layer** (not a new MCP service) · Depends on: [Phases 1–7](phase-7-project-workspace.md)

## Goal

Compose Phases 1–7 into a single end-to-end workflow that answers an open-ended question without the developer manually chaining commands. This is the **capstone deliverable** — the proof that the architecture works as a system, not just as separate tools.

## Commands

```bash
arch investigate "Why was customer reassigned?"
```

## Responsibilities

- Gather context (Context Builder)
- Discover relevant files (Repo Scanner + Graph)
- Build execution flow (Flow Analysis)
- Identify candidate causes (Impact Analysis)
- Generate hypotheses
- Produce a written investigation report
- Save the investigation to the Project workspace

## Definition of Done

`arch investigate` on a real bug in the test project produces a report that correctly identifies the actual root cause among its top hypotheses.

## Why this phase is last, and why it matters

This isn't a new MCP service — it's the composition of everything built in Phases 1–7 behind one open-ended command. It's listed last because it's only possible once every other phase exists, but it's the phase that actually delivers on the vision doc's promise ("a shared understanding layer between developers, codebases, and AI agents"). Earlier drafts buried this as "just another phase" among ten; it's surfaced here as the payoff the other eight phases are building toward.
