# Architecture Detective AI — Project Phases Plan

This plan has 9 build phases (0–8), aligned 1:1 with the MCP services defined in `mcp-architecture.md`. Each phase maps to exactly one service, so "is Phase N done?" and "is the {X} MCP done?" are the same question.

---

# Phase 0 — Foundations & Parser Strategy

## Goal

Establish the adapter interface and parsing strategy that every later phase depends on, before committing to Django/Angular-specific feature work.

## Features

- Define the common intermediate representation (entity, relationship, field, call-site) all adapters must emit
- Define the adapter plugin interface (`discover()`, `parse()`, `emit()`)
- Decide and prototype the parsing approach per language (Python AST for Django, tree-sitter or TS compiler API for Angular)
- Define project config format (what's in scope, exclusions, adapter selection)
- Build a "hello world" adapter that round-trips one trivial entity through the full pipeline, to prove the interface works end-to-end

## Output

- Adapter interface specification
- Parsing strategy decision record
- Minimal working adapter skeleton

## Definition of Done

A toy Django model can be parsed into the common intermediate representation and printed back out, through the adapter interface — not hardcoded.

---

# Phase 1 — Repository Scanner

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

---

# Phase 2 — Dependency Graph & Query Engine

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

---

# Phase 3 — Flow Analysis

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

---

# Phase 4 — Impact Analysis

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

---

# Phase 5 — Context Builder

## Goal

Automatically assemble bounded, relevant investigation context for a target entity or question.

## Commands

```bash
arch context CustomerAssignment
```

## Features

- Combine relevant output from Phases 1–4 (graph + flow + impact) for one target
- Rank by relevance and cap to a size/token budget — never dump the full graph

## Output

- Relevant models, forms, services, signals, tasks
- Related frontend components
- Execution flow

## Definition of Done

`arch context` for a known entity produces a bundle that a developer agrees contains "everything I'd need," within a defined size limit.

---

# Phase 6 — AI Prompt Generator

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

---

# Phase 7 — Project & Knowledge Workspace

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

---

# Phase 8 — AI Investigation Assistant

## Goal

Compose Phases 1–7 into a single end-to-end workflow that answers an open-ended question without the developer manually chaining commands. This is the capstone deliverable — the proof that the architecture works as a system, not just as separate tools.

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

---

# Future Ideas (Out of Scope for Phases 0–8)

- VSCode extension
- JetBrains plugin
- Web UI
- Neo4j visualization
- Git history integration
- Jira integration
- Slack integration
- Log analysis
- Audit trail integration
- Multi-repository support
- Additional language/framework adapters (Rails, FastAPI, React, Vue, ...)

---

# Project-Level Success Criteria

A developer should be able to answer, without manually reading hundreds of files:

- What is this?
- Where is it used?
- What modifies it?
- What happens after it changes?
- Which files matter?
- Give me a complete AI prompt.
