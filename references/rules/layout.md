# Skill Package and Task Workspace Layout

Two locations serve different purposes. Never write task data into an installed skill.

## Installed skill: read-only instructions

```text
apk-reverse-engineering/
├─ SKILL.md
└─ references/
   ├─ workflows/
   ├─ rules/
   ├─ docs/
   ├─ tools/
   └─ templates/
      ├─ info/
      ├─ context/
      └─ tools/
```

SKILL.md is the only installed skill entry point. workflows/ contains its twelve internal procedures. rules/ holds shared conventions; docs/ holds usage and schema descriptions; tools/ holds usage notes, not executable implementations; templates/ holds empty templates.

Resolve documentation links from the containing file, independently of the current shell directory. Reference files stay inside this skill so copy and symlink installations remain usable.

Do not create task directories, local inventories, caches, packages, or exports under references/. Do not edit installed templates with real user data.

## User-selected task workspace: inputs and generated data

Use the directory selected by the user or infer the existing task workspace from supplied inputs/records. Do not choose a new destination when ambiguity affects existing files. Create only the directories required by the authorized task.

```text
task-workspace/
├─ apks/                       # Original packages and accompanying inputs
├─ unpacked/                   # Per-task extraction/conversion intermediates
├─ info/                       # APK information records
├─ context/                    # Per-task summaries, status, logs, and backups
├─ tools/                      # Separately obtained tools and local inventory
│  ├─ cache/
│  └─ registry.local.json
└─ sample_game_a_v1_art_outputs/ # Results and their indexes/reports
```

The directory name task-workspace is illustrative, not required. Existing user layouts take precedence: record explicit mappings instead of moving resources to fit these examples. If the user supplies an original package elsewhere, keep it there and record its path; an apks/ copy is optional and requires an appropriate task scope.

- **apks/**: original APK/XAPK/APKS packages and accompanying inputs, not processed results. Register sources, original filenames, sizes/hashes, and versions in info/.
- **unpacked/**: intermediate materials under game_id_project_version_resource_type_task_time_unpacked. Preserve raw materials separately from transformed candidates and retain layouts/external-data relationships required by tools.
- **info/**: one CSV per package, containing only basic APK information. Use the [APK schema](../docs/apk-info.md); artifact locations and task progress belong in context.
- **context/**: one directory per task, shared by all workflows. Holds summaries, stage state, logs, change records, and backups outside Unity Assets. Use [task records](../docs/task-records.md).
- **tools/**: local inventory and separately obtained tools. Cache portable distributions by tool/version/platform, preserve dependencies, and register existing installations without moving them. Use [tool conventions](../tools/README.md#tool-cache-and-reuse).
- **outputs directories**: organized art, code, or config at the workspace root or a user-specified target. Include indexes, provenance, failures, and known limitations. Preserve Unity/Rider project structures. Keep reports outside actual data rows and Unity Assets where practical.

## Paths, copying, and preservation

- Recorded workspace-relative paths and command input/output paths are relative to the task workspace. Document links are relative to the installed document.
- For inputs or target projects outside the workspace, record explicit absolute paths or clearly identified project-relative mappings.
- Tool executables are resolved from the actual environment/local inventory, never from documentation filenames. Command examples use <configured-tool-directory> placeholders.
- Preserve source resource names, .meta files, and GUID relationships as required by the relevant workflow.
- Apply [naming](naming.md) and [execution](execution.md) rules for versions, collisions, candidate writes, and overwrites.
- Do not automatically move, rename, clean, or delete existing resource projects, historical tasks, or backups.
- Preserve historical paths and append mappings if authorized moves occur. Keep durable indexes with outputs and task-specific facts in context.
- Keep caches, credentials, temporary environments, incomplete downloads, and actual task data outside the public skill repository.
