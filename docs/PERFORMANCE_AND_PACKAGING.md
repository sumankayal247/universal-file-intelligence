# Performance and Packaging

**Status:** Target budgets only. Actual artifact sizes and resource usage must be measured from release builds.

## 1. Product principle

UFI should not become a storage problem while helping users manage storage. Keep the base install small, avoid bundled models, and make heavyweight integrations optional.

## 2. Proposed size targets

| Metric | Target |
|---|---:|
| Initial Windows installer download | Aim for 100 MB or less |
| Initial Linux package download | Aim for 100 MB or less |
| Core installed footprint | Aim for 150 MB or less |
| Preferred upper core footprint | 200 MB |
| Bundled AI model size | 0 MB |
| Default cache | Configurable; initial budget up to 100 MB |

These are product goals, not verified facts or guarantees. PyQt6 and Python runtime packaging may make the initial targets difficult. Measure compressed download and installed size separately on each platform.

Do not compromise security, correctness, accessibility, or required functionality solely to meet a size target. If the measured build is larger, publish the result and optimize based on evidence.

## 3. Dependency strategy

- Bundle only the Qt modules actually used.
- Avoid shipping multiple Python environments.
- Use one shared runtime for desktop, CLI, and MCP where practical.
- Prefer standard-library features where suitable.
- Keep optional parsers and media tooling in optional dependency groups.
- Do not bundle local LLMs or embedding models.
- Avoid duplicate libraries with overlapping functionality.
- Audit licenses, native binaries, and transitive dependencies.
- Generate third-party notices for each release.

## 4. Optional components

Potential optional components include video metadata and frame analysis, additional document extractors, advanced image processing, local embedding models, and additional archive handlers.

Optional components must be clearly described, version-compatible, removable, and installed only with user consent. Core scanning, exact hashing, metadata search, and MCP STDIO must not depend on them.

## 5. Storage footprint inside user data

| Data | Proposed handling |
|---|---|
| Application binaries | Installation directory |
| SQLite metadata | User application-data directory |
| Full-text index | Local database or dedicated local index |
| Thumbnails | Configurable cache |
| Temporary scan data | Bounded and cleaned up |
| Quarantine | User-selected or clearly documented location |
| Optional models | User-selected location |

Users should be able to inspect storage usage, configure cache limits, clear rebuildable caches, and choose data locations. Clearing a cache must not delete original files or required recovery records.

## 6. Performance requirements

Measure:
- Cold and warm startup.
- Idle memory and CPU.
- Scan throughput on HDD, SATA SSD, NVMe, and removable media.
- Hashing throughput and I/O impact.
- Search latency for small and large indexes.
- UI responsiveness during scans.
- Cancellation latency.
- Database growth.
- Cache growth and eviction.

Do not publish performance claims until measured on documented hardware and datasets.

## 7. Resource controls

- Bounded worker pools.
- Configurable scan intensity.
- Pause, resume, and cancel.
- Chunked file reads and hashing.
- Avoid loading entire large files into memory.
- Backpressure for filesystem events.
- Bounded thumbnail generation.
- Cache eviction policy.
- Exclusions for system and application directories.
- Graceful behavior on low-memory machines.

## 8. Packaging strategy

Evaluate PyInstaller and native platform packaging through prototypes.

### Windows
- Signed installer when signing becomes available.
- Clean install, upgrade, repair, and uninstall tests.
- Avoid requiring administrator privileges unless necessary.
- Preserve user data during uninstall unless explicitly requested.

### Linux
- Evaluate AppImage, Flatpak, and distribution-native packages.
- Document sandbox permissions and filesystem access.
- Test on selected distributions and desktop environments.
- Do not assume all distributions share the same libraries or paths.

## 9. Release validation

For every release candidate:
- Measure compressed installer/package size.
- Measure installed footprint.
- Inventory bundled dependencies.
- Verify offline installation and startup.
- Test upgrade and uninstall.
- Verify no model or optional dependency is downloaded automatically.
- Test on a clean machine or VM for each supported platform.
- Publish system requirements and known limitations.
