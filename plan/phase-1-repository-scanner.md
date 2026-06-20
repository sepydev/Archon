# Phase 1 — Repository Scanner

> Part of [Architecture Detective AI](../vision.md) — see [full phase plan](../project_phases_plan.md) and [MCP architecture](../mcp-architecture.md)
> Maps to: **Repo Scanner MCP** · Depends on: [Phase 0](phase-0-foundations.md)

## Goal

Create a structured representation of the codebase, using the Phase 0 adapter interface.

## Features

- Django adapter: models, forms, serializers, viewsets/views, signals, Celery tasks, URLs
- Angular adapter: components, services, forms
- Normalize all adapter output into the common intermediate representation
- Index the resulting project structure

## Output

- JSON metadata
- Indexed project structure

## Example

```text
Customer
CustomerForm
CustomerSerializer
CustomerViewSet
CustomerComponent
```

## Definition of Done

Running `arch scan .` on a real Django + Angular project produces a complete, correct entity inventory with no manual fixups.
