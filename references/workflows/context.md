# Create and Update Context Records

Before execution, read the [naming rules](../rules/naming.md), [layout rules](../rules/layout.md), [execution rules](../rules/execution.md), and [delivery rules](../rules/delivery.md). The user's explicit instructions take precedence over default conventions.

When calling another workflow, read its entry point through the [workflow index](../docs/skills.md). The main workflow and subflows share one task record. Use the [record templates](../docs/task-records.md), and confirm available tools through the [tool inventory](../tools/README.md).

- Path base: use the user-selected task workspace for inputs and generated artifacts. Resolve documentation links relative to this installed file; follow the shared layout rules. Never write into the installed skill.

- Use case: record task actions/results for another person or AI to continue extraction, downloads, conversion, mapping, repairs, queries, and checks.
- Required inputs:
  - Task type, user request, and processing scope.
  - Game and project version; mark not applicable for game-independent tasks.
  - Current task identifier, input paths, and existing record locations, if any.
  - Actual actions, artifacts, results, and evidence provided by the caller.
- Outputs:
  - A task-record directory under context.
  - Task summary, stage status, operation logs, and report entry points.
  - Unfinished items, blockers, and next steps.
- Boundaries:
  - Organize existing facts only; do not automatically extract, check, compile, or run validation to fill out a report.
  - Do not present planned work as complete or historical results as current.
  - Reference large artifacts without duplicating them. Use workspace-relative paths; identify environments for external paths.
  - The coordinating workflow and conversion workflows share one task record and update their respective stages.
  - context holds task history and artifact entry points; file indexes accompany outputs, APK information goes in info, and long-term instructions belong in docs. Do not duplicate the same index.
- Steps:
  1. Create a record directory for a new task according to naming rules. For continuation, reuse the original identifier and record the current continuation time and scope.
  2. Save the user goal, game/version, sources and known hashes, targets, scope, and execution boundaries; mark unknown facts as unknown.
  3. Update context at task start, major-stage completion/failure, and finish/pause, distinguishing pending, running, completed, partial, failed, blocked, and cancelled states.
  4. Record actual tool versions, key parameters, inputs/outputs, success/failure counts, errors, and logs. Save detailed logs separately and keep only key facts in summaries.
  5. List artifact flow, actual changes, performed/unperformed checks, outstanding issues, and new/updated common tools. Reference resource reports, mapping lists, change lists, or query evidence appropriate to the task.
  6. Identify reusable artifacts, failed stages, retry prerequisites, and next steps. After abnormal interruption, inspect remaining artifacts rather than treating the last running stage as completed.
  7. Update context paths when artifact locations change. Put newly confirmed basic APK facts in info and provide reusable conclusions for docs.
- Completion criteria:
  - Another person can understand the goal, completed actions, artifact locations, omissions, and continuation point without reading the whole conversation.
  - Stages/status match actual results, with clear paths and evidence sources.
  - Distinguish completed, unfinished, unchecked, and inferred information.
  - Complete records do not mean successful business processing; failed, paused, and cancelled tasks also require complete records.
- Exceptions:
  - Missing action/result facts: mark unknown; do not invent history.
  - Concurrent stage updates: record each stage first, then aggregate status without overwriting others.
  - Records differ from artifacts: retain history and append discrepancies/corrections; do not silently rewrite old facts.
  - Record write failure: explicitly state records were not saved and what is missing, retaining information for later recovery.
  - Follow shared rules for interruptions, duplicate names, and overwrites.
