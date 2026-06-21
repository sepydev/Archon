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




## Hardening the Adapter Contract

These six additions don't expand Phase 0's scope so much as make its existing deliverable (the adapter interface) actually enforceable instead of aspirational. Without them, "adapter interface" is a convention; with them, it's a contract something can fail.

### 1. IR Versioning
Define `ir_version` on the intermediate representation from the first emitted object, not retrofitted later.
- **Why now:** the first schema change after adapters exist becomes a migration problem instead of a field bump.
- **Done when:** every IR object carries `ir_version`, and a bump policy (what counts as breaking vs. additive) is written down.

### 2. Entity Taxonomy, Reserved Early
Reserve entity type names — `Model`, `Service`, `APIEndpoint`, `Component`, `Permission` — even for types no phase implements yet.
- **Why now:** prevents each phase from inventing its own ad hoc type names as it goes.
- **Guardrail:** a reserved-but-unimplemented type must throw `NotImplemented`, not silently half-work. Reserving the name is not license to partially build it early.
- **Done when:** the full taxonomy enum exists; only types with an owning phase are actually constructible.

### 3. Mandatory Source Location
Every emitted object must include `file`, `line`, and `column`.
- **Why now:** without it, downstream phases (Impact Analysis, Prompt Generator) can point at *what* but not *where* — which is most of the value of a code-aware system.
- **Guardrail:** "mandatory" only means something if the contract tests (#6) reject objects missing it. Schema field alone is not enforcement.
- **Done when:** contract tests fail any adapter emitting an object without all three fields.

### 4. Reference Adapter (formerly "Hello World Adapter")
Rename and treat as the literal template new adapters are copied from — not throwaway scaffolding.
- **Why now:** "Hello World" signals disposable; "Reference" signals "this is the pattern."
- **Guardrail:** held to the same bar as a real adapter — passes its own contract tests, exercises the taxonomy types it claims to support.
- **Done when:** a new adapter can be built by copying the Reference Adapter's structure and contract test file with minimal deviation.

### 5. Adapter Capabilities Declaration
Adapters declare what they support: models, endpoints, permissions, call graph, etc.
- **Why now:** without this, "adapter returns nothing for call graph" is indistinguishable from "this codebase has no call graph here" — a silent gap that surfaces as a confusing bug in Phase 3+, not as a clear limitation.
- **Done when:** every downstream service can check `adapter.capabilities` before querying, and a missing capability is a declared skip, not a silent empty result.

### 6. Adapter Contract Tests
A validation suite every adapter must pass, checking versioning, taxonomy correctness, mandatory fields, and capability honesty (declared capabilities are actually exercised, not just listed).
- **Why now:** this is what turns #1–#5 from conventions into something enforceable. Without it, none of the above are guaranteed — just hoped for.
- **Done when:** the Reference Adapter passes 100% of its own declared capabilities under the suite, and the suite is wired to run against any new adapter (Django, Angular, future) before it's considered shippable.

---

**Net effect:** Phase 0 defines and stubs all six. None are *proven* until a real adapter runs through them — that validation happens in Phase 1, when the Django and Angular adapters become the first real test of a contract that, until then, has only been exercised by the Reference Adapter itself.
