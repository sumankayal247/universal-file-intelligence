# Agent Integration

**Status:** Planned. No MCP server or agent tools have been implemented yet.

## 1. Goal

Allow a user's own local AI agent to discover and use UFI's supported capabilities without operating the desktop GUI. UFI must remain useful without an AI agent. Agent integration is an additional interface to the offline core engine, not a cloud AI dependency.

## 2. Integration surfaces

### MCP over STDIO — primary integration
- The agent launches a UFI MCP process locally.
- Requests and responses use standard input/output.
- No listening network port is required.
- Diagnostics go to standard error, never protocol output.
- The process uses the same user configuration and core services as the desktop app.
- Access is constrained by an explicit agent permission profile.

### MCP over HTTP — optional
- Bind to loopback only by default.
- Require a per-installation or per-session authentication secret.
- Reject unexpected Host and Origin values where applicable.
- Do not expose the service to LAN interfaces by default.
- Provide explicit start/stop controls and visible status.
- Use transport and authentication patterns supported by the selected MCP SDK.
- Do not assume loopback binding alone is a complete security boundary.

### CLI
Provide stable commands for scripting, diagnostics, and automation. CLI commands call the same capability services and policy checks as MCP and GUI.

### Local API / IPC
A versioned local interface may be provided for custom applications. Choose IPC or loopback HTTP after prototype and threat-model review. Avoid maintaining a second business-logic implementation.

## 3. Proposed tool catalogue

Names are illustrative and must be finalized with schemas and versioning.

| Category | Candidate tools | Default access |
|---|---|---|
| System | get_system_info, get_drives | Read |
| Scan | scan_directory, get_scan_status, cancel_scan | Read / scoped start |
| Storage | analyze_storage, find_large_files | Read |
| Duplicates | find_duplicates, compare_files | Read |
| Search | search_files, search_by_content | Read |
| Metadata | get_file_metadata, classify_files | Read |
| Organization | suggest_organization, preview_moves | Read / plan |
| Rename | suggest_renames, preview_renames | Read / plan |
| Archives | list_archive_contents, test_archive | Read |
| Integrity | check_file_integrity, find_corrupted_files | Read |
| Actions | create_action_plan, execute_action_plan | Approval-gated |
| History | get_action_history, undo_action | Read / policy-gated |

Every tool needs bounded inputs, explicit outputs, clear errors, cancellation for long jobs, and documented permission requirements.

## 4. Preferred agent workflow

1. Agent discovers tools and reads UFI capability descriptions.
2. Agent checks allowed roots and available permissions.
3. Agent searches or analyzes files using read-only tools.
4. Agent creates a typed action plan.
5. UFI validates paths, scope, collisions, risk, and current file state.
6. UFI returns a human-readable preview.
7. User approves the exact plan through the configured approval mechanism.
8. Agent requests execution using the approved plan identifier.
9. UFI revalidates approval, permissions, and source state before each operation.
10. UFI records results and returns an audit reference.
11. Agent can inspect history and request supported undo.

An agent must not turn a read-only grant into a mutation grant by changing tool arguments.

## 5. Permission model

Proposed tiers:
- **Read-only:** inspect configured roots, metadata, scan results, and search results.
- **Plan-only:** create proposed actions and previews, but cannot execute them.
- **Approved actions:** execute a specific user-approved plan.
- **Trusted automation:** optional advanced mode with narrow scopes and configurable operation limits.

Permissions should be scoped by agent identity, allowed root directories, capability identifiers, operation risk, maximum affected files/bytes, approval requirements, and expiration/revocation.

Do not provide arbitrary shell execution, arbitrary Python execution, or unrestricted filesystem access through MCP.

## 6. Agent Center

Users should be able to view connected agents, transport, last connection, granted roots, capabilities, pending approvals, active jobs, recent tool calls, and action outcomes. Provide pause, disconnect, and revoke controls.

Approval must be tied to a specific action-plan version and exact affected items. If the plan changes, approval is invalidated.

## 7. Tool contract standards

Each tool definition must document:
- Stable name and semantic version.
- Purpose and non-goals.
- Input schema with limits and validation.
- Output schema and pagination.
- Permission and approval requirements.
- Side effects and idempotency.
- Cancellation and progress semantics.
- Error codes and examples.
- Compatibility policy.

Large results should be paginated or returned as job/result references.

## 8. Offline and privacy requirements

- MCP STDIO works without internet access.
- No telemetry or remote model call is required.
- Do not transmit file contents or metadata to a remote service.
- Tools operate only on UFI-authorized roots.
- Logs avoid recording sensitive file contents.
- Any future optional network feature is disabled by default and clearly disclosed.

## 9. Acceptance criteria

- A local MCP client launches UFI over STDIO while offline.
- It discovers and invokes read-only tools.
- It cannot read outside configured roots.
- It can create a plan but cannot execute it without required approval.
- Revoking an agent removes its access.
- Tool schemas and errors are documented.
- GUI, CLI, and MCP results are consistent because they use the same core services.
