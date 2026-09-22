---
name: extract
description: "Use when the user provides an APK and requests parsing, unpacking, reverse engineering, or similar APK analysis."
---

# Extract Resources from an APK

Before execution, read the [naming rules](../../apkbreak_rules/naming.md), [layout rules](../../apkbreak_rules/layout.md), [execution rules](../../apkbreak_rules/execution.md), and [delivery rules](../../apkbreak_rules/delivery.md). The user's explicit instructions take precedence over default conventions.

When calling another skill, read its entry point through the [skill index](../../apkbreak_docs/skills.md). The main workflow and subflows share one task record. Use the [record templates](../../apkbreak_context/README.md), and confirm available tools through the [tool inventory](../../apkbreak_tools/README.md).

- Path base: the repository root is apkbreakdown; management directories are under apkbreak/, intermediates under apkbreak/apkbreak_unpacked/, and outputs remain at the repository root. Recorded paths are relative to the repository root.

This skill coordinates the other skills to complete the reverse-engineering workflow.

- Use case: the user provides an APK and requests parsing, unpacking, reverse engineering, or similar APK analysis.
- Required inputs: package filename, already placed in apkbreak_apks. Accept APK, XAPK, APKS, and other supported packages. Specify art, code, config, multiple types, or all.
- Rules:
  - Do not ask again for resource types the user already specified.
  - Extract all package-contained resources of the selected types by default; do not ask for priority modules such as combat, city building, or number gates.
  - Do not create special reports for priority resources. Keep a unified file index, failure list, and acceptance results; use the parsed-resource query/analysis skill for later specific lookups.
  - Pass the full package-contained scope of selected types to conversion skills without asking again for priority modules.
  - If art is selected and Unity import is required, obtain the target Unity project directory beforehand and respect the rule against creating projects independently.
  - Record the project version when identifiable; ask if it cannot be identified and do not guess.
- Steps:
  - Identify formats from content and use registered, applicable tools. Record gaps when tools are missing instead of relying on team members' personal skills.
  - Follow the naming rules under apkbreak_rules.
  - Do not call the remote-resource download skill.
  - Read info and historical records, assess reuse from input hashes, scope, and stage state, and process only missing items or those requiring regeneration.
  - Identify package structure, technology stack, and project version.
  - Extract/decrypt as needed, using apkbreak_tools where applicable.
  - Update context at task start, each stage completion/failure, and finish, recording inputs, tools/parameters, outputs, status, and continuation.
  - Save extracted results in unpacked.
  - Call conversion skills according to type:
    1. Code: recover unpacked code and organize code projects.
    2. Resources: convert unpacked art resources and import into Unity.
    3. Tables: restore unpacked data tables and export to Excel.
- Package structure and technology identification:
  - For XAPK, APKS, split APKs, and asset packs, first inventory container members and inspect base, configuration splits, and resource packs separately. Base-only checks do not prove complete package extraction.
  - According to selected types, inspect assets, res, lib, classes*.dex, Manifest, resources.arsc, StreamingAssets, and manifests. If plaintext code is absent, identify assemblies, bytecode, Native code, script containers, and compressed/encrypted materials; extension-search failure is not proof of absent code.
  - Technology clues include Mono assemblies, IL2CPP libil2cpp.so/global-metadata.dat, Lua, JavaScript, WASM, Flutter assets, and Cocos scripts. Register observed types only; do not assume Unity.
  - Select tools by materials: aapt2, apkanalyzer, or apktool for APK metadata; JADX/baksmali for DEX; delegate other materials to conversion skills. Confirm availability, versions, and dependencies; named tools are not necessarily installed.
  - Match manifest entries to physical files and count base-built-in, split/asset-pack-built-in, remote declarations, and missing entities separately. Remote declarations do not prove local resources exist.
  - For "all", cover every physical source for the selected types. Delegate object identification to container, not filename keywords. Count code/resources separately and record unidentified materials and reasoning.
  - Record package sizes and SHA-256 before/after processing; pause delivery and investigate discrepancies. Store indexes/reports according to shared rules.
- Registered tools:
  - Delegate built-in Bundle aggregate files to container and code/tables to corresponding conversion skills. Do not apply Sample Game A-specific tools to unconfirmed formats.
- Completion criteria:
  - Requested content has been organized into outputs according to conversion-skill standards.
  - Record successes, failures, omissions, unprocessed items, and unchecked items separately in context.
  - Record unpacked/outputs locations, sources, and processing history.
  - Record artifact/index/report entry points in context and newly confirmed APK information in info.
  - Mark unfinished work partial; creating outputs alone does not mean full success.
- Exceptions:
  - Interrupted tasks: inspect stage records, reuse applicable completed work, and resume from the first incomplete/failed/invalidated stage. Do not treat partial artifacts as completed.
  - Directory name collisions: confirm the directory belongs to the same input/task; replace only files managed by this task. Retain/report unknown sources, different versions, and manually modified files.
  - Missing resources: label manifest-listed resources absent from the package as "not provided in package"; do not download automatically or count them as extraction failures.
  - Parsing failure: record the reason for the affected resource type, continue independent types, and summarize each separately.
