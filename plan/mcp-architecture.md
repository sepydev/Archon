# MCP Architecture for Architecture Detective AI

## Overview

This system is designed as a set of MCP (Model Context Protocol-style) services that break down codebase understanding into modular, composable intelligence units.

Instead of one monolithic scanner, the system is split into specialized services that each handle a single responsibility.

The CLI acts as an orchestrator that chains these services together.

---

## Core Idea

- Each MCP service is independent
- Each service produces structured output
- Services can be chained together
- AI is used only on top of structured data
- System understanding comes before prompt generation

---

## MCP Service Layers

### 1. Repo Scanner MCP

Responsible for scanning raw source code.

- Extracts Django models, forms, views, signals
- Extracts Angular components, services, forms
- Builds structured metadata from code

Output: `structured code inventory`

---

### 2. Dependency Graph MCP

Builds relationships between all discovered entities.

- Model → Serializer
- View → API → Frontend Service
- Signal → Service → Task

Output: `system graph`

---

### 3. Flow Analysis MCP

Describes execution behavior.

- What happens when an action is triggered
- Signal chains
- Async task flows

Output: `execution paths`

---

### 4. Impact Analysis MCP

Answers “what can modify this?”

- Find all possible writers of a model/field
- Detect mutation paths
- Identify risky dependencies

Output: `modification sources`

---

### 5. Context Builder MCP

Aggregates relevant data for a specific query.

- Combines graph + flow + impact data
- Builds AI-ready context packages

Output: `structured context bundle`

---

### 6. Prompt Generator MCP

Converts context into AI prompts.

- Claude prompt format
- GPT prompt format
- Cursor / coding agent formats

Output: `AI-ready prompts`

---

### 7. Project MCP

Manages investigation workflows.

- Projects
- Tasks
- Notes
- History of analysis

Output: `structured investigation workspace`

---

## CLI Orchestration Layer

The CLI acts as a thin wrapper over MCP services:

```bash
arch scan .
arch graph
arch flow Customer
arch detect Customer
arch context Customer
arch prompt issue-123



Each command internally calls one or more MCP services.


## Data Flow

Codebase
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


Design Principles

* Keep services independent
* Never mix scanning with reasoning
* Prefer structured output over raw text
* AI is optional, not required
* Graph is the source of truth
* CLI is orchestration only


## Goal

To transform a codebase into a system that can be queried, explored, and reasoned about — both by humans and AI — without manually reading the entire codebase.