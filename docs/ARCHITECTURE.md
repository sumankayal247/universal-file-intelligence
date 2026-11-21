# UFI Architecture

**Status:** Proposed architecture. Validate through prototypes before implementation.

## 1. Core principle

UFI is a core application engine with multiple interfaces, not a GUI application with an API bolted on later.

The desktop GUI, MCP server, CLI, and local API must invoke the same application services and capability implementations. No interface may duplicate filesystem or business logic.

## 2. High-level system

Human user -> Desktop GUI  
Local AI agent / developer -> MCP, CLI, or local API  
All interfaces -> Interface adapters -> Capability/application services -> Domain and policy layer -> Filesystem adapters and extractors -> Windows/Linux filesystems and SQLite indexes.

The diagram is conceptual. Final dependency direction and process boundaries must be established by architecture tests.

## 3. Layer responsibilities

### Interfaces
- Desktop: presentation, navigation, progress, previews, and approvals.
- MCP: discoverable agent tools, schema validation, identity, and permission checks.
- CLI: scriptable commands and human-readable output.
- Local API / IPC: versioned programmatic interface.

Interfaces must not directly perform raw filesystem operations except through approved core services.

### Application services
Coordinate use cases such as scanning, finding duplicates, querying metadata, proposing organization, and executing action plans.

### Domain
Contains typed models and rules independent of PyQt6, MCP SDK, FastAPI, or platform APIs. Examples include FileRecord, ScanJob, DuplicateGroup, SearchQuery, ActionPlan, ActionResult, and PermissionScope.

### Infrastructure
Provides SQLite repositories, filesystem adapters, metadata extractors, hashing, archive inspection, and platform integrations.

### Policy and safety
Validates scope, authorization, operation risk, approval state, and audit requirements. Mutating services must pass through policy enforcement.

## 4. Capability registry

Use a central registry to publish stable application capabilities to all interfaces.

A capability definition should include:
- Stable identifier and version.
- Description and typed input/output schema.
- Read-only or mutating classification.
- Required permission scope.
- Risk level and approval policy.
- Cancellation and progress behavior.
- Audit requirements.
- Error model and idempotency behavior where applicable.

The registry must only execute explicitly registered, validated handlers. It is not a mechanism for arbitrary Python execution.

## 5. Proposed package layout

- src/ufi/app: bootstrap, configuration, lifecycle.
- src/ufi/domain: entities, value objects, errors, policies.
- src/ufi/application: use cases and orchestration.
- src/ufi/capabilities: shared capability definitions and handlers.
- src/ufi/infrastructure/database: SQLite, migrations, repositories.
- src/ufi/infrastructure/filesystem: traversal and filesystem providers.
- src/ufi/infrastructure/extractors: format-specific metadata/text extractors.
- src/ufi/infrastructure/hashing: exact and perceptual fingerprints.
- src/ufi/infrastructure/archives: archive readers and validators.
- src/ufi/infrastructure/monitoring: filesystem change events.
- src/ufi/interfaces/desktop: PyQt6 views and presentation.
- src/ufi/interfaces/mcp: MCP server and tool adapters.
- src/ufi/interfaces/cli: command-line interface.
- src/ufi/interfaces/api: local API or IPC adapter.
- src/ufi/platform/windows and src/ufi/platform/linux: platform-specific implementations.
- src/ufi/security: authorization, audit, and path safety.
- tests: unit, integration, contract, security, and performance suites.

This is a proposed layout, not a requirement to create empty folders before they have a real implementation.

## 6. Database direction

Use SQLite for local metadata and job state. Enable foreign keys, transactions, schema migrations, and a documented backup strategy. Evaluate WAL mode against actual filesystem and shutdown behavior.

Likely logical tables:
- schema_migrations
- volumes and scan_roots
- scan_jobs and scan_checkpoints
- files and file_metadata
- file_hashes and duplicate_groups
- extracted_text with FTS5
- saved_searches
- action_plans, action_items, and action_journal
- audit_events
- agent_profiles and permission_grants

Derive the final schema from use cases and test migrations. Do not store full file contents by default. Account for case sensitivity, volume identity, replacement, and path changes.

## 7. File identity and indexing

A path is not a permanent file identity. Track enough information to detect replacement or change, such as volume/root identity, normalized path under platform rules, size, timestamps, and optional filesystem identifiers. Invalidate hashes when source state changes.

Never trust a filename extension as proof of file format. Use signatures and safe parser validation where supported.

## 8. Scanning and concurrency

- Scans are background jobs with progress and cancellation.
- Persist checkpoints so interrupted scans can resume safely.
- Use bounded worker pools and configurable I/O/CPU limits.
- Avoid unbounded queues and loading full file contents into memory.
- Handle permission errors as scan results, not fatal crashes.
- Define symlink, junction, mount-point, and network-volume policies.
- Revalidate file state before acting on indexed results.
- Keep the GUI responsive.

## 9. Platform abstraction

Isolate platform-specific behavior behind interfaces for volume discovery, path normalization, file identity, permissions, trash/quarantine integration, filesystem events, and packaging.

Windows and Linux do not have identical path, case, permission, timestamp, or trash semantics. Document differences rather than pretending they are interchangeable.

## 10. Process model

Start with a desktop process plus an independently launchable MCP STDIO process. Shared code and storage do not require every interface to run in the same process.

If a persistent local service is later required, define lifecycle, authentication, locking, upgrades, and single-instance behavior explicitly. Do not start a listening network server automatically without a clear user-visible setting.

## 11. Architecture quality gates

- Domain and application layers import no PyQt6 modules.
- Interfaces call shared capabilities rather than private infrastructure.
- Every mutating capability passes through authorization and action policy.
- Capability schemas are versioned and contract-tested.
- Platform-specific code is isolated and tested on its target OS.
- Database schema changes are migrated and tested.
- Large operations are bounded, cancellable, and observable.
