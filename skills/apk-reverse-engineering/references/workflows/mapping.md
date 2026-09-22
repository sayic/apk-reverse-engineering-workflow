# Map Data Tables and Modify Target Tables

Before execution, read the [naming rules](../rules/naming.md), [layout rules](../rules/layout.md), [execution rules](../rules/execution.md), and [delivery rules](../rules/delivery.md). The user's explicit instructions take precedence over default conventions.

When calling another workflow, read its entry point through the [workflow index](../docs/skills.md). The main workflow and subflows share one task record. Use the [record templates](../docs/task-records.md), and confirm available tools through the [tool inventory](../tools/README.md).

- Path base: use the user-selected task workspace for inputs and generated artifacts. Resolve documentation links relative to this installed file; follow the shared layout rules. Never write into the installed skill.

- Use case: convert recovered tables according to target-project fields/business rules and write target tables.
- Required inputs:
  - Source config outputs directory, game, and version.
  - Target table paths and worksheets to modify.
  - Mapping scope, such as specified tables, levels, record IDs, or all records.
  - Existing mapping configurations/tools, if any; establish missing mappings from code, schemas, and user requirements.
- Outputs:
  - Reusable mapping configuration.
  - Modified target tables.
  - Change inventories listing files, worksheets, record IDs, fields, and before/after values.
  - Reports of unmapped fields, conflicts, skipped records, and outcomes.
- Boundaries:
  - Do not modify source config outputs.
  - Modify only specified target tables and scope; do not change business code, generate runtime tables, or commit SVN on your own.
  - Similar field names do not establish identical meanings; establish units, enums, IDs, and references.
  - Do not invent unsupported defaults or allocate potentially conflicting IDs.
  - Preserve existing structure, formulas, formatting, and unrelated data; do not add, delete, or rename fields without authorization.
  - For missing critical mappings, first complete the determinable mapping plan, then consolidate questions.
- Steps:
  1. Read info, context, source/target tables to confirm versions, worksheets, structures, primary keys, and change scope.
  2. Record source/target fields, type conversions, units, enums, ID correspondences, filters, and supported defaults, including evidence and undetermined items.
  3. Distinguish additions, updates, and deletions. Define primary/composite matching keys; repeated execution must not add duplicate records. Deletions require explicit user scope.
  4. List expected additions, updates, deletions, skips, and conflicts. Check target ID occupancy, related records, and required-field sources. Do not write affected records while critical conflicts remain.
  5. Save pre-change copies and generate candidate results with mapping tools. Do not force writes to open files or files modified externally during processing.
  6. Perform appropriate mapping checks by default, write candidates to targets, and validate saved results. Follow explicit skips or independent validation. Record actual files, worksheets, IDs, row counts, fields, and unfinished items.
  7. Record mappings, tool versions, sources, targets, backups, and outcomes. Share the task record when called by another workflow.
- Completion criteria:
  - Requested mappings are applied and targets saved.
  - Mapping configuration and actual change inventories are retained for tracing/reuse.
  - Unmapped fields, conflicts, skips, and failures are explicit.
  - Record file writes, data checks, runtime table generation, and game runtime validation separately, identifying unperformed stages.
  - Mark required missing records/fields as partial completion.
- Exceptions:
  - Unclear semantics/conversions: retain as awaiting confirmation; do not guess values.
  - Duplicate keys or ID conflicts: list them without overwriting or renumbering on your own.
  - Missing related records: pause affected records/dependencies; do not create broken links.
  - Tool failures/incomplete candidates: retain original tables and record failed stages.
  - Open/externally changed targets: retain candidates and report, rereading before deciding how to merge.
  - Follow shared rules for interruptions, duplicate names, and overwrites.
