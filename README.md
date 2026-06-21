# Archon

I’m going to build a CLI-based architecture intelligence tool that scans codebases (Django, Angular, etc.) and turns them into a queryable system.  

If I can pull it off, it should help developers understand what is happening across backend and frontend, trace flows, and generate AI-ready context for debugging or feature work.


### [Vision](plan/vision.md)
### [Architecture](plan/mcp-architecture.md)

## Structure 
The project is designed as a monorepo composed of multiple independently structured modules. Each directory represents a self-contained sub-project within the same repository, allowing clear separation of concerns while enabling shared tooling, unified version control, and coordinated development. New adapters can be added without modifying the core architecture, as long as they adhere to the defined interfaces and contract tests.```

```
Archon/
├── archon-core/              # IR schema, Adapter interface, registry, contract tests — Phase 0
│   ├── ir/
│   ├── adapter_base.py
│   └── contract_tests/
├── archon-adapter-reference/ # the template, trivial but contract-complete
├── archon-adapter-django/    # Phase 1
├── archon-adapter-angular/   # Phase 1
├── archon-cli/                # orchestration layer, later phases
└── archon-adapter-fastapi/   # future — added with zero edits to archon-core

```