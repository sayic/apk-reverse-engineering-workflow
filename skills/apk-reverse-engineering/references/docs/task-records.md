# Task Records

Create actual task records from [summary.md](../templates/context/summary.md) and [status.json](../templates/context/status.json). Use [changes.csv](../templates/context/changes.csv) when formal files change. Empty templates are not executed tasks.

- Purpose: retain summaries, stage status, changes, and tool logs for each task so the team or AI can continue.
- Task types: organize according to naming.md, covering extraction, downloading, parsing, conversion, mapping, repair, import, query, and checks.
- Minimal directory structure:

```text
context/
└─ sample_game_a_v1_extract_20000101_120000/
   ├─ summary.md
   ├─ status.json
   ├─ changes.csv
   └─ logs/
```

- File responsibilities:
  - summary.md: goals, results, artifact locations, validation, issues, and next steps.
  - status.json: stage status, inputs/outputs, tool-log entry points, and continuation actions; use the template below.
  - changes.csv: additions/modifications/deletions to formal artifacts and targets, with before/after hashes; create only when corresponding changes occur, using the fields below.
  - logs/: actual tool logs; create only when logs exist.

#### summary.md: Task summary template

```markdown
# Task Summary

## 1. Task Information

- Task identifier:
- Task type:
- Game and project version:
- User request and processing scope:
- Input files or directories:
- Start time and time zone:
- End or pause time:
- Actual processing duration for this run:

## 2. Results

- Task status:
- Completed work:
- Unfinished work and reasons:
- Main processing methods, tools, and versions:
- New or updated common tools: none / tool path and purpose

## 3. Artifact Flow and Changes

| Purpose | Path | Contents |
|---|---|---|
| Input materials | | |
| unpacked | | |
| outputs or external targets | | |
| File indexes and detailed reports | | |
| Change inventories and backups | | |

- Counts of additions, modifications, and deletions:
- Unresolved file conflicts:

## 4. Validation Results

| Check | Status | Result and evidence location |
|---|---|---|
| | | |

- Unperformed checks and reasons:
- Known omissions, limitations, and inferences:

## 5. Continuation

- Directly reusable results:
- Partial or unusable content:
- Current blockers and prerequisites:
- Next action: none / stage to resume and inputs to use
```

- Usage:
  - Write "none" for absent items, "not applicable" where irrelevant, and "unknown" for undetermined facts.
  - Validation status: passed, failed, not performed, or not applicable.
  - Directories may occupy multiple rows, such as one extraction producing art, code, and config.
  - Actual processing duration excludes waiting or paused time; use unknown if it cannot be calculated accurately.
  - Update the current summary on continuation while preserving historical logs; do not erase failures or reasoning.
- Simplified extraction records:
  - The user supplies an APK and art, code, config, or all; do not additionally require priority modules such as combat, city building, or number gates.
  - Extract all package-contained resources of the selected types by default; reuse applicable results without requiring repeat processing.
  - Do not produce special reports for priority resources; retain a unified file index, failure list, and acceptance results.
  - Record selected resource types directly as summary scope; do not add a special priority-resource report section.
  - Use the parsed-resource query/analysis workflow for later specific lookups. Repairs, mappings, and imports into other projects still require explicit targets.

#### status.json: Stage status and continuation

- Purpose: record progress and continuation points without repeating the task summary.
- Minimal template:

```json
{
  "task_id": "sample_game_a_v1_extract_20000101_120000",
  "status": "paused",
  "updated_at": "2000-01-01T12:00:00+08:00",
  "stages": [
    {
      "id": "extract",
      "status": "completed",
      "input_paths": [
        "apks/sample_game_a_v1.xapk"
      ],
      "output_paths": [
        "unpacked/sample_game_a_v1_code_20000101_120000_unpacked"
      ],
      "log_paths": [],
      "issue": "",
      "next_action": ""
    },
    {
      "id": "convert_code",
      "status": "paused",
      "input_paths": [
        "unpacked/sample_game_a_v1_code_20000101_120000_unpacked"
      ],
      "output_paths": [
        "sample_game_a_v1_code_outputs"
      ],
      "log_paths": [
        "context/sample_game_a_v1_extract_20000101_120000/logs/convert_code_ilspy_20000101_120500.log"
      ],
      "issue": "Conversion interrupted; partial outputs exist",
      "next_action": "Inspect generated files and continue unprocessed assemblies"
    }
  ]
}
```

- Rules:
  1. Record major stages, not individual commands; use only stages that actually exist.
  2. Tasks and stages share status values: pending (not started), running, completed, partial, failed, blocked, paused, and cancelled. Use skipped for user-requested skips and explain in issue.
  3. Update at stage start/completion/failure/pause. The entry skill and workflows share this file.
  4. completed is only a continuation clue. Reassess reuse under execution rules before rerunning; reference tool inventories for file-level progress rather than listing every file here.
  5. Record validation as a separate stage. log_paths points to tool logs; use an empty array if none exist.
- Example paths and progress demonstrate the format only, not actual task results.

#### changes.csv: Actual file changes

- Purpose: record changes for handover, rollback, and detecting edits since prior generation.

| Column | Meaning |
| --- | --- |
| action | add, modify, or delete |
| path | Path of the changed file |
| before_sha256 | Pre-change hash; blank for additions |
| after_sha256 | Post-change hash; blank for deletions |
| backup_path | Pre-change backup location; blank when absent |
| reason | Reason, such as re-exporting or repairing material references |

Example (hashes and backup paths are placeholders):

```csv
action,path,before_sha256,after_sha256,backup_path,reason
add,sample_game_a_v1_code_outputs/example.cs,,<new_hash>,,code recovery
modify,sample_game_a_v1_art_outputs/Assets/a.mat,<old_hash>,<new_hash>,<backup_path>,repair material references
```

- Rules:
  1. One row per actually changed file. Omit identical or skipped files.
  2. Record formal artifacts and target files, excluding temporaries, caches, tool logs, task records, and changes.csv itself.
  3. Register only after successful writes. Planned changes and failed attempts go in context, not as completed changes.
  4. For repeated edits to one file within a task, retain initial/final states; keep intermediate details in logs.
  5. Do not generate this file for read-only tasks. Record renames as deletion at the old path and addition at the new path, with their relationship noted.

#### logs/: Raw tool output

- Purpose: preserve execution evidence for diagnosing failures without another complex report template.
- Rules:
  1. Create the directory only when tool logs exist; no empty directories for queries/discussions without logs.
  2. Save each execution separately as stage_tool_start_time.log. Retries create new logs without overwriting prior ones.
  3. Prefer native tool formats, retaining JSON/CSV extensions rather than forcing .log. Record versions, parameters, start/end times, and exit codes; callers supplement missing metadata.
  4. Keep key results/failures in summary.md, not whole logs. Point the relevant status.json stage's log_paths to logs.
  5. Logs alone do not establish success. Even Done or exit code zero still requires delivery-standard validation. Redact passwords, access tokens, and other sensitive log data.

Example:

```text
logs/
├─ convert_code_ilspy_20000101_120500.log
└─ convert_code_ilspy_20000101_121000.log
```

#### Query task records

- The query workflow mainly writes summary.md in the current query task directory, recording the question, versions, conclusions, evidence paths, and unresolved items.
- Put conclusions under Results and evidence checks under Validation Results.
- Record query progress, completion, or blocking in status.json.
- Do not generate changes.csv: queries do not modify resources, and writing task records does not count as file changes.
- Create logs/ only when execution logs need retention.

Example:

```text
context/
└─ sample_game_a_v1_query_20000101_120000/
   ├─ summary.md
   └─ status.json
```
