# Tool Documentation

This package includes nine tool interface and usage documents, not their executable Python implementations. The names and examples describe expected behavior; they do not prove that any tool is installed, available, or validated for the user's input.

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

## Before running a tool

1. Inspect the task workspace's tools/registry.local.json and actual installed programs. Confirm version, platform, dependencies, and input compatibility.
2. Obtain a matching implementation only within the user's task authorization. If it is unavailable, report the dependency or use a confirmed compatible tool within scope.
3. Replace <configured-tool-directory> in examples with the actual implementation location. Keep the complete distribution and required modules; documentation filenames are not executables.
4. Run commands from the user-selected task workspace. Inputs, candidate outputs, and report paths are workspace-relative; installed references remain read-only.
5. Keep raw inputs, use new candidate/report locations, and follow the shared conflict rules. Record exit codes, failures, and unchecked scope.
6. Do not claim that installation of this skill installs AssetRipper, ILSpy, LuaDec, Ghidra, Unity, Python, or an MCP server.

The documented interfaces expect Python 3.10+; PNG decoding may need Pillow, and AssetRipper automation requires a compatible running service. Check the actual implementation's requirements. Tools and real APK/Unity processing have not been validated by packaging this skill. No test program is bundled.

## Tool Cache and Reuse

These conventions apply to all workflows. The cache retains tools acquired during actual tasks; it does not require downloading a complete toolset in advance or extend installation, download, or upgrade authorization.

1. Check tools/registry.local.json before calls, then inspect the referenced cache/installation. Confirm existence, version, platform, dependencies, and input compatibility; registration does not prove current availability.
2. Store portable tools such as ILSpy/Il2CppDumper under tools/cache/<tool_name>/<version>/<platform>/, with OS and architecture in the platform identifier. Retain full release directories and dependencies, not just entry files. Keep versions side by side without overwriting versions still used by tasks.
3. Stage new downloads separately, then move them into cache only after checking completion, source, and integrity. Do not register partial downloads as available. Retain/report same-version different-content conflicts without silent overwrites. Record publisher checksums separately from locally calculated hashes; a local hash does not prove source authenticity.
4. For Unity, system runtimes, and installed tools, register entry points/dependencies without moving installations. If tools need configuration/runtime-parameter changes, use working copies in task directories to avoid altering shared caches.
5. After tools are ready, update local inventories with versions, entry points, sources, hashes, dependencies, and confirmation status. Task records separately capture actual versions, parameters, and logs; caches do not hold business artifacts.
6. Record missing, damaged, or incompatible caches and obtain replacements or select versions within current authorization. Do not retry indefinitely, automatically upgrade, or clean old versions.

Copy the [empty inventory](../templates/tools/registry.json) to tools/registry.local.json. Create inventories/cache directories only on first use; do not put actual environment data into public templates.

Each tools entry uses these fields:

| Field | Meaning |
| --- | --- |
| name, version, platform | Tool name, exact version, OS, and architecture |
| mode | cached or installed |
| entry_path | Entry point; workspace-relative for cached tools, local absolute path allowed for installed tools |
| source | Acquisition source without authentication data or signature parameters; store credentials separately |
| hashes | File path, SHA-256, and role (such as release archive or entry point); locally calculated |
| publisher_checksum | Publisher-provided algorithm/value; null if absent |
| dependencies | Runtime/version requirements; empty array if none |
| status, checked_at | available, unverified, or unavailable, and actual confirmation time; null when unconfirmed |
| notes | Format compatibility, limitations, or unavailability reasons |

available only means the tool environment has been confirmed available; it does not prove successful processing of a particular package. Put download addresses, local paths, and actual hashes only in the ignored local inventory.
