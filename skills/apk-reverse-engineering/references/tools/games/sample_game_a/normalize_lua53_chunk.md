# Sample Game A Lua 5.3 Header Conversion

Correct observed format-1 Lua headers in copies for subsequent reading by compatible Lua tools.

- Documented interface version: 1.0; expected runtime: Python 3.10+. The implementation is not bundled.
- Input: Lua chunks with the specified signature, version, and field sizes.
- Output: converted copies; stdout reports added/reused.
- Exit codes: 0 means completion within this tool's scope; 2 means failed checks, partial completion, or invalid parameters; other nonzero codes indicate exceptions. Retain existing results after exceptions without treating them as complete success.
- Before use: replace example paths with the actual task paths. Write outputs to candidate directories and use new report paths for the current execution. Run commands from the user-selected task workspace root. The script location below is a placeholder for a separately obtained, compatible implementation.

```powershell
python "<configured-tool-directory>/games/sample_game_a/normalize_lua53_chunk.py" "unpacked/task_directory/source.luac" "unpacked/task_directory/normalized/source.luac"
```

Handles only Lua 5.3 format=1 with field sizes 4/4/8/8; it is not a general Lua decryptor. Rejects identical input/output paths and target conflicts; does not execute bytecode. Compatible LuaDec/luac checks are still required afterward. Retain instruction-level results if decompilation fails.

See the [tool overview](../../README.md) for versions and validation scope. Actual tasks record input hashes, full parameters, logs, exit codes, and artifact locations. This tool does not write the main task's summary/status.
