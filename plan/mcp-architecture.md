# MCP Architecture for Architecture Detective AI

## Overview

This system is built as a set of MCP (Model Context Protocol-style) services that break codebase understanding into modular, composable intelligence units.

Instead of one monolithic scanner, the system is split into specialized services that each handle a single responsibility. The CLI is a thin orchestrator that chains these services together. Nothing in the core engine is hardcoded to a specific language or framework — stack-specific knowledge lives in **adapters** that plug into the core.

---

## Core Idea

- Each MCP service is independent
- Each service produces structured output
- Services can be chained together
- Stack-specific logic (Django, Angular, ...) lives in adapters, not in the core services
- AI is used only on top of structured data
- System understanding comes before prompt generation

---

## Foundation Layer: Adapter Interface

Before any service can run, the system needs a stable contract between "the engine" and "a specific stack."

- Defines a common intermediate representation (entity, relationship, field, call-site) that every adapter must produce
- Defines the plugin interface adapters implement: `discover()`, `parse()`, `emit()`
- Parsing strategy is decided here (AST-based per language, e.g. Python `ast` / tree-sitter for JS-TS), not per-service
- Django + Angular ship as the first adapter pair; they are consumers of this interface, not part of it

Output: `adapter interface + first-party Django/Angular adapter`

---

## MCP Service Layers

### 1. Repo Scanner MCP

Responsible for scanning raw source code through the adapter layer.

- Invokes the active adapter(s) to extract entities: Django models, forms, views, signals, serializers; Angular components, services, forms
- Normalizes adapter output into the common intermediate representation
- Builds structured metadata from code

Output: `structured code inventory`

---

### 2. Dependency Graph MCP

Builds relationships between all discovered entities, and exposes them for querying.

- Model → Serializer
- View → API → Frontend Service
- Signal → Service → Task
- Provides direct query operations over the graph (find, related, usages) — these are read paths into this service, not a separate layer

Output: `system graph` (queryable)

---

### 3. Flow Analysis MCP

Describes execution behavior, across both synchronous and asynchronous paths.

- What happens when an action is triggered (sync call chains)
- Signal chains
- Async task flows (Celery, queues, scheduled jobs)

Output: `execution paths`

---

### 4. Impact Analysis MCP

Answers "what can modify this?"

- Find all possible writers of a model/field
- Detect mutation paths
- Identify risky or surprising dependencies

Output: `modification sources`

---

### 5. Context Builder MCP

Aggregates relevant data for a specific query, within a bounded size.

- Combines graph + flow + impact data for a target entity or question
- Ranks relevance and respects a token/size budget — does not dump the entire graph
- Builds AI-ready context packages

Output: `structured context bundle`

---

### 6. Prompt Generator MCP

Converts a context bundle into AI prompts via pluggable output formatters.

- Claude prompt format
- GPT prompt format
- Cursor / coding agent formats
- New formats are added as formatters, not as new code paths through the rest of the system

Output: `AI-ready prompts`

---

### 7. Project MCP

Manages investigation workflows and persists what's been learned.

- Projects and tasks
- Notes and findings
- Architecture snapshots over time
- History of analysis, queryable later (this is the persistence layer; there is no separate "knowledge base" service — it's the same store)

Output: `structured investigation workspace`

---

## Composition Layer: AI Investigation Assistant

Not a new MCP service — a CLI-level workflow that composes services 1–7 end-to-end to answer an open-ended question (e.g. `arch investigate "Why was the customer handed over?"`) without the user manually chaining commands.

---

## CLI Orchestration Layer

The CLI is a thin wrapper over MCP services:

```bash
arch scan .
arch graph
arch find Customer
arch flow Customer
arch impact Customer
arch context Customer
arch prompt claude issue-123
arch investigate "Why was the customer handed over?"
```

Each command internally calls one or more MCP services.

---

## Data Flow

```
Codebase
  ↓
Adapter Layer (stack-specific parsing)
  ↓
Repo Scanner MCP
  ↓
Dependency Graph MCP
  ↓
Flow Analysis MCP
  ↓
Impact Analysis MCP
  ↓
Context Builder MCP
  ↓
Prompt Generator MCP (optional AI layer)
  ↓
Project MCP (persisted throughout)
```

---

## Design Principles

- Keep services independent
- Never mix scanning with reasoning
- Stack-specific logic stays in adapters, never leaks into core services
- Prefer structured output over raw text
- AI is optional, not required
- Graph is the source of truth
- CLI is orchestration only

---

## Goal

To transform a codebase into a system that can be queried, explored, and reasoned about — both by humans and AI — without manually reading the entire codebase.
