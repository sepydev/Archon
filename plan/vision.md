# Architecture Detective AI

## Vision

Architecture Detective AI is a CLI-first platform that transforms a codebase into a queryable system.

It builds a deep understanding of the entire application, including backend services, frontend applications, background jobs, integrations, data flows, and the relationships between them. Rather than forcing developers to manually navigate hundreds of files, the platform provides architectural knowledge that can be explored through simple commands and queries.

The primary goal is to help developers understand what is happening within a project, regardless of whether the logic lives in the backend, frontend, asynchronous tasks, third-party integrations, or multiple systems working together.

The platform should be able to answer questions such as:

- What modifies this model?
- What happens after this action?
- Which forms can update this field?
- Which frontend screens use this API?
- Which services, signals, tasks, or integrations are involved in this workflow?
- Why might this object have changed?
- Which files are relevant to this bug or feature?
- How does this business process work end-to-end?

Beyond architecture exploration, the platform acts as a context engine for AI-assisted development.

By combining architectural knowledge, execution flows, and project-specific context, it can generate high-quality prompts or directly interact with AI agents to:

- Investigate bugs
- Analyze production issues
- Implement features
- Complete user stories
- Review architectural impact
- Generate implementation plans
- Assist with refactoring tasks

The long-term vision is to create a shared understanding layer between developers, codebases, and AI agents.

Instead of providing AI with isolated files or manually collected context, the platform supplies a complete architectural view of the system, allowing both humans and AI to make decisions with a full understanding of how the application works.

## Mission

Turn codebases into queryable systems and provide humans and AI with architectural context instead of raw source code.