# UX and Design Direction

**Status:** Product design direction; visual system and interaction prototypes are not yet implemented.

## 1. Product experience

UFI should feel like a polished, approachable desktop product rather than a technical disk-cleaning console. It must remain powerful for advanced users without making ordinary users understand filesystem jargon.

Design for everyday users who want to understand storage and advanced users who need filters, metadata, automation, and agent control.

## 2. Design principles

- Clear hierarchy and calm visual density.
- Light and dark themes.
- Accessible contrast, focus states, and scalable typography.
- Consistent icons and plain-language labels.
- Progressive disclosure: simple defaults, advanced controls when needed.
- Useful empty, loading, error, permission-denied, and offline states.
- Responsive layouts for different desktop sizes.
- Keyboard navigation and discoverable shortcuts.
- Never use color alone to communicate status.
- Never disguise destructive actions as ordinary navigation.

## 3. Proposed navigation

1. **Overview:** drive summary, recent discoveries, scan status, and recommendations.
2. **Storage Explorer:** drives, folder drill-down, treemap, large files, and category breakdowns.
3. **Duplicates:** exact duplicates, similar images, comparison, and review.
4. **File Intelligence:** file types, metadata, forgotten files, integrity findings, and classification.
5. **Search:** natural-language filters, advanced filters, full-text search, and saved searches.
6. **Organization:** folder suggestions, rename proposals, and bulk-operation previews.
7. **Action Center:** pending approvals, dry runs, active jobs, results, quarantine, history, and undo.
8. **Agent Center:** connected agents, transports, permissions, tool catalogue, active calls, and audit history.
9. **Settings:** scan roots, exclusions, resource limits, storage locations, privacy, and appearance.

Navigation may change after usability testing. Do not add a screen merely to mirror a backend module.

## 4. Overview dashboard

Potential content:
- Total, used, and free storage by volume.
- Storage distribution by category.
- Largest folders and recent storage growth.
- Scan status and last successful scan.
- Duplicate candidates and other findings requiring review.
- Recent actions and recovery status.
- Agent connection status, if configured.

Every metric must identify its source and freshness. Estimates and candidates must not be presented as confirmed reclaimable space.

## 5. Search experience

Provide one prominent search entry point with:
- Natural-language filter parsing that works locally.
- Suggestions for supported query patterns.
- A visible interpretation of the parsed query.
- Advanced filter controls.
- Search scope and index freshness.
- Clear distinction between filename search and extracted-content search.

Example: “Find videos larger than 500 MB modified before 2022 in Downloads.”

If the query is ambiguous, ask the user to clarify or show interpreted filters before running an expensive scan.

## 6. Review-first interactions

For bulk operations, show:
- Number of affected files and total bytes.
- Source and destination summary.
- Conflicts and skipped items.
- Whether an operation is reversible.
- Expected disk-space impact.
- Required permissions.
- Exact approval action.

Do not rely on one generic Clean button for unrelated operations.

## 7. Agent Center

Users should be able to add/remove an agent profile, view supported connection methods, copy setup instructions, inspect granted roots and capabilities, switch access modes, review pending plans, see active jobs and recent calls, and revoke access immediately.

Agent activity should be understandable without reading protocol logs.

## 8. Accessibility and usability

- Keyboard-operable navigation and dialogs.
- Visible focus indicators.
- Screen-reader-friendly labels and table semantics where supported.
- Scalable text and high-contrast support.
- Non-color status indicators.
- Confirmation dialogs that explain consequences.
- Avoid animation that obscures progress or causes discomfort.
- Test with users unfamiliar with storage terminology.

## 9. UI technology

PyQt6 is the current proposal. Evaluate widget choices and rendering performance with real data before committing to a large custom UI framework. Keep UI code separate from domain and application services.

## 10. Validation

Before declaring UX complete:
- Conduct task-based tests with non-technical users.
- Test large result sets and long-running scans.
- Test small windows and display scaling.
- Test keyboard-only workflows.
- Test dark and light themes.
- Verify destructive actions are clear and difficult to trigger accidentally.
