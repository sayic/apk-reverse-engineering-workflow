# Sample Game A MDL Assembly Header Restoration

Restore the DOS prefix of specific .mdl files into new DLL copies.

- Documented interface version: 1.0; expected runtime: Python 3.10+. The implementation is not bundled.
- Input: a single-level directory containing *.mdl files.
- Outputs: DLL copies, per-file success/failure records, and hashes.
- Exit codes: 0 means completion within this tool's scope; 2 means failed checks, partial completion, or invalid parameters; other nonzero codes indicate exceptions. Retain existing results after exceptions without treating them as complete success.
- Before use: replace example paths with the actual task paths. Write outputs to candidate directories and use new report paths for the current execution. Run commands from the user-selected task workspace root. The script location below is a placeholder for a separately obtained, compatible implementation.

```powershell
python "<configured-tool-directory>/games/sample_game_a/restore_mdl_assemblies.py" "unpacked/task_directory/mdl" --output "unpacked/task_directory/dll_candidates" --report "context/task_directory/logs/mdl_run1.json"
```

Check PE signatures, header bounds, and CLR directory declarations before writing. These checks are not complete CLR metadata parsing; independently inspect DLLs with ILSpy or a metadata reader. The method applies only to confirmed formats with altered DOS prefixes alone; it does not automatically recover method logic.

See the [tool overview](../../README.md) for versions and validation scope. Actual tasks record input hashes, full parameters, logs, exit codes, and artifact locations. This tool does not write the main task's summary/status.
