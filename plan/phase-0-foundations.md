# Phase 0 — Foundations & Parser Strategy

> Part of [Architecture Detective AI](vision.md) — see [full phase plan](project_phases_plan.md) and [MCP architecture](mcp-architecture.md)

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

## Why this phase exists

Every phase from 1 onward depends on a stable contract between "the engine" and "a specific stack." Skipping this and going straight to Django-specific scanning (as in earlier drafts of this plan) means every later service silently absorbs Django assumptions, making a second adapter (Rails, FastAPI, ...) a rewrite instead of an addition.
