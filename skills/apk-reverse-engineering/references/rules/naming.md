# Naming Conventions

- Scope:
  - Applies to new task-workspace directories, tools, logs, reports, and artifact roots. Installed skill package names and existing user layouts are not renamed by these rules.
  - Preserve original resource names, code identifiers, table fields, and fixed names required by Unity, Rider, and other tools; do not force renaming.
  - Use lowercase English letters, numbers, and underscores in filenames; retain file extensions.
  - Chinese text is allowed in document bodies.
- Game identifiers:
  - Use one consistent lowercase English identifier per game, such as sample_game_a or sample_game_b.
  - Record the mapping between game identifiers, product names, and application package names in info.
  - Do not alternate between abbreviations for the same game's directory prefix.
- Version identifiers:
  - Use the project version consistently in the form v followed by the version value, such as v1.
  - Record the source of the project version in info, retaining the installation-package and resource-manifest versions.
  - Ask the user if the project version cannot be identified; do not guess.
  - Register different inputs for the same project version separately; do not overwrite information merely because versions match.
- Resource types:
  - art: models, textures, materials, Shaders, Prefabs, animations, effects, and accompanying audio/video.
  - code: assemblies, scripts, decompiled code, and related code materials.
  - config: data tables and business configuration.
  - A task may process multiple types. Organize artifacts by type; required dependencies can be shared across types without forcing mutual exclusivity.
- Time and task identifiers:
  - Use yyyymmdd_hhmmss, such as 20000101_120000; record the time zone in task information.
  - Use the task start time; keep the original task identifier when resuming after an interruption.
  - Append a sequence number for naming collisions within the same second; do not overwrite existing tasks.
- File and directory names:
  - Packages: apks/game_id_project_version.original_extension.
  - Intermediates: unpacked/game_id_project_version_resource_type_task_time_unpacked.
  - Parsed artifacts: game_id_project_version_resource_type_outputs at the task workspace root.
  - Task records: context/game_id_project_version_task_type_task_time.
  - Common tools and scripts: descriptive lowercase English names, such as `download_resources.py`.
  - Reports and indexes: consistent template names, such as `summary.md`, `files.csv`, and `status.json`; do not repeat the full task name.
- Task types:
  - extract: installation-package extraction.
  - download: remote downloads.
  - parse: independent container parsing.
  - convert: code, art, or data-table conversion.
  - map: data-table mapping.
  - repair: resource repairs.
  - import: resource imports.
  - query: queries and analysis.
  - check: convention checks.
  - Subflows reuse the main task directory; do not create separate tasks merely because another workflow is called.
- Conflicts and exceptions:
  - Record the original filename when renaming or archiving a package; do not change its contents.
  - For packages with the same name but different contents, append the first 8 SHA-256 characters; extend the suffix if collisions remain.
  - For outputs with the same name, apply continuation and conflict rules to determine whether merging is allowed; do not automatically overwrite or mix different sources.
  - When converting invalid names, record original-to-new mappings. Do not use naming rules to bulk-rewrite internal resource references.

Example:

```text
apks/sample_game_a_v1.xapk
unpacked/sample_game_a_v1_art_20000101_120000_unpacked/
sample_game_a_v1_art_outputs/
sample_game_a_v1_code_outputs/
sample_game_a_v1_config_outputs/

context/
└─ sample_game_a_v1_extract_20000101_120000/
   ├─ summary.md
   ├─ status.json
   └─ changes.csv
```
