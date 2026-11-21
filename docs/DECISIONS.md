# Decision Log

This file records decisions and constraints established during product planning. Decisions may be revised, but changes must include a reason and impact assessment.

## D-001 — Product identity
**Status:** Accepted  
**Decision:** Project name is Universal File Intelligence (UFI). Repository: sumankayal247/universal-file-intelligence.

## D-002 — Target operating systems
**Status:** Accepted  
**Decision:** Windows and Linux are first-class targets.  
**Implication:** Platform-specific filesystem, path, permissions, trash, volume, packaging, and monitoring behavior must be isolated behind adapters and tested on both systems.

## D-003 — Offline-first product
**Status:** Accepted  
**Decision:** Core functionality must work offline, without API keys, cloud accounts, or an installed language model.  
**Implication:** Local rules, signatures, hashing, metadata, SQLite, and local search form the baseline. No mandatory remote AI dependency.

## D-004 — External AI-agent compatibility
**Status:** Accepted  
**Decision:** UFI should be controllable by a user's own local AI agent. MCP is the primary planned integration. CLI and a local programmatic interface are additional surfaces.  
**Implication:** Agent integration is a core architectural requirement, not a late add-on.

## D-005 — Shared capability engine
**Status:** Accepted  
**Decision:** GUI, MCP, CLI, and API must use the same core application services and capability registry.  
**Implication:** Do not duplicate business logic in interface adapters.

## D-006 — Agent permissions
**Status:** Accepted  
**Decision:** Agents must not receive unrestricted operating-system access. Read-only is the default. Mutating operations use scoped permissions, action plans, and approval requirements.  
**Implication:** No arbitrary shell or Python execution through UFI agent tools.

## D-007 — Product scope
**Status:** Accepted  
**Decision:** Build a full-featured professional file intelligence suite, not merely a lightweight disk cleaner or conventional file explorer.  
**Implication:** Storage analysis, duplicates, classification, search, organization, integrity, archives, agent integration, and safety are planned product areas.

## D-008 — Human-friendly UI
**Status:** Accepted  
**Decision:** The application must have a polished, approachable UI suitable for non-technical users while retaining advanced controls. Light/dark themes, accessible design, and review-first workflows are desired.

## D-009 — Technology direction
**Status:** Proposed, pending prototype validation  
**Decision:** Python + PyQt6 desktop; modular Python core; SQLite and FTS5; MCP Python SDK; optional FastAPI/local IPC; platform adapters.  
**Implication:** Do not treat every proposed library as final until dependency, packaging, and cross-platform tests are complete.

## D-010 — Application footprint
**Status:** Accepted as target, not guarantee  
**Decision:** Aim for a core installed footprint around 150 MB, with 200 MB as the preferred upper target. Aim for installer/package downloads around 100 MB where practical.  
**Implication:** Measure real builds, trim unused dependencies, keep models out of the base install, and make heavyweight features optional. Never sacrifice safety or correctness to hit the number.

## D-011 — AI model distribution
**Status:** Accepted  
**Decision:** Bundle no AI models in the default installer.  
**Implication:** Core features must not rely on a model. Optional local models may be user-installed separately.

## D-012 — Video and heavyweight processing
**Status:** Proposed  
**Decision:** Treat FFmpeg and video similarity as optional capabilities unless prototypes show an acceptable footprint and licensing/distribution path.

## D-013 — Safe file operations
**Status:** Accepted  
**Decision:** No silent destructive cleanup. Use previews, explicit approval, audit history, quarantine, and undo where technically possible.  
**Implication:** Never promise every operation is reversible; make limitations visible.

## D-014 — Work-in-progress transparency
**Status:** Accepted  
**Decision:** Repository and documentation must clearly state that the project is in planning/WIP until implementation and release gates are complete. Planned features must not be described as implemented.

## D-015 — Project license
**Status:** Accepted
**Decision:** Use Apache License 2.0 (SPDX: Apache-2.0) for the project. The license permits reuse, modification, redistribution, and commercial use subject to its conditions, and includes an express contributor patent grant. Third-party dependencies retain their own licenses and notices. See LICENSE.

## Open decisions

- Minimum supported Windows versions and Linux distributions.
- Exact Python version and dependency management strategy.
- PyQt6 packaging approach and Qt module selection.
- MCP SDK version, transports, and client compatibility matrix.
- Persistent local service versus on-demand process lifecycle.
- Local API versus IPC choice.
- Exact database schema and file identity model.
- Filesystem event strategy on Windows and Linux.
- Supported document, archive, image, audio, and video formats.
- Quarantine location and operating-system trash integration.
- Final installer formats and code-signing strategy.
- Whether external contributions need a separate contributor agreement.
