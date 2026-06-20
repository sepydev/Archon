# Phase 6 — AI Prompt Generator

> Part of [Architecture Detective AI](../vision.md) — see [full phase plan](../project_phases_plan.md) and [MCP architecture](../mcp-architecture.md)
> Maps to: **Prompt Generator MCP** · Depends on: [Phase 5](phase-5-context-builder.md)

## Goal

Generate high-quality, target-specific prompts for coding assistants from a context bundle.

## Commands

```bash
arch prompt claude issue-123
arch prompt gpt issue-123
arch prompt cursor issue-123
```

## Output

Ready-to-use prompts containing:

- Architecture overview
- Relevant files
- Execution flow
- Investigation targets
- Known relationships

## Definition of Done

A generated Claude prompt, pasted cold into a fresh session, lets it correctly answer "what does this code do and what touches it" without the developer adding extra context.

## Note on scope

Each target (Claude, GPT, Cursor) should be a pluggable formatter over the same context bundle, not a separate code path. Adding a fourth target later should mean writing one formatter, not touching the other three.
