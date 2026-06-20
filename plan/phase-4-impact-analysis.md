# Phase 4 — Impact Analysis

> Part of [Architecture Detective AI](../vision.md) — see [full phase plan](../project_phases_plan.md) and [MCP architecture](../mcp-architecture.md)
> Maps to: **Impact Analysis MCP** · Depends on: [Phase 3](phase-3-flow-analysis.md)

## Goal

Help investigate bugs and unexpected behavior by identifying everything that can modify a given entity.

## Commands

```bash
arch impact Customer
arch impact CustomerAssignment
```

## Output

Potential sources of modification.

## Example

```text
Possible modifiers:

- CustomerViewSet
- CustomerAdmin
- ReassignmentService
- AssignmentSignal
- SyncTask
```

## Example Questions

- Why was this object modified?
- Which code paths can update this field?

## Definition of Done

`arch impact` lists all real write paths to a field in the test project, with no missed mutation sources.

## Note on naming

This phase was called "Detective Mode" in earlier drafts of the phases plan, while the architecture doc called the same capability "Impact Analysis MCP." Renamed here for consistency — same capability, one name across both docs.
