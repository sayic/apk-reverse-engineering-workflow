---
name: query
description: "The user requests finding resources, code, configuration values, or business logic in existing outputs, such as level IDs, initial projectile speeds, effect paths, or field meanings."
---

# Query and Analyze Parsed Resources

Before execution, read the [naming rules](../../apkbreak_rules/naming.md), [layout rules](../../apkbreak_rules/layout.md), [execution rules](../../apkbreak_rules/execution.md), and [delivery rules](../../apkbreak_rules/delivery.md). The user's explicit instructions take precedence over default conventions.

When calling another skill, read its entry point through the [skill index](../../apkbreak_docs/skills.md). The main workflow and subflows share one task record. Use the [record templates](../../apkbreak_context/README.md), and confirm available tools through the [tool inventory](../../apkbreak_tools/README.md).

- Path base: the repository root is apkbreakdown; management directories are under apkbreak/, intermediates under apkbreak/apkbreak_unpacked/, and outputs remain at the repository root. Recorded paths are relative to the repository root.

- Use case: find resources, code, values, or logic in existing outputs, such as level IDs, initial projectile speeds, effect paths, and field meanings.
- Required inputs:
  - Game name and query.
  - Project version; if unspecified, inspect available versions in info and ask only if version ambiguity affects the answer.
  - Scope or known clues, such as modules, table/resource names, record IDs.
- Outputs:
  - Conclusions and evidence locations.
  - Relevant code, table, and resource relationships.
  - Version differences, inferences, evidence gaps, and task records.
- Boundaries:
  - Query existing outputs only; do not read unpacked or automatically extract, download, repair, or modify data.
  - Prefer the specified version and distinguish cross-version evidence; do not combine multiple versions into one conclusion.
  - Cross-analysis of code, config, and art outputs is allowed.
  - Matching filenames, keywords, or fields alone do not confirm behavior.
  - "Not found in outputs" does not mean "absent from the original game".
  - Do not execute extracted code; distinguish static analysis from runtime validation.
- Steps:
  1. Establish the requested object/value/behavior, confirm APK/project versions from info, then locate outputs/indexes through context.
  2. Prefer relevant indexes, field documentation, and analysis notes before artifacts. Match historical conclusions to the current version.
  3. Narrow directories/types and search names, IDs, fields, or references; do not scan all games/versions by default.
  4. For values, trace fields, read code, and calculations. For resources, trace logical paths, Prefabs, materials, and actual references. For behavior, trace entry points, branches, and configuration conditions.
  5. Distinguish original values, calculated values, and runtime-dependent results. Mark unsupported inferences and identify missing evidence.
  6. Give the direct conclusion first, followed by paths, classes/functions, fields, record IDs, and other evidence. Separate version-specific results and differences.
  7. Record the query, versions, evidence entry points, conclusions, and unconfirmed items, sharing the main task record when applicable.
- Keyword and evidence tracing:
  - Cover Chinese/English names, abbreviations, synonyms, classes/functions/enums, tables, IDs, UI text keys, Prefabs, and load paths as appropriate. Expand only within established outputs.
  - Start from visible feature text, resource bindings, or code entry points; trace important branches, verify formulas/enums/configuration with actual rows, then confirm visual-resource loading/binding. Not every query requires a full gameplay report.
  - Types, fields, and method names establish structure only. Distinguish IL2CPP metadata stubs, recovered bodies, Native addresses, and resource configuration. State missing implementations in outputs instead of jumping into unpacked for more extraction.
  - For GPU, ECS, Jobs, Burst, or indirect drawing, combine CPU scheduling, Buffer/container fields, Shader/ComputeShader, and Prefab/scene bindings; similar type names do not prove a runtime chain.
  - Check field domains, actual records, and code readers before claiming table relationships; matching names or coincidentally equal values are insufficient.
  - Record module-to-code/key-function-to-table/field-to-resource chains as needed. Mark weak links for confirmation; extraction logs cannot substitute for business conclusions.
- Completion criteria:
  - Answer the question or explain why existing outputs cannot support an answer.
  - Trace conclusions to specific versions, code, configuration, or resources.
  - Distinguish confirmed facts, inferences, and unresolved items.
  - Suggest required additional extraction/runtime validation without performing it automatically.
- Exceptions:
  - Missing requested version/type outputs: state the gap rather than silently using another version.
  - Broken indexes: locate artifacts within agreed outputs without rewriting indexes on your own.
  - Structural-only code or missing key implementations: state recovery limits; do not invent behavior from method names.
  - Conflicting code/data/history: present evidence and conditions separately; preserve unresolved questions.
  - No results: state checked scope and evidence gaps, distinguishing not found, not extracted, and unparseable.
