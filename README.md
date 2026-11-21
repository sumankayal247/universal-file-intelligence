# Universal File Intelligence (UFI)

> An offline-first, cross-platform file intelligence suite for Windows and Linux, designed for people and their local AI agents.

**Status: WORK IN PROGRESS.** UFI is currently in the planning and architecture stage. No production-ready application features are available yet. This repository currently contains the project specification and development plan, not a usable file-management application.

## What is UFI?

UFI aims to help people understand, search, verify, and organize files without becoming another complicated file explorer or disk cleaner. It combines storage analysis, file classification, duplicate detection, content-aware search, safe organization workflows, and local AI-agent integration.

UFI has two equally important interfaces:

- **Human interface:** a polished, accessible desktop application for everyday users and power users.
- **Agent interface:** a documented, permission-controlled interface through which a user's own local AI agent can discover and operate supported UFI capabilities.

Both interfaces must use the same core engine. File-management logic must not be duplicated in the GUI, MCP server, CLI, or API.

## Product commitments

- **Windows + Linux:** first-class support for both platforms.
- **Offline-first:** core features work without internet access, cloud accounts, API keys, or an installed language model.
- **Agent-native:** planned MCP, CLI, and local programmatic interfaces.
- **Human approval:** destructive or high-impact operations are reviewable and require authorization.
- **Small footprint:** keep the base installation lean; do not bundle large AI models or heavyweight optional tools by default.
- **Safe by design:** previews, action plans, audit records, quarantine, and recovery where technically possible.
- **Approachable UX:** make powerful file operations understandable to non-technical users.

## Planned capabilities

- Visual disk-space explorer, folder analysis, and large-file discovery.
- Exact duplicate detection using cryptographic hashes.
- Similar-image detection and optional video similarity analysis.
- File identification using content signatures, not extensions alone.
- Metadata extraction and full-text indexing for supported document formats.
- Forgotten-file and old-download discovery, with timestamp limitations clearly explained.
- Smart classification, rename suggestions, and bulk organization previews.
- Archive browsing and integrity testing without indiscriminate extraction.
- File-health checks for supported formats.
- Local natural-language filter search that does not require an LLM.
- Optional local semantic-search integrations.
- MCP tools, CLI commands, and a local API/IPC for agents and automation.
- Reviewable action plans, quarantine, history, and supported undo operations.

All capabilities above are planned, not implemented. See the roadmap for actual progress.

## Proposed technology direction

| Area | Current proposal |
|---|---|
| Desktop UI | Python + PyQt6 |
| Core engine | Modular Python packages |
| Local database | SQLite |
| Full-text search | SQLite FTS5 |
| Agent integration | MCP Python SDK |
| Programmatic interface | CLI and local API / IPC |
| Filesystem monitoring | Watchdog plus platform adapters |
| Image processing | Pillow and perceptual hashing |
| Video metadata | Optional FFmpeg / FFprobe |
| Packaging | Evaluate PyInstaller and platform-native packaging |

Technology choices are provisional until validated with prototypes, dependency audits, platform tests, and release-size measurements.

## Documentation

- [Project plan](docs/PROJECT_PLAN.md): phased roadmap, deliverables, acceptance criteria, and progress tracking.
- [Architecture](docs/ARCHITECTURE.md): core engine, module boundaries, data flow, and proposed structure.
- [Agent integration](docs/AGENT_INTEGRATION.md): MCP, CLI, local API, permissions, and tool contracts.
- [Safety and security](docs/SAFETY_AND_SECURITY.md): authorization, destructive operations, audit, quarantine, and recovery.
- [UX and design](docs/UX_AND_DESIGN.md): product experience, navigation, accessibility, and visual direction.
- [Performance and packaging](docs/PERFORMANCE_AND_PACKAGING.md): footprint budgets, resource limits, and release validation.
- [Decision log](docs/DECISIONS.md): decisions, trade-offs, and unresolved questions.
- [Discussion log](docs/DISCUSSION_LOG.md): summary of the product discussions that established this direction.
- [Contributing](CONTRIBUTING.md): engineering and contribution workflow.

## Development status

| Area | Status |
|---|---|
| Product direction and constraints | Documented |
| Initial architecture proposal | Documented; needs validation |
| Project roadmap | Drafted |
| Application implementation | Not started |
| Automated tests | Not started |
| Windows release | Not started |
| Linux release | Not started |
| MCP server implementation | Not started |

A document being present does not mean the corresponding feature is implemented or production-ready. Tasks must only be marked complete after their acceptance criteria and tests pass.

## Non-goals for the initial release

- Replacing every feature of a full operating-system file manager.
- Requiring a cloud account or remote AI service.
- Bundling a large language model or embedding model in the base installer.
- Running destructive cleanup automatically.
- Declaring a file safe to delete solely because it is old, large, or apparently unused.
- Giving agents unrestricted shell or operating-system access.

## License

UFI is intended to be distributed under the [Apache License 2.0](LICENSE). Third-party dependencies and optional integrations retain their own licenses and notices.

## Important

UFI is not yet ready for use on personal data or production systems. Until implementation and safety testing are complete, treat this repository as a project specification and development workspace.
