# Sample Game A Bundle Downloads

Filter resources and recursive dependencies from a fixed gameres manifest and validate cached/downloaded content.

- Documented interface version: 1.0; expected runtime: Python 3.10+. The implementation is not bundled.
- Inputs: manifest and explicit package name/platform/CDN; optional cache directory and resource filters.
- Outputs: manifest snapshot, download_plan.tsv, and download_summary.json; execution also generates per-file download_events.jsonl. Downloaded entities go in the output directory; reused cache entries retain their original paths.
- Exit codes: 0 means completion within this tool's scope; 2 means failed checks, partial completion, or invalid parameters; other nonzero codes indicate exceptions. Retain existing results after exceptions without treating them as complete success.
- Before use: replace example paths with the actual task paths. Write outputs to candidate directories and use new report paths for the current execution. Run commands from the user-selected task workspace root. The script location below is a placeholder for a separately obtained, compatible implementation.

```powershell
python "<configured-tool-directory>/games/sample_game_a/download_bundles.py" --manifest "unpacked/task_directory/gameres" --output-dir "unpacked/task_directory/download_candidates" --report-dir "context/task_directory/logs/download_run1" --package-name "your.package" --platform "Android" --cdn "<resource_service_address>" --all-missing
```

By default, generate a plan without network requests. If the user authorized downloads, add --execute to the actual command and use a new report-dir. Repeat --asset-pattern / --bundle-pattern as needed. --all-missing selects the entire manifest and skips valid cached content. --preset models-vfx is only a name-based prefilter, not proof of object coverage. Dependencies are recursive by default; document scope limits when using --no-dependencies.

CDN, platform, and package name are required; do not reuse historical addresses. --workers, --timeout, and --attempts-per-cdn bound concurrency/retries. --max-files and --max-bytes limit downloads and return 2 when items remain unprocessed. Mark existing different-content targets CONFLICT without overwriting. Reuse validated entities on reruns. HTTP Range resume is not supported; interrupted files are downloaded again, and leftover .download_*.part files are not valid results. Avoid credentials in full URLs written to logs.

See the [tool overview](../../README.md) for versions and validation scope. Actual tasks record input hashes, full parameters, logs, exit codes, and artifact locations. This tool does not write the main task's summary/status.
