# UFI Project Plan

**Status:** Planning / Work in Progress  
**Last updated:** 2026-11-21  
**Progress rule:** A phase is not complete merely because code exists. Its acceptance criteria and required tests must pass.

## 1. Product goal

Build a professional, offline-first file intelligence suite for Windows and Linux. UFI must make storage understandable and manageable for everyday users while exposing the same core capabilities to local AI agents and developers.

## 2. Non-negotiable constraints

1. Core functionality works without internet, API keys, accounts, or a language model.
2. Windows and Linux are first-class targets, with platform-specific behavior isolated behind adapters.
3. GUI, MCP, CLI, and local API use the same core capability implementations.
4. Destructive operations are planned, reviewable, authorized, logged, and recoverable where possible.
5. The base application remains lean. AI models and heavyweight tools are optional.
6. Large scans are incremental, resumable, cancellable, and resource-aware.
7. No feature is labelled complete while it has placeholders, untested edge cases, or missing documentation.

## 3. Phase roadmap

All phases begin as **Not started**.

### Phase 0 — Product and technical validation
- [ ] Confirm supported Windows versions and Linux distributions.
- [ ] Prototype PyQt6 packaging on Windows and Linux.
- [ ] Measure the bundled size of a minimal PyQt6 application.
- [ ] Prototype the core engine without GUI imports.
- [ ] Validate MCP SDK transport choices and client compatibility.
- [ ] Compare local IPC/API options and document security implications.
- [ ] Audit candidate dependencies, licenses, and maintenance health.
- [ ] Define baseline hardware and performance test datasets.
- [ ] Record high-impact decisions in DECISIONS.md.

**Acceptance:** High-risk architecture choices have evidence from prototypes; packaging and compatibility assumptions are measured.

### Phase 1 — Repository and application foundation
- [ ] Establish package metadata, supported Python version, dependency groups, and lock strategy.
- [ ] Create application bootstrap and configuration system.
- [ ] Create structured logging with privacy-conscious defaults.
- [ ] Create platform detection and capability adapters.
- [ ] Establish SQLite connection management, schema versioning, and migrations.
- [ ] Define typed domain models and error taxonomy.
- [ ] Set up formatting, linting, static checks, and test automation.
- [ ] Add CI for supported Windows and Linux environments.
- [ ] Add minimal startup and clean shutdown.

**Acceptance:** Clean checkout installs reproducibly; application starts and exits on both platforms; migrations and CI tests pass.

### Phase 2 — Filesystem abstraction and scanner
- [ ] Define a filesystem provider interface.
- [ ] Implement Windows and Linux filesystem adapters.
- [ ] Handle permissions, symlinks, junctions, mount points, hidden files, and inaccessible paths.
- [ ] Define scan scopes, exclusions, and protected system locations.
- [ ] Implement incremental scanning and change detection.
- [ ] Persist scan jobs, checkpoints, and status.
- [ ] Support pause, resume, cancel, and crash recovery.
- [ ] Add configurable worker and I/O limits.
- [ ] Prevent recursive traversal loops and duplicate traversal.
- [ ] Record scan provenance and timestamp reliability.

**Acceptance:** Tests cover permission failures, symlinks, mount boundaries, large trees, cancellation, interrupted scans, and recovery on both platforms.

### Phase 3 — Storage intelligence
- [ ] Aggregate bytes and file counts by drive, directory, category, and file type.
- [ ] Identify largest files and folders.
- [ ] Build treemap-ready data structures.
- [ ] Detect old downloads and stale candidates without assuming access time is reliable.
- [ ] Track storage snapshots and growth trends.
- [ ] Add filters, sorting, and drill-down queries.
- [ ] Ensure analysis does not copy file contents unnecessarily.

**Acceptance:** Aggregates reconcile with scanner data; large-directory analysis remains responsive; UI and CLI results agree.

### Phase 4 — Duplicate and similarity intelligence
- [ ] Apply cheap candidate filters before hashing, such as file size.
- [ ] Implement chunked exact hashing with algorithm/version metadata.
- [ ] Detect duplicates across approved scan roots.
- [ ] Build stable duplicate groups and comparison details.
- [ ] Implement perceptual image similarity as a confidence-based feature.
- [ ] Evaluate video similarity as an optional, resource-intensive extension.
- [ ] Handle changing files and hash invalidation safely.
- [ ] Provide recommendations without automatically deleting candidates.

**Acceptance:** Exact duplicate results are reproducible and tested; similarity results show confidence and limitations; no action occurs without approval.

### Phase 5 — File identification, metadata, and content extraction
- [ ] Identify formats using signatures and validated parsers, not extensions alone.
- [ ] Build an adapter model for metadata extractors.
- [ ] Extract image, audio, video, archive, and document metadata where supported.
- [ ] Add text extraction for explicitly supported document formats.
- [ ] Apply file-size, time, and memory limits to parsers.
- [ ] Treat malformed and hostile files as untrusted input.
- [ ] Store extractor versions and extraction status.
- [ ] Make optional format handlers installable separately.

**Acceptance:** Fixture corpus covers valid, malformed, truncated, and adversarial files; extraction is bounded and does not execute file content.

### Phase 6 — Search and query intelligence
- [ ] Implement indexed metadata search.
- [ ] Add SQLite FTS5 for extracted text.
- [ ] Define a safe structured query model.
- [ ] Parse common natural-language filters locally: size, type, dates, folders, duplicate status.
- [ ] Explain parsed filters before executing ambiguous queries.
- [ ] Support saved searches and reusable filters.
- [ ] Design optional semantic search as a separate opt-in extension.

**Acceptance:** Query parsing has deterministic tests, unsupported requests fail clearly, and core search works offline without an LLM.

### Phase 7 — Organization and file operations
- [ ] Define a typed action-plan model.
- [ ] Add dry-run previews for moves, renames, and supported cleanup actions.
- [ ] Detect destination collisions and path-length constraints.
- [ ] Verify source identity and state immediately before execution.
- [ ] Implement staged operations and transaction journals.
- [ ] Implement quarantine with original-path manifests.
- [ ] Implement supported undo and recovery.
- [ ] Add bulk-operation limits and cancellation.
- [ ] Require explicit approval according to action risk.

**Acceptance:** Fault-injection tests cover crashes and partial failures; no silent overwrite or permanent deletion; supported undo restores expected state.

### Phase 8 — MCP, CLI, and local API
- [ ] Create a capability registry with typed input/output schemas.
- [ ] Expose read-only MCP tools first.
- [ ] Add resource and prompt support only where useful and secure.
- [ ] Add agent discovery and setup documentation.
- [ ] Add per-agent identity and permission profiles.
- [ ] Add local HTTP transport only with loopback binding and authentication.
- [ ] Add CLI commands over the same capability layer.
- [ ] Add local API/IPC with explicit versioning.
- [ ] Add approval handoff for mutating actions.
- [ ] Add contract tests proving interface parity with the core engine.

**Acceptance:** Independent MCP clients discover and use read-only tools offline; mutation tools obey GUI-equivalent permission and approval rules.

### Phase 9 — Safety, privacy, and security hardening
- [ ] Complete threat model and security review.
- [ ] Enforce path scope and canonicalization checks.
- [ ] Prevent symlink/junction escape from approved roots.
- [ ] Add local API authentication and origin/host protections.
- [ ] Add audit records for agent and human actions.
- [ ] Add redaction rules for logs and diagnostics.
- [ ] Validate archive extraction paths against traversal attacks.
- [ ] Add database backup and recovery procedures.
- [ ] Test malicious filenames, parser inputs, and race conditions.
- [ ] Document irreversible operations and limits of undo.

**Acceptance:** Security suite passes; high-risk findings are resolved or documented; agent actions are attributable and auditable.

### Phase 10 — Desktop UX
- [ ] Implement navigation and responsive layouts.
- [ ] Implement overview dashboard.
- [ ] Implement storage explorer and treemap.
- [ ] Implement duplicate review.
- [ ] Implement file intelligence and search screens.
- [ ] Implement action center and operation history.
- [ ] Implement Agent Center and permission management.
- [ ] Add light/dark themes, keyboard navigation, accessible contrast, and scalable typography.
- [ ] Add empty, loading, error, permission-denied, and long-running states.
- [ ] Conduct usability testing with non-technical users.

**Acceptance:** Core flows are usable without documentation; keyboard and accessibility checks pass; UI does not freeze during scans or large queries.

### Phase 11 — Performance and packaging
- [ ] Establish measured baseline for startup, idle RAM, scan throughput, and indexing.
- [ ] Optimize packaging and remove unused Qt modules and dependencies.
- [ ] Keep AI models out of the default installer.
- [ ] Make FFmpeg and other heavyweight integrations optional where feasible.
- [ ] Build Windows installer and Linux packages.
- [ ] Test install, upgrade, uninstall, and data preservation.
- [ ] Test offline installation and first launch.
- [ ] Verify application size budgets using actual release artifacts.
- [ ] Produce dependency and third-party license notices.

**Acceptance:** Release artifacts pass clean-machine installation tests; measured footprint and resource use are published; optional components do not silently download.

### Phase 12 — Release readiness
- [ ] Complete end-to-end tests on Windows and Linux.
- [ ] Complete regression, accessibility, security, and performance suites.
- [ ] Verify documentation and CLI/MCP examples against the shipped build.
- [ ] Prepare release notes, known limitations, and recovery guidance.
- [ ] Tag a release only after maintainer review.

**Acceptance:** No known release-blocking defects, no incomplete core flows, reproducible builds, and all release gates signed off.

## 4. Definition of done

A feature is complete only when all applicable conditions are true:

- [ ] Requirements and edge cases are documented.
- [ ] Implementation is complete; no placeholder or mock behavior remains.
- [ ] Unit and integration tests pass.
- [ ] Windows and Linux behavior is verified or limitations are documented.
- [ ] Failure, permission, cancellation, and recovery paths are tested.
- [ ] Performance and resource impact are measured.
- [ ] GUI, CLI, MCP, and API surfaces are updated where applicable.
- [ ] Security and privacy implications are reviewed.
- [ ] User and developer documentation is updated.
- [ ] Review confirms the feature is production-ready.

## 5. Change control

Keep this plan current. Do not mark partial work complete. For major architecture changes, document the proposal and open a reviewable pull request before merging. Prefer small, focused changes. Never weaken safety or offline requirements merely to meet a schedule or size target.
