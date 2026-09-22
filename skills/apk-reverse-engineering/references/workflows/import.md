# Import Parsed Art Resources into Another Project

Before execution, read the [naming rules](../rules/naming.md), [layout rules](../rules/layout.md), [execution rules](../rules/execution.md), and [delivery rules](../rules/delivery.md). The user's explicit instructions take precedence over default conventions.

When calling another workflow, read its entry point through the [workflow index](../docs/skills.md). The main workflow and subflows share one task record. Use the [record templates](../docs/task-records.md), and confirm available tools through the [tool inventory](../tools/README.md).

- Path base: use the user-selected task workspace for inputs and generated artifacts. Resolve documentation links relative to this installed file; follow the shared layout rules. Never write into the installed skill.

- Use case: copy specified resources from art outputs and their required dependencies into another Unity project.
- Required inputs:
  - Source art outputs directory and specific resources.
  - Target Unity project directory and import location.
  - The target project is open, Unity MCP is connected, and the connection is confirmed to point to the target.
  - Scope, such as a Prefab, effect, or resource group.
- Outputs:
  - Resources and required dependencies in the target project.
  - Source-to-target path mappings and file inventories.
  - Reports of missing dependencies, conflicts, and import status.
- Boundaries:
  - Copy by default; do not delete or modify source outputs.
  - Do not independently change resource colors, Shader behavior, particle parameters, animations, position, or scale.
  - Do not repair visuals, install dependencies, or change global target settings on your own.
  - Place all copied resources in the user's specified location, not scattered elsewhere in the target project.
  - Do not add business integration, scene mounting, or Addressables configuration unless explicitly requested.
  - Apply necessary path/reference adaptations only to target copies, recording them without changing presentation parameters.
  - By default, validate references, Shader state, and visuals under delivery.md, using compilation, previews, or Play Mode as appropriate. Follow explicit skips. Builds require task scope or existing authorization; list unperformed checks.
- Steps:
  1. Read info, indexes, and historical records to confirm source version, target project, and import directory. Confirm MCP's actual connected project before Unity operations.
  2. Follow real references to materials, textures, Shaders, animations, nested Prefabs, and other dependencies to create a copy list. Record omissions separately; do not copy only the main Prefab.
  3. For one resource, create a same-named directory under the user's location by default and group its dependencies there. For multiple resources, use the task's import directory, retaining necessary internal subdirectories without moving assets to external shared folders.
  4. Avoid overwriting same-named dependencies and preserve required relative layouts. Determine source provenance before overwriting target file/GUID conflicts.
  5. Copy resources and .meta files, preserving GUIDs where conflict-free. If new GUIDs are necessary, update related references together within target copies and record mappings. Keep unresolved references blocked.
  6. Confirm import through the target Unity project and check references/visuals by default. Follow and record independent-validation or skip instructions. Report preexisting source omissions separately from new import issues.
  7. Record source/target paths, copy lists, adaptations, conflicts, checks, and rollback locations. Reuse the main task record when called by another workflow.
- Batch copying and final-project checks:
  - Identify .cs, .asmdef, .dll, response files, and assembly conflicts before copying; do not incidentally copy all exported code. Record intentional exclusions separately and mark affected resources incomplete without mixing them into source-extraction omissions.
  - For large imports, establish layout/dependencies with a small batch, then copy in resumable batches with per-batch inventories and .meta mappings.
  - Check Windows long paths, case folding, invalid characters, and same-named dependencies against source lists/path mappings. Interrupted partial directories do not establish complete import.
  - Deliver actual file copies by default rather than hard links or Junctions, preventing subsequent changes from affecting sources. Explain physical sources and boundaries for existing mounts.
  - Inspect files, GUIDs, and actual references at final target paths, not just source projects or temporary copies. Distinguish Unity built-ins, intentional exclusions, preexisting omissions, and new omissions.
  - Report Missing Script/custom serialization separately from texture, Mesh, or material damage. Importable placeholder Shaders do not prove original visuals; visual repairs remain a separate task scope.
- Registered tools:
  - Use [verify_meta_guids](../tools/common/verify_meta_guids.md) when source/target relative layouts match. For intentional path/GUID changes, check actual mappings rather than assuming one-to-one paths.
  - Use [inspect_unity_assets](../tools/common/inspect_unity_assets.md) to support target static-reference checks. Unknown references remain unresolved, not intentionally excluded just because source directories were not copied. Add only confirmed external dependencies to known GUID lists.
  - These tools inspect only specified directories and do not replace target Unity import/visual validation; follow the user's current validation instructions.
- Completion criteria:
  - Specified resources and acquired required dependencies have been copied to the agreed location.
  - Source outputs remain unchanged and target provenance/adaptations are traceable.
  - Missing dependencies, conflicts, and unchecked items are listed.
  - Record copying, Unity import, visuals, and business integration separately.
  - Mark missing required dependencies or unresolved conflicts as partial completion.
- Exceptions:
  - Target project closed or MCP connected elsewhere: pause target operations and identify the required project.
  - Existing target files: reuse matching sources/contents; retain/report different or manually changed content without overwriting directly.
  - GUID conflicts or ambiguous same-named Shaders: record impact and pause affected resources if reliable isolation is impossible; do not change shared implementations on your own.
  - Required scripts, plugins, or dynamically loaded resources: list exact dependencies rather than copying all game code/plugins.
  - Abnormal imported visuals: record issues and route requested fixes to the repair workflow; do not repair during copying on your own.
  - Follow shared rules for interruptions, duplicate names, and overwrites.

Default layout:

```text
user_specified_directory/
└─ a/
   ├─ a.prefab
   ├─ xxx.mat
   ├─ xxx.png
   └─ aaa.shader
```
