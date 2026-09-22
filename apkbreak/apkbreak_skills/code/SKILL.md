---
name: code
description: "The user requests recovering assemblies, scripts, bytecode, or other code materials from unpacked and organizing them into code outputs."
---

# Recover Unpacked Code and Organize Code Projects

Before execution, read the [naming rules](../../apkbreak_rules/naming.md), [layout rules](../../apkbreak_rules/layout.md), [execution rules](../../apkbreak_rules/execution.md), and [delivery rules](../../apkbreak_rules/delivery.md). The user's explicit instructions take precedence over default conventions.

When calling another skill, read its entry point through the [skill index](../../apkbreak_docs/skills.md). The main workflow and subflows share one task record. Use the [record templates](../../apkbreak_context/README.md), and confirm available tools through the [tool inventory](../../apkbreak_tools/README.md).

- Path base: the repository root is apkbreakdown; management directories are under apkbreak/, intermediates under apkbreak/apkbreak_unpacked/, and outputs remain at the repository root. Recorded paths are relative to the repository root.

- Use case: recover assemblies, scripts, bytecode, or other code materials from unpacked and organize them into code outputs.
- Required inputs:
  - unpacked directory.
  - Game and project version, from info or the upstream task.
  - Scope, such as all code or specified assemblies/script modules.
- Outputs:
  - xxx_code_outputs created according to naming rules.
  - Readable code with a project or browsing entry point.
  - Code indexes, project-structure notes, recovery-failure reports, and missing-content reports.
- Boundaries:
  - Do not modify raw materials in unpacked.
  - Do not download remote code or invent business logic.
  - Project files and reading guides may be generated, but do not fabricate classes, method implementations, or return values to make compilation pass.
  - Distinguish decompiled code, type/field structures, instruction-level results, and added explanations; do not call them original source.
  - Record Rider accessibility, readability, and compilation separately. Default validation covers readability, recovery level, indexes, and project entry points. Reading/analysis tasks do not require forced compilation; follow explicit validation skips.
- Steps:
  - Read info and context to confirm version, scope, and reusable stages.
  - Process assemblies, Lua/other bytecode, DEX, IL2CPP, and other materials according to their actual types; do not assume all games can be recovered into C# projects.
  - Record input-to-output mappings. Record individual file/method failures and continue independent work.
  - Preserve namespaces, modules, and directory relationships. Generate necessary Rider project files where suitable; otherwise provide directory entry points and reading instructions without forcing a compilable project.
  - Index type, source, output location, and recovery level. Update context at start, stage completion/failure, and finish; record artifact/index entry points at finish. Update newly confirmed APK technology information in info. Share the task record when called by another skill.
- Processing by material:
  - JADX/baksmali DEX output is Android shell, SDK, or JNI code, not necessarily Unity business C#. Count Mono assemblies, script bytecode, IL2CPP metadata, and Native implementations separately.
  - Package resources and Native libraries copied into a source project's resources by tools such as JADX do not count as source code. Retain raw payloads in unpacked; code outputs contains reading material, project metadata, and explicitly classified analysis materials.
  - Keep instruction-level results if high-level decompilation fails and record the downgrade scope. Strings, type names, and signatures are not recovered method logic.
- Unity IL2CPP:
  - Obtain libil2cpp.so and global-metadata.dat from the same version and installation snapshot. Separate Native files by ABI, recording paired paths, hashes, ABI, Unity, and metadata versions. Do not mix versions or architectures.
  - Retain existing ScriptingAssemblies.json, RuntimeInitializeOnLoads.json, DEX, and Unity Data as evidence for assemblies, initialization entry points, Android code, and resource bindings.
  - Check tool versions, architectures, and runtimes before Il2CppDumper/Cpp2IL. Handle missing runtimes under shared environment rules. Apply compatibility configuration to working copies and record actual versions. Check RequireAnyKey, Console.ReadKey, or similar waits before noninteractive execution.
  - "may be protected" or "find JNI_OnLoad" are hints only; assess registration locations and actual artifacts. If failure occurs only at an input wait and core artifacts are validated, record "generation complete; process shutdown exception", not total failure or unqualified success.
  - DummyDll contains type, field, and method-signature metadata stubs, not original method bodies. dump.cs provides layout/offsets, script.json maps Native addresses, and stringliteral.json supplies string clues; label their roles.
  - If method logic is needed and supported, Cpp2IL may recover IL for decompilation; this is reconstruction. Actual Native implementations require address mapping and read-only disassembly; resource MonoScript/MonoBehaviour data only supplements bindings/configuration.
  - Forced Unity version parameters require version evidence. If Cpp2IL reports ELF addresses outside Program Header Table ranges and the issue is traced to Native Method Detection compatibility, confirm the tool version supports disabling nativemethoddetector before retrying in independent outputs. Record the skipped layer and reliability limits; this is not a universal default.
  - Independently read generated assemblies, check type/method/field counts against existing assembly lists, and count business/framework assemblies separately. Record empty bodies, exception stubs, and undecompilable regions.
  - For Native tasks, confirm target ABI, address ranges, and key RVA/VA locations. Do not expand unrelated tasks into Native analysis. Openable projects or bulk C# generation do not prove method recovery.
- Registered tools and format experience:
  - Convert confirmed Sample Game A Lua 5.3 format-1 headers with [normalize_lua53_chunk](../../apkbreak_tools/games/sample_game_a/normalize_lua53_chunk.md) on copies, then use compatible Lua tools. Do not apply to other versions/field sizes; header conversion is not logic recovery.
  - Use [restore_mdl_assemblies](../../apkbreak_tools/games/sample_game_a/restore_mdl_assemblies.md) for confirmed .mdl files with only the DOS prefix altered. Independently check CLR metadata/decompilation afterward; MZ alone does not establish success.
  - Use [build_ghidra_targets](../../apkbreak_tools/common/build_ghidra_targets.md) for IL2CPP address lists within user scope. Distinguish RVA, VA, and Ghidra load base; do not add the base again unconditionally. This tool does not generate Native pseudocode.
- Completion criteria:
  - Specified materials have been processed, classified, and indexed.
  - State which methods have recovered logic, which have only structures/instructions, and which failed or remain unprocessed.
  - Report project opening/compilation only according to performed checks.
  - Mark unmet scope as partial.
- Exceptions:
  - Missing tools or unsupported formats: record blockers and retain inputs.
  - Individual module failure: retain successes and continue independent modules.
  - Abnormal tool exit after generation: mark results awaiting checks; file counts do not establish completeness.
  - Follow apkbreak_rules for interruptions, duplicate names, and overwrites.
