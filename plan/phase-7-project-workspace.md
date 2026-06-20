# Phase 7 — Project & Knowledge Workspace

> Part of [Architecture Detective AI](../vision.md) — see [full phase plan](../project_phases_plan.md) and [MCP architecture](../mcp-architecture.md)
> Maps to: **Project MCP** · Depends on: [Phase 6](phase-6-prompt-generator.md)

## Goal

Organize investigations, store findings, and build persistent project knowledge over time.

## Commands

```bash
arch project create drive-permissions
arch task create investigate-domain-access
```

## Features

- Store findings, notes, and generated prompts
- Store architecture snapshots
- Track investigations over time
- Reuse prior findings as context in later investigations on the same project

## Example

```text
Project:
    Drive Permissions

Known Findings:
    - Parent folder grants writer access
    - Child folders inherit permissions
    - Domain permissions only applied in X
```

## Definition of Done

A finding saved in one investigation session is automatically surfaced as relevant context in a later, related investigation on the same project.

## Note on scope

Earlier drafts split this into two phases — "Projects & Tasks" and "Knowledge Base." They're merged here: both are the same persistence layer (the Project MCP) viewed from two angles. Splitting them implied a sequencing dependency between "storing a note" and "storing a finding" that doesn't actually exist.
