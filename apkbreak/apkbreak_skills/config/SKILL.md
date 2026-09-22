---
name: config
description: "The user requests organizing configuration, script tables, binary tables, and similar unpacked materials into readable, queryable data tables."
---

# Restore Unpacked Data Tables and Export to Excel

Before execution, read the [naming rules](../../apkbreak_rules/naming.md), [layout rules](../../apkbreak_rules/layout.md), [execution rules](../../apkbreak_rules/execution.md), and [delivery rules](../../apkbreak_rules/delivery.md). The user's explicit instructions take precedence over default conventions.

When calling another skill, read its entry point through the [skill index](../../apkbreak_docs/skills.md). The main workflow and subflows share one task record. Use the [record templates](../../apkbreak_context/README.md), and confirm available tools through the [tool inventory](../../apkbreak_tools/README.md).

- Path base: the repository root is apkbreakdown; management directories are under apkbreak/, intermediates under apkbreak/apkbreak_unpacked/, and outputs remain at the repository root. Recorded paths are relative to the repository root.

- Use case: organize configuration, script tables, binary tables, and similar unpacked materials into readable, queryable data tables.
- Required inputs:
  - unpacked directory.
  - Game and project version, from info or the upstream task.
  - Scope: all data tables or specified tables.
  - Same-version code outputs, if available, to help interpret structures and fields; these are not mandatory.
- Outputs:
  - `xxx_config_outputs` following naming rules.
  - Excel files; retain complete structured results such as JSON for complex nested data.
  - Table indexes, table/field descriptions, parsing-failure reports, and missing-content reports.
- Boundaries:
  - Do not modify raw unpacked materials or invent data, default values, or business rules.
  - Preserve original field names and values; put inferred Chinese meanings in documentation rather than replacing fields.
  - Distinguish missing fields, empty strings, zero, and null; do not interchange them.
  - Export IDs, long integers, and leading-zero identifiers with suitable types to avoid Excel conversion.
  - Do not execute extracted business scripts to load data; prefer static parsers.
  - This skill restores/exports data. Modifying another project's tables belongs to the table-mapping skill.
  - Keep field interpretations, inferred uses, and confidence in separate documentation or explanatory worksheets, outside original data rows. Export text as text to prevent Excel interpreting formulas, dates, or other types. Preserve original-to-exported table/field/name mappings when names change.
- Steps:
  1. Confirm source and reuse scope from info, context, versions, target tables, and existing results.
  2. Identify format and structure from actual CSV, JSON, Lua tables, databases, and binary tables. Prefer schemas, parsers, and same-version code for field information; mark unknowns rather than guessing.
  3. Restore complete data while preserving records, fields, array order, and nesting. Merge split tables only with structural evidence; record part sources, order, and duplicate-key conflicts.
  4. Export ordinary tables as rows/columns. Represent nesting with explicit child-table relationships or JSON cells, retaining complete structured results. Split data or provide another complete format beyond Excel limits; do not silently truncate.
  5. Record table purposes and field types, examples, meanings, and evidence. Cross-check same-version code where available; mark inferences and distinguish cross-version evidence.
  6. Index sources, output paths, record/field counts, and parsing state. Record artifacts/indexes in context and only newly confirmed APK information in info. Share the main task record.
- Format and field recovery details:
  - Alongside CSV/JSON, consider XML, Lua static tables or bytecode constants, Proto, SQLite, compressed containers, and AssetBundle configuration. Delegate containers to container while retaining data-structure parsing here.
  - For split/incremental tables, inspect parent tables, schemas, and parsers and inherit evidenced real field names. Label positional col_1-style fields as incompletely identified; do not present placeholders as originals.
  - Document types, examples, value ranges/enums, related code, and evidence. Check value domains and read logic before establishing table relationships; matching names alone are insufficient.
  - If fully parsed data cannot fit ordinary rows/columns, retain complete JSON, SQLite, or another suitable format and explain Excel limits. Separate successful structured parsing from successful Excel export; do not recount the former as parsing failure.
- Registered tools and format experience:
  - Use [resolve_table_vext](../../apkbreak_tools/games/sample_game_a/resolve_table_vext.md) for already-decoded index/data/vExt structures to restore fields by positions/reference flags. Raw Lua/binary decoding requires suitable tools.
  - JSON is the recovered-data reference. Do not fill absent fields with null. Companion CSV encodes cells as JSON to distinguish missing, null, empty strings, and nested values; do not let Excel infer types directly.
  - Export Excel from complete structured results, retaining long integers, leading zeros, and text. This tool does not replace target-project business-table mapping.
- Completion criteria:
  - Requested tables have been processed; successful results are fully exported and indexed.
  - Unknown fields, missing parts, parsing failures, and unchecked items are listed.
  - Separate data-recovery completeness from certainty about field semantics; unknown meanings do not indicate failed parsing.
- Exceptions:
  - Unsupported formats or missing schemas: retain originals and explain blockers; string scans are not complete tables.
  - Individual table failures: continue independent tables and mark the overall task partial.
  - Duplicate keys or split-table conflicts: retain conflict information without selecting or overwriting arbitrarily.
  - Follow shared rules for interruptions, duplicate names, and overwrites.
