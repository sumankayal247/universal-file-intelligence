# Contributing to UFI

Thank you for considering a contribution. UFI is currently a work in progress, and its architecture and requirements are still being validated.

## Before starting

- Read the README.
- Review docs/PROJECT_PLAN.md.
- Review docs/ARCHITECTURE.md.
- Check docs/DECISIONS.md for existing constraints.
- For major changes, open an issue or discussion describing the problem and proposed approach before implementation.

## Engineering principles

- Keep the core engine independent of PyQt6 and interface-specific frameworks.
- Route GUI, MCP, CLI, and API behavior through shared application capabilities.
- Preserve offline-first operation.
- Keep Windows and Linux behavior behind tested platform adapters.
- Prefer small, focused, reviewable changes.
- Avoid unrelated formatting or refactoring.
- Do not introduce heavyweight dependencies without measuring their impact and reviewing their licenses.
- Do not add bundled AI models to the base installation.
- Never bypass authorization, action planning, or audit requirements for convenience.
- Treat file contents and metadata as potentially sensitive and untrusted.

## Tests

Add or update tests for every behavior change. Include failure cases, permission errors, cancellation, and platform-specific behavior where relevant.

Before requesting review, run the relevant test suite, formatting/lint checks, and type/static checks. State clearly which checks were run and which could not be run.

## Documentation and roadmap

Update relevant documentation when behavior, architecture, tool schemas, permissions, dependencies, or user workflows change.

A task may be marked complete in PROJECT_PLAN.md only after its acceptance criteria pass. Partial implementations and placeholders must remain visibly incomplete.

## Pull requests

- Keep pull requests focused.
- Explain the problem, proposed solution, and trade-offs.
- Include tests and documentation updates.
- Include screenshots or recordings for user-visible changes when useful.
- Identify Windows/Linux test coverage.
- For major architectural work, use a draft pull request for early review before merging.
- Document new decisions in docs/DECISIONS.md.

## Security issues

Do not publish exploitable security details in a public issue before coordinating a responsible fix. Report security concerns privately to the repository owner until a disclosure process is established.
