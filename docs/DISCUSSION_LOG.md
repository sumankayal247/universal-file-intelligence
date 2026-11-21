# Product Discussion Log

This document summarizes the initial planning conversation and records the reasoning behind UFI's current scope. It is not a transcript and does not imply that any planned feature has been implemented.

## 1. Original concept

The project began as Universal File Intelligence, a desktop utility intended to understand what consumes storage and help users manage files.

Initial feature ideas:
- Visual disk-space explorer.
- Exact duplicate detection using hashes.
- Similarity-based duplicate detection.
- Forgotten and unused file discovery.
- Large-file discovery and old-download identification.
- Bulk file organization.
- Smart rename suggestions based on file contents.
- Archive preview without full extraction.
- Potentially corrupted-file detection.
- Natural-language searches such as finding old videos larger than 500 MB.
- Reviewable results; nothing should be deleted without user approval.

The original technology idea was Python, PyQt6, and SQLite.

## 2. Product scope agreed

The direction was expanded to a full-featured professional file intelligence suite rather than a simple cleaner.

Planned intelligence areas:
1. Storage intelligence.
2. Duplicate and similarity intelligence.
3. File identification, classification, and metadata.
4. Smart organization and rename suggestions.
5. Local search and natural-language filters.
6. File health and archive inspection.
7. Action planning, review, execution, and recovery.
8. External AI-agent integration.

## 3. Platform and offline requirements

Selected direction:
- Windows + Linux.
- Completely offline core functionality.
- Full-featured professional suite.
- Polished, approachable UI for ordinary users.

Core functionality must not depend on cloud AI, API keys, or a bundled model. Optional local models may extend semantic search but are not required for normal use.

## 4. Agent-native direction

A key requirement is that any local AI agent a user already has should be able to control UFI's supported features.

Proposed approach:
- MCP over STDIO as the primary integration.
- Optional authenticated loopback MCP over HTTP for clients that need a persistent local endpoint.
- CLI and a versioned local API/IPC for developers and custom automation.
- Shared capability registry so every interface invokes the same core services.
- Agent Center for connections, permissions, pending approvals, active jobs, and history.

Agents should inspect files, analyze storage, search, compare duplicates, propose organization, create action plans, and execute only after required authorization. They must not receive unrestricted shell or filesystem access.

## 5. Safety direction

UFI handles important personal data. The project must use:
- Read-only agent access by default.
- Explicit, scoped permissions.
- Dry-run previews and action plans.
- Approval bound to the exact plan and affected items.
- Revalidation immediately before execution.
- Quarantine instead of routine permanent deletion.
- Audit history and recovery manifests.
- Undo only where technically possible.
- Clear disclosure of operations that cannot be reversed.

## 6. UI direction

The UI should be modern, polished, accessible, and uncluttered.

Proposed screens:
- Overview.
- Storage Explorer.
- Duplicates.
- File Intelligence.
- Search.
- Organization.
- Action Center.
- Agent Center.
- Settings.

Desired qualities include light/dark themes, visual storage analysis, grid/list/treemap views, keyboard shortcuts, accessible contrast, and clear progress/error states.

## 7. Application size discussion

Because UFI exists to help reduce clutter, its own footprint should remain modest.

Agreed targets:
- Aim for a core installed footprint around 150 MB.
- Treat 200 MB as the preferred upper target.
- Aim for installer/package downloads around 100 MB where practical.
- Bundle no AI models.
- Make FFmpeg, advanced parsers, and other heavyweight components optional where feasible.
- Measure actual Windows and Linux release artifacts before making public size claims.

These are targets, not measured build results.

## 8. Proposed technology direction

Current proposal:
- Python + PyQt6 desktop.
- Modular Python core engine.
- SQLite metadata and FTS5 full-text search.
- MCP Python SDK.
- CLI and local API/IPC.
- Platform adapters for Windows and Linux.
- Pillow/ImageHash for image handling and similarity.
- Optional FFmpeg/FFprobe for video metadata and analysis.
- Incremental scanner, bounded background workers, and resumable jobs.

All dependency and packaging choices remain subject to prototype validation.

## 9. Development approach

The project should be implemented in phases with production-quality acceptance criteria.

- Validate high-risk architecture and packaging choices before committing.
- Build the shared core before expanding interfaces.
- Treat MCP integration as a first-class interface, not an afterthought.
- Keep the roadmap updated.
- Do not mark partial work complete.
- For major architecture changes, document the proposal and use a reviewable pull request before merging.
- Test Windows and Linux independently.
- Keep the README honest about WIP status.

## 10. Current project state

At the time this document was created, the GitHub repository was empty. The initial repository setup adds documentation and a license only. It does not create an application implementation.

The next work should be Phase 0 of PROJECT_PLAN.md: technical prototypes, dependency and license review, measured packaging tests, and architecture decisions.
