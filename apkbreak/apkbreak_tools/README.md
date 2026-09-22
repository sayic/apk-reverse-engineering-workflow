# Shared Tools

An initial set of 10 command-line tools has been organized from an older project, divided into common tools and Sample Game A-specific tools. Version 1.0; current validation covers synthetic samples and simulated transfers, not real packages or Unity projects.

| Tool | Usage and boundaries |
| --- | --- |
| Lua 5.3 header conversion | [normalize_lua53_chunk](games/sample_game_a/normalize_lua53_chunk.md) |
| MDL assembly header restoration | [restore_mdl_assemblies](games/sample_game_a/restore_mdl_assemblies.md) |
| Positional field and vExt restoration | [resolve_table_vext](games/sample_game_a/resolve_table_vext.md) |
| Built-in Bundle extraction | [extract_builtin_bundles](games/sample_game_a/extract_builtin_bundles.md) |
| Manifest downloads and validation | [download_bundles](games/sample_game_a/download_bundles.md) |
| GUID comparison | [verify_meta_guids](common/verify_meta_guids.md) |
| Unity resource static checks | [inspect_unity_assets](common/inspect_unity_assets.md) |
| AssetRipper HTTP calls | [run_assetripper_export](common/run_assetripper_export.md) |
| Ghidra method target list | [build_ghidra_targets](common/build_ghidra_targets.md) |

Each usage document sits alongside its same-named .py file; read the document before running --help. Share supporting scripts, documentation, and empty templates in apkbreak_tools, excluding cache and local inventories. Do not copy only individual scripts: common/tooling.py and games/sample_game_a/gameres.py are internal dependencies.

## Execution conventions

- Python 3.10+. Apart from optional PNG decoding with Pillow, local scripts use only the standard library. AssetRipper calls require an already-running compatible external program. There are no automatic dependency-installation or environment-upgrade steps.
- Keep original inputs read-only and output to a new candidate directory under the task's unpacked location. Retain/report same-named files with different contents; no force-overwrite option is provided. Follow shared rules for formal replacements.
- Put reports in context for the current run with new filenames each time. Download report directories must be new. Callers save stdout/stderr to logs; the main skill maintains summary, status, and formal change inventories.
- Exit code 0 means completion only within documented tool scope, not whole-task acceptance. Retain successes after partial failures; reuse by hash on continuation without automatic cleanup.
- Directory arguments must not point to workspace roots or entire drives for exhaustive checks. Directory checks exclude caches/version-control metadata and do not follow directory links; exclusions are outside checked scope.

## Validation and Provenance

Synthetic regression:

```powershell
python -B -X utf8 apkbreak/apkbreak_tools/tests/test_tools.py
```

Tests use temporary directories and simulated HTTP, without real downloads or calls to Unity, AssetRipper, or Ghidra. External tools and compatibility with real format versions still require task-specific validation.

## Capabilities Not Yet Migrated

- Target business-table mapping tools: vExt restoration is not target-project field mapping.
- RenderDoc, ILSpy, LuaDec, AssetRipper, Unity MCP: external programs have not been copied or installed; actual tasks must register versions/environments.
- HDR restoration script: the original depends on old indexes and includes lossy tone mapping. Useful format/color-processing knowledge is in art/repair skills; it is not a default color-repair tool.
- Ghidra Java batch export, Sample Game B-specific payload processing, and historical mounting/migration scripts: retained in the old project. Only reusable portions are adopted; version-specific settings are not presented as general capabilities.

## Tool Cache and Reuse

These conventions apply to all skills. The cache retains tools acquired during actual tasks; it does not require downloading a complete toolset in advance or extend installation, download, or upgrade authorization.

1. Check apkbreak/apkbreak_tools/registry.local.json before calls, then inspect the referenced cache/installation. Confirm existence, version, platform, dependencies, and input compatibility; registration does not prove current availability.
2. Store portable tools such as ILSpy/Il2CppDumper under apkbreak/apkbreak_tools/cache/<tool_name>/<version>/<platform>/, with OS and architecture in the platform identifier. Retain full release directories and dependencies, not just entry files. Keep versions side by side without overwriting versions still used by tasks.
3. Stage new downloads separately, then move them into cache only after checking completion, source, and integrity. Do not register partial downloads as available. Retain/report same-version different-content conflicts without silent overwrites. Record publisher checksums separately from locally calculated hashes; a local hash does not prove source authenticity.
4. For Unity, system runtimes, and installed tools, register entry points/dependencies without moving installations. If tools need configuration/runtime-parameter changes, use working copies in task directories to avoid altering shared caches.
5. After tools are ready, update local inventories with versions, entry points, sources, hashes, dependencies, and confirmation status. Task records separately capture actual versions, parameters, and logs; caches do not hold business artifacts.
6. Record missing, damaged, or incompatible caches and obtain replacements or select versions within current authorization. Do not retry indefinitely, automatically upgrade, or clean old versions.

Copy the [empty inventory](templates/registry.json) to apkbreak/apkbreak_tools/registry.local.json. Create inventories/cache directories only on first use; do not put actual environment data into public templates.

Each tools entry uses these fields:

| Field | Meaning |
| --- | --- |
| name, version, platform | Tool name, exact version, OS, and architecture |
| mode | cached or installed |
| entry_path | Entry point; repository-relative for cached tools, local absolute path allowed for installed tools |
| source | Acquisition source without authentication data or signature parameters; store credentials separately |
| hashes | File path, SHA-256, and role (such as release archive or entry point); locally calculated |
| publisher_checksum | Publisher-provided algorithm/value; null if absent |
| dependencies | Runtime/version requirements; empty array if none |
| status, checked_at | available, unverified, or unavailable, and actual confirmation time; null when unconfirmed |
| notes | Format compatibility, limitations, or unavailability reasons |

available only means the tool environment has been confirmed available; it does not prove successful processing of a particular package. Put download addresses, local paths, and actual hashes only in the ignored local inventory.
