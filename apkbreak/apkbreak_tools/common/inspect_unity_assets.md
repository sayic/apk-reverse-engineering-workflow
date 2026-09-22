# Unity Resource Static Checks

Check empty files, meta files, duplicate GUIDs, text-resource references, Shader declarations, and PNG decoding, retaining unchecked items.

- Tool version: 1.0; runtime: Python 3.10+.
- Input: an explicit Assets subdirectory; optionally, a JSON array of confirmed external GUIDs.
- Output: JSON results with uncovered content, declared Mesh counts, placeholder Shaders, and unresolved references listed separately.
- Exit codes: 0 means completion within this tool's scope; 2 means failed checks, partial completion, or invalid parameters; other nonzero codes indicate exceptions. Retain existing results after exceptions without treating them as complete success.
- Before use: replace example paths with the actual task paths. Write outputs to candidate directories and use new report paths for the current execution. Run commands from the repository root.

```powershell
python apkbreak/apkbreak_tools/common/inspect_unity_assets.py --assets "apkbreak/apkbreak_unpacked/task_directory/Assets" --report "apkbreak/apkbreak_context/task_directory/logs/assets_run1.json"
```

PNG checks require Pillow. If absent, record the check as not performed and return 2 without automatic installation. Other parts use only the standard library. --known-external-guids registers evidenced external GUIDs; do not mark unknown references as intentional exclusions by default. Only a small explicit set of Unity built-in GUIDs is recognized; others await confirmation.

Mesh checks count text declarations only, not geometry buffers. Binary serialization, audio/video, and other formats are outside this tool's decoding scope and must be marked unchecked. Placeholder Shaders or incomplete checks do not receive a full pass. Reports do not substitute for Unity compilation/import, Shader compilation, or visual acceptance.

See the [tool overview](../README.md) for versions and validation scope. Actual tasks record input hashes, full parameters, logs, exit codes, and artifact locations. This tool does not write the main task's summary/status.
