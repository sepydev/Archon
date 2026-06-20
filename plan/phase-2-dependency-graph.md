# Phase 2 — Dependency Graph & Query Engine

> Part of [Architecture Detective AI](../vision.md) — see [full phase plan](../project_phases_plan.md) and [MCP architecture](../mcp-architecture.md)
> Maps to: **Dependency Graph MCP** · Depends on: [Phase 1](phase-1-repository-scanner.md)

## Goal

Connect all discovered entities into a queryable graph.

## Features

- Model → Serializer
- Serializer → View
- View → URL
- Angular Service → API
- Angular Component → Service
- Form → Model
- Query commands as read operations over the graph (not a separate service)

## Commands

```bash
arch graph
arch find Customer
arch related Customer
arch usages Customer
```

## Output

Graph database / graph representation, queryable via CLI.

## Example

```text
Customer
 ↓
CustomerSerializer
 ↓
CustomerViewSet
 ↓
CustomerApi
 ↓
CustomerComponent
```

## Example Questions

- Where is Customer used?
- Which forms update Customer?
- Which APIs expose Customer?

## Definition of Done

`arch related Customer` returns every entity connected to Customer across both backend and frontend, with no false negatives on the test project.

## Note on scope

Earlier drafts of this plan treated "Query Engine" as its own phase, separate from "Relationship Graph." They're merged here because query commands are just read paths into the same graph — there's no independent service or data model behind them.
