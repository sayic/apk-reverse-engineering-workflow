# Parse Resource Containers

Before execution, read the [naming rules](../rules/naming.md), [layout rules](../rules/layout.md), [execution rules](../rules/execution.md), and [delivery rules](../rules/delivery.md). The user's explicit instructions take precedence over default conventions.

When calling another workflow, read its entry point through the [workflow index](../docs/skills.md). The main workflow and subflows share one task record. Use the [record templates](../docs/task-records.md), and confirm available tools through the [tool inventory](../tools/README.md).

- Path base: use the user-selected task workspace for inputs and generated artifacts. Resolve documentation links relative to this installed file; follow the shared layout rules. Never write into the installed skill.

- Use case: parse objects and dependencies inside Bundles in extracted/downloaded files for subsequent code, art, or data-table conversion.
- Required inputs:
  - Input files or unpacked directory.
  - Game and project version, from info or the upstream task.
  - Scope: all containers or specified resources.
  - Relevant manifests, offset tables, part information, or format documentation, if available.
- Outputs:
  - Parsed files or object data within the current task's unpacked directory.
  - Container/object indexes and dependency mappings.
  - Failure, missing-content, and unsupported-content reports.
- Boundaries:
  - Do not alter original containers; decode, reassemble, and repair copies or independent outputs.
  - Do not download remote resources, import into Unity, or generate final code projects or Excel files.
  - Identify formats from actual content and evidence; not every container is a Unity AssetBundle.
  - Successful splitting, recognized headers, or manifest entries do not establish complete parsing.
  - Do not execute contained business scripts or fabricate missing objects/dependencies.
- Steps:
  1. Read info and context to confirm version, scope, and reusable artifacts. The caller first checks existing container records for the same task, reusing applicable results and calling this workflow only for unparsed, failed, or invalidated items. Decide reuse under execution.md, not merely from entering another workflow.
  2. Inspect formats, compression/encryption, and relationships among name tables, offset tables, parts, and external data files; select suitable tools.
  3. Restore entries using evidenced offsets, lengths, and part order; record source positions. Mark uncertain boundaries for confirmation instead of guessing.
  4. Read actual object types, identifiers, and references, building logical-path-to-container-to-object indexes. Include dependencies required by specified resources and list omissions separately.
  5. Preserve layouts and external data needed by subsequent tools. Distinguish same-named objects with stable identifiers to prevent overwrites.
  6. Record container recognition, splitting, object parsing, and dependency completeness separately. Perform scope-appropriate validation by default, respecting explicit skips and listing unchecked items.
  7. Record tool versions, parameters, inputs/outputs, and index locations. Share the main/conversion task record and return control to the caller for subsequent stages.
- Custom containers and parts:
  - For a large data file plus name/offset table, identify string-length encoding, endianness, offset width, and record counts; do not assume a particular game's fixed format.
  - Determine boundaries using explicit lengths or evidenced adjacent offsets. Check offset ordering, bounds, overlap, record counts, and trailing data; document format-permitted shared regions separately.
  - Confirm numbering and assembly rules for *_splitN-style parts before rebuilding copies/independent outputs. Splitting success still requires checking actual headers and objects.
- Large manifests and object scans:
  - Build logical-path/object-ID-to-container, container-to-dependency, and alias-to-physical-file indexes once, avoiding full Bundle traversal per path. Estimate cost using small samples when scale is unknown.
  - Identify AudioClip, Mesh, Texture2D, VideoClip, and other types using actual object types, Class IDs, or serialization types. For "all", scan all in-scope physical containers or supply reviewable coverage evidence; filenames containing audio/model are insufficient.
  - Recursively collect dependencies from target containers and record target/dependency counts, missing IDs, cycles, and errors. Avoid revisiting dependency cycles. Complete manifest dependencies do not prove every object exported.
  - Preserve logical-path-to-container-to-object-ID-to-dependency-to-output mappings, including external .resS/.resource streams, standalone files, and audio Banks. A present main container does not establish complete payloads.
  - MonoBehaviour type-tree failures may reflect missing type information, tool versions, or cross-file references. Record container, PathID, type, and error separately; review MonoScript/Prefab references or another parser instead of automatically counting missing entities.
  - Save indexes and status separately for manifest parsing, entity checks, object scans, and dependency collection. After caller timeouts, inspect processes and completion markers before restarting. Incomplete scans do not establish full coverage.
- Registered tools and format experience:
  - For Sample Game A combinations of BundleOffsetTable.bytes, gameres, and aggregate data files, use [extract_builtin_bundles](../tools/games/sample_game_a/extract_builtin_bundles.md). Passing header/CRC checks does not mean object parsing is complete.
- Completion criteria:
  - Requested containers have been processed and successful results/indexes saved.
  - Objects can be traced to their source container (bundle) or part (resource_part0).
  - Unparseable content, missing entities/dependencies, and unchecked items are listed.
  - If only splitting succeeded and contained objects remain unreadable, mark partial completion rather than claiming usable recovered resources.
- Exceptions:
  - Unknown format/encryption: record evidence and blockers, retaining inputs.
  - Missing parts, offset tables, or external data: pause affected items and continue independent containers.
  - Individual object failure: retain successes and record object IDs/errors; do not discard all valid container results.
  - Incompatible tools: record applicability; do not force parsing by changing original files.
  - Follow shared rules for interruptions, duplicate names, and overwrites.
