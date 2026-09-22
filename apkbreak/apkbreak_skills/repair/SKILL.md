---
name: repair
description: "The user requests fixing color, transparency, texture, material, or effect-display problems in imported resources."
---

# Repair Effects, Materials, and Textures

Before execution, read the [naming rules](../../apkbreak_rules/naming.md), [layout rules](../../apkbreak_rules/layout.md), [execution rules](../../apkbreak_rules/execution.md), and [delivery rules](../../apkbreak_rules/delivery.md). The user's explicit instructions take precedence over default conventions.

When calling another skill, read its entry point through the [skill index](../../apkbreak_docs/skills.md). The main workflow and subflows share one task record. Use the [record templates](../../apkbreak_context/README.md), and confirm available tools through the [tool inventory](../../apkbreak_tools/README.md).

- Path base: the repository root is apkbreakdown; management directories are under apkbreak/, intermediates under apkbreak/apkbreak_unpacked/, and outputs remain at the repository root. Recorded paths are relative to the repository root.

- Use case: fix color, transparency, texture, material, or effect-display problems in imported resources.
- Required inputs:
  - Target Unity project directory and exact resource paths.
  - Symptoms and scope, such as washed-out colors, pink rendering, opaque transparent regions, excessive brightness, or texture misalignment.
  - Game/version, obtainable from info.
  - Original screenshots, video, source assets, or frame captures, if available. Without reference material, define the reliably confirmable repair scope.
  - MCP enabled and project open.
- Outputs:
  - Repaired Prefabs, materials, texture import settings, or Shaders.
  - Change inventories, repair evidence, original backups, and rollback instructions.
  - Reports of performed checks, remaining differences, and unvalidated items.
- Boundaries:
  - Do not modify original packages or raw unpacked materials.
  - Modify only specified resources and necessary dependencies; do not replace project-wide Shaders/materials independently.
  - Do not upgrade Unity, change pipelines/global color space, or install dependencies on your own.
  - Prefer recovering existing references/correct parameters. Where required within repair scope, compatibility Shaders may be written but must be labeled manual repairs, not original Shader recovery.
  - Do not hide unexplained problems with arbitrary particle colors, brightness, or texture changes.
  - Do not change unrelated particle counts, timing, positions, scales, or business logic.
  - By default validate references, Shader state, and visuals under delivery.md, with compilation/previews/Play Mode as needed. Follow explicit skips. Builds require task scope or existing authorization; identify unperformed checks.
- Steps:
  1. Read info/context to confirm resources, Unity version, and pipeline. Save pre-change files and available reference visuals; record conditions under which the issue occurs.
  2. Trace material slots, Shaders, textures, and parameters from Prefab Renderers/particle renderers. Inspect incorrect bindings, missing dependencies, and exporter placeholders, not filenames alone.
  3. As relevant, inspect RGB/Alpha, sampling channels, material/particle colors, blending, depth/sorting, texture import settings, and vertex data. Use existing RenderDoc captures when needed to confirm actual bindings/render state.
  4. Fix references/parameters first. Implement necessary compatibility only if original Shaders cannot be used, distinguishing direct-evidence repairs from approximations.
  5. Determine reference impact before editing shared materials, Shaders, textures, or settings. If others are affected, create target-specific copies and adapt references instead of changing other resources' behavior.
  6. Save necessary changes only, avoiding unrelated Prefab serialization. By default inspect persisted references/visuals, following independent-validation or skip instructions. Keep camera, background, lighting, and playback time consistent for comparisons.
  7. Record causes, evidence, before/after parameters, files, shared-dependency impact, and outstanding differences. Update context and affected outputs indexes/path records.
- Floating-point textures and format conversion:
  - For texture export errors, inspect actual TextureFormat, streamed-data location/length, and pixel dimensions. RGBAHalf and other floating-point formats cannot be decoded directly as 8-bit pixels.
  - Retain original HDR data. Per-image maximum normalization or 8-bit conversion changes brightness relationships and is not a default repair. Document losses, color space, and Alpha rules for necessary preview/target conversions.
  - Record payload recovery, preview conversion, and material color repair separately. Openable PNG files do not prove original appearance was recovered.
- Completion criteria:
  - Requested repairs are saved, traceable, and reversible.
  - Distinguish reference fixes, parameter fixes, and manual compatibility implementations.
  - Record saved files, Shader compilation, normal previews, and normal runtime behavior separately.
  - Without visual checks, state "Repairs saved; visual results await confirmation".
  - Without original reference evidence, do not promise full equivalence to the original game. Mark failures or missing required dependencies as partial.
- Exceptions:
  - Missing textures, materials, or required data: record missing sources; do not fabricate original resources.
  - Uncertain original appearance: state evidence gaps and possible approximations; ask if the goal remains unclear.
  - Repairs affect out-of-scope resources: isolate changes; if impossible, explain impact rather than expanding scope.
  - Suspected runtime code/dynamic parameters: trace supporting evidence instead of masking problems statically; establish a separate scope for code changes.
  - Follow apkbreak_rules for interruptions, duplicate names, and overwrites.
