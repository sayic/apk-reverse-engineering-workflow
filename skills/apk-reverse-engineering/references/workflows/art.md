# Convert Unpacked Art Resources and Import into Unity

Before execution, read the [naming rules](../rules/naming.md), [layout rules](../rules/layout.md), [execution rules](../rules/execution.md), and [delivery rules](../rules/delivery.md). The user's explicit instructions take precedence over default conventions.

When calling another workflow, read its entry point through the [workflow index](../docs/skills.md). The main workflow and subflows share one task record. Use the [record templates](../docs/task-records.md), and confirm available tools through the [tool inventory](../tools/README.md).

- Path base: use the user-selected task workspace for inputs and generated artifacts. Resolve documentation links relative to this installed file; follow the shared layout rules. Never write into the installed skill.

- Use case: the user requests organizing extracted art resources and importing them into Unity.
- Required inputs: unpacked directory, target Unity project directory, scope, and a user-provided Unity project whose directory must itself be xxx_art_outputs.
- Outputs: resources in the target project, file indexes, and missing-item and failure reports.
- Boundaries: do not create Unity projects, download remote resources, write Shaders, or repair visuals on your own. Record the target Unity version and rendering pipeline. Report incompatibilities rather than upgrading the project, switching pipelines, installing dependencies, or changing global settings.
- Steps:
  - Read source, version, and historical records and confirm directories exist. Use an already-established scope directly; all resources or specified Prefabs/effects are supported.
  - Determine whether inputs are Bundles, serialized files, or already-exported Unity resources, and select the appropriate tools.
  - Preserve relationships among Prefabs, materials, textures, animations, and other assets. Extract required dependencies with specified objects; record omissions instead of presenting placeholders as successful recovery.
  - Establish the target import subdirectory and preserve .meta files and GUID references. Reuse files only when sources and contents match. For different contents at the same path or GUID conflicts, retain existing files, stage conflicting items, and report them; do not rewrite GUIDs or references without authorization. Continue non-conflicting resources.
  - Update context at task start, stage completion/failure, and finish, recording artifact/index locations. Reuse the main task record when called by another workflow.
- Export and media checks:
  - Record exporter version, detected Unity version, and export mode. Dummy/placeholder Shaders preserve only some names or references and do not prove recovery of original Passes, keywords, variants, or visuals. Keep original binary evidence in unpacked and mark placeholders in output indexes.
  - Record direct copying, extraction, decoding, and transcoding separately. Preserve source format, actual payload format, and final format; changing extensions is not conversion. Even identical hashes do not authorize deduplicating distinct logical resources; retain their mappings.
  - Inspect embedded audio payloads and external .resS/.resource data, including Wwise/FMOD Banks when required. Check actual decoding, channels, sample rates, and duration; do not label truncated audio or raw compressed payloads as playable files.
  - Check image dimensions/decodability, Mesh vertices/indices/material references, and video tracks/duration/decodability according to target type. File headers, declared lengths, required blocks, and actual payloads must agree; record empty files, truncation, and conversion failures separately.
  - Count exporter warnings, unsupported objects, missing dependencies, and final export failures separately. Follow container procedures to review object-read failures rather than repeating all container parsing.
- Merging and import checks:
  - For large imports, confirm layout/references on a small batch before proceeding in resumable batches. Check assembly conflicts if exports contain .cs, .asmdef, .dll, or response files. List isolated content; missing required dependencies prevents claiming successful import.
  - Build candidate merges for multilayer artifacts before applying shared conflict rules. Do not silently prefer newer layers. Check Windows long paths, invalid characters, and case collisions against source inventories; a successful copy command does not prove nothing was missed.
  - Determine completeness from actual files/references in the final target project, checking object counts, exported-file counts, additions/skips/conflicts, and failures. Explain that objects and files are not one-to-one.
  - Distinguish Unity built-in GUIDs, intentional exclusions, historical omissions, and actual missing references. Report Missing Script/custom-serialization issues separately from texture, Mesh, or material corruption. Validate target import, compilation, and Shader errors within task scope.
- Registered tools and special texture experience:
  - Call a compatible local AssetRipper through [run_assetripper_export](../tools/common/run_assetripper_export.md), explicitly providing the service address, settings, and tool version. generated_unverified indicates only candidate results.
  - Use [inspect_unity_assets](../tools/common/inspect_unity_assets.md) for supporting static checks and retain its unchecked items. It does not validate Mesh buffers, audio/video, or Unity runtime behavior.
  - If Texture2D image conversion fails, first inspect actual RGBAHalf, RGBA32, or Alpha8 formats, .resS paths, offset, size, and pixel counts. Library failure does not necessarily indicate missing pixels.
  - Converting HDR floating-point textures to 8-bit PNG can lose brightness above 1. Do not normalize by maximum value by default to "repair colors". Retain floating-point payloads, and document tone mapping, color space, Alpha handling, and flipping for previews; do not claim lossless recovery.
- Completion criteria:
  - Specified resources have been converted into the agreed Unity project location, with an index listing conversion failures, missing dependencies, and unchecked items.
  - Describe actual usability from performed checks; do not promise the original game's runtime appearance by default.
  - Mark partial completion if conversion failures, unresolved conflicts, or missing required dependencies remain.
