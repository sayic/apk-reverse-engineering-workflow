# Positional Field and vExt Restoration

Expand data tables using positions and reference flags in index, preserving absent fields, null, empty strings, zero, and nested values.

- Documented interface version: 1.0; expected runtime: Python 3.10+. The implementation is not bundled.
- Input: already-decoded JSON containing index, data, and optional vExt.
- Outputs: per-table .resolved.json and .resolved.csv files, plus a processing report.
- Exit codes: 0 means completion within this tool's scope; 2 means failed checks, partial completion, or invalid parameters; other nonzero codes indicate exceptions. Retain existing results after exceptions without treating them as complete success.
- Before use: replace example paths with the actual task paths. Write outputs to candidate directories and use new report paths for the current execution. Run commands from the user-selected task workspace root. The script location below is a placeholder for a separately obtained, compatible implementation.

```powershell
python "<configured-tool-directory>/games/sample_game_a/resolve_table_vext.py" "unpacked/task_directory/table.json" --output "unpacked/task_directory/table_candidates" --report "context/task_directory/logs/table_run1.json"
```

JSON is the complete data reference. CSV uses JSON encoding for every present cell: blank cells mean missing fields, null means null, and a double-quoted empty string means an original empty string. CSV is intended for programmatic reading, not direct Excel type inference; the config workflow exports Excel using text-type protection. Check vExt bounds and field-position conflicts without executing original Lua. This tool neither parses raw bytecode nor maps business tables to target projects.

See the [tool overview](../../README.md) for versions and validation scope. Actual tasks record input hashes, full parameters, logs, exit codes, and artifact locations. This tool does not write the main task's summary/status.
