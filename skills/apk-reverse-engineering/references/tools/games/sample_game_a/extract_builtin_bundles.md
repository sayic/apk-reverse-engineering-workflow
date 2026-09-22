# Sample Game A Built-in Bundle Extraction

Use matching offset tables and gameres to extract built-in UnityFS entries from aggregate files.

- Documented interface version: 1.0; expected runtime: Python 3.10+. The implementation is not bundled.
- Inputs: same-version BundleOffsetTable.bytes, gameres, and data files named by the table.
- Outputs: Bundle files and a JSON report with offsets, CRC results, hashes, and uncovered-byte counts.
- Exit codes: 0 means completion within this tool's scope; 2 means failed checks, partial completion, or invalid parameters; other nonzero codes indicate exceptions. Retain existing results after exceptions without treating them as complete success.
- Before use: replace example paths with the actual task paths. Write outputs to candidate directories and use new report paths for the current execution. Run commands from the user-selected task workspace root. The script location below is a placeholder for a separately obtained, compatible implementation.

```powershell
python "<configured-tool-directory>/games/sample_game_a/extract_builtin_bundles.py" --asset-dir "unpacked/task_directory/AssetBundles" --output "unpacked/task_directory/bundle_candidates" --report "context/task_directory/logs/bundles_run1.json"
```

Check duplicate entries, bounds, overlap, headers, and CRCs. Return 2 for unexplained aggregate-file bytes. Mark entries present in the manifest but absent from the offset table as not_in_offset_table; this does not establish remote availability. This is not an object-level parser; the container workflow must still scan actual objects and dependencies.

See the [tool overview](../../README.md) for versions and validation scope. Actual tasks record input hashes, full parameters, logs, exit codes, and artifact locations. This tool does not write the main task's summary/status.
