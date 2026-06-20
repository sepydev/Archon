# Architecture Detective AI

## Vision

Architecture Detective AI is a CLI-first platform that transforms a codebase into a queryable system.

It builds a deep understanding of an application — backend services, frontend applications, background jobs, integrations, data flows, and the relationships between them — and exposes that understanding through simple commands and queries, instead of requiring developers to manually navigate hundreds of files.

The platform's understanding is built on a **language- and framework-agnostic core**. The first supported stack is Django + Angular, but "Django + Angular" is an adapter plugged into the core, not the core itself. Adding support for another stack should mean writing a new adapter, not rewriting the engine.

The platform should be able to answer questions such as:

- What modifies this model?
- What happens after this action?
- Which forms can update this field?
- Which frontend screens use this API?
- Which services, signals, tasks, or integrations are involved in this workflow?
- Why might this object have changed?
- Which files are relevant to this bug or feature?
- How does this business process work end-to-end?

## Beyond Exploration: A Context Engine for AI

Architecture Detective AI also acts as a context engine for AI-assisted development. By combining architectural knowledge, execution flows, and project-specific context, it can generate high-quality prompts or directly drive AI agents to:

- Investigate bugs
- Analyze production issues
- Implement features
- Complete user stories
- Review architectural impact
- Generate implementation plans
- Assist with refactoring tasks

## Long-Term Vision

Create a shared understanding layer between developers, codebases, and AI agents. Instead of feeding AI isolated files or manually collected context, the platform supplies a structured architectural view of the system — so both humans and AI act with a full understanding of how the application actually works, not a guess reconstructed from a handful of open files.

## Mission

Turn codebases into queryable systems, and give humans and AI architectural context instead of raw source code.
