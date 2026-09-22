# Unity GUID Comparison

Compare .meta GUIDs at matching source/target relative paths, checking duplicate GUIDs and orphaned meta files.

- Tool version: 1.0; runtime: Python 3.10+.
- Inputs: explicitly specified source and target resource directories, both existing.
- Output: JSON listing missing/different GUIDs, duplicate GUIDs, and orphaned or unreadable meta files.
- Exit codes: 0 means completion within this tool's scope; 2 means failed checks, partial completion, or invalid parameters; other nonzero codes indicate exceptions. Retain existing results after exceptions without treating them as complete success.
- Before use: replace example paths with the actual task paths. Write outputs to candidate directories and use new report paths for the current execution. Run commands from the repository root.

```powershell
python apkbreak/apkbreak_tools/common/verify_meta_guids.py "apkbreak/apkbreak_unpacked/task_directory/source_assets" "apkbreak/apkbreak_unpacked/task_directory/target_assets" --report "apkbreak/apkbreak_context/task_directory/logs/guid_run1.json"
```

Applies only to copies preserving relative layouts. For path mappings or intentional GUID changes, compare actual mappings separately. Empty scopes do not pass; directory meta files are recognized correctly. This tool does not establish dependency completeness, Unity importability, or visual correctness.

See the [tool overview](../README.md) for versions and validation scope. Actual tasks record input hashes, full parameters, logs, exit codes, and artifact locations. This tool does not write the main task's summary/status.
