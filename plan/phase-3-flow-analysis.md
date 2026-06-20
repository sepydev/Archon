# Phase 3 — Flow Analysis

> Part of [Architecture Detective AI](../vision.md) — see [full phase plan](../project_phases_plan.md) and [MCP architecture](../mcp-architecture.md)
> Maps to: **Flow Analysis MCP** · Depends on: [Phase 2](phase-2-dependency-graph.md)

## Goal

Understand execution paths, both synchronous and asynchronous.

## Commands

```bash
arch flow Customer
arch flow CustomerAssignment
```

## Example Output

```text
Customer.save()
 ↓
post_save signal
 ↓
AssignmentService
 ↓
CreateAssignmentTask
```

## Example Questions

- What happens after customer creation?
- Which tasks run after this signal?
- Which services are involved?

## Definition of Done

`arch flow` correctly traces at least one multi-hop signal → service → async task chain in the test project, matching manual inspection of the code.
