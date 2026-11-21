# Safety and Security

**Status:** Requirements for implementation. UFI is not yet safe to use on important personal data.

## 1. Security objective

UFI can inspect and modify large amounts of personal data. Prevent accidental data loss, unauthorized access, path escapes, and unsafe parser behavior.

## 2. Threat model

Consider:
- Accidental user selection or bulk-action mistakes.
- A malicious or compromised local AI agent.
- An untrusted MCP client or local process.
- Malicious filenames and filesystem structures.
- Symlink, junction, mount, and path traversal attacks.
- Malformed images, documents, archives, and media files.
- Files changing between scan, preview, and execution.
- Application crash, power loss, or partial filesystem operations.
- Database corruption or stale index records.
- Unauthorized access to a local HTTP interface.

Local software cannot guarantee protection against a fully compromised operating-system account. Document this boundary.

## 3. Default-deny permissions

- No broad filesystem access without user selection.
- No automatic administrator/root escalation.
- No unrestricted shell or code execution.
- No network listener enabled by default.
- MCP agents start with read-only access.
- Mutating actions require explicit authorization according to risk.
- Permission grants are visible, scoped, and revocable.

## 4. Action risk levels

| Risk | Examples | Required behavior |
|---|---|---|
| Low | Search, inspect metadata, analyze storage | Read permission |
| Medium | Create folders, copy files, reversible rename | Plan and configured approval |
| High | Bulk move, bulk rename, quarantine | Exact preview and explicit approval |
| Critical | Permanent deletion, overwrite, destructive cleanup | Avoid by default; separate explicit confirmation and strong safeguards |

Permanent deletion should not be the normal cleanup path. Prefer quarantine or operating-system trash where semantics are reliable.

## 5. Action-plan lifecycle

Draft -> Validated -> Previewed -> Awaiting approval -> Approved -> Revalidated -> Executing -> Completed / Partially completed / Failed -> Reversible where supported.

Approval is bound to an immutable plan version and exact affected objects. Any change to paths, file identity, operation, or destination invalidates approval.

## 6. File-operation safeguards

Before execution:
- Canonicalize and validate source and destination paths.
- Confirm the operation remains inside authorized roots.
- Resolve symlink/junction behavior according to explicit policy.
- Check file identity, size, timestamps, and relevant fingerprints again.
- Detect destination collisions.
- Refuse silent overwrite.
- Verify available storage and permissions where relevant.
- Apply operation limits.
- Record a journal entry before changing data.

During execution:
- Use atomic rename/move primitives when supported.
- Use same-volume staging when possible.
- Verify results after each operation.
- Handle partial failure explicitly.
- Support cancellation without ambiguous state.
- Do not claim atomicity across multiple filesystems or multiple operations.

## 7. Quarantine and recovery

Quarantine should retain original path and volume identity, new quarantine location, metadata and optional verification hash, action-plan and audit identifiers, timestamp, operation result, and recovery instructions.

Quarantine is not a backup. It can still be lost through disk failure, manual removal, or volume changes.

Undo must be advertised only when UFI has enough information and the original data still exists. Overwritten data, permanent deletion, external modifications, and cross-volume edge cases may not be reversible.

## 8. Path and filesystem safety

- Prevent traversal outside approved roots.
- Define symlink and junction traversal behavior.
- Detect loops and repeated targets.
- Treat mount boundaries explicitly.
- Handle case sensitivity and Unicode normalization carefully.
- Avoid trusting path strings supplied by an agent.
- Revalidate paths immediately before mutation.
- Account for race conditions between validation and use.
- Test Windows drive paths, UNC paths, Linux mount paths, and unusual filenames.

## 9. Archive and parser safety

- Never execute files during inspection.
- Treat file contents as untrusted input.
- Set maximum nesting depth, entry count, expanded size, compression ratio, processing time, and memory limits.
- Reject path traversal entries and unsafe absolute paths.
- Avoid extracting archives merely to preview their contents.
- Use maintained libraries and track security advisories.
- Isolate especially risky parsers if the threat assessment requires it.

## 10. Local agent/API security

- STDIO transport is preferred for local agent integrations.
- HTTP transport binds to loopback only by default.
- Require authentication for persistent local services.
- Validate Host and Origin where relevant.
- Do not assume loopback means trusted.
- Keep per-agent grants and audit identity.
- Bound expensive operations.
- Never allow tool arguments to bypass core authorization.
- Do not expose arbitrary command execution.

## 11. Privacy

- No telemetry by default.
- No cloud upload or remote AI inference in core functionality.
- Avoid logging full file contents, secrets, or unnecessary personal paths.
- Provide log retention controls.
- Keep indexes and caches local.
- Clearly explain where databases, thumbnails, quarantine, and optional models are stored.

## 12. Required security testing

- Path traversal and symlink escape tests.
- Malicious archive and decompression-bomb fixtures.
- Malformed file parser tests.
- Race-condition and file-replacement tests.
- Permission and privilege-boundary tests.
- MCP scope and approval bypass tests.
- Local API authentication tests.
- Crash and power-loss simulation for action journals.
- Database corruption and recovery tests.

No destructive capability should ship until its threat review, failure handling, and recovery behavior have been tested.
