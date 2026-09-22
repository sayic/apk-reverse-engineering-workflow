# apkbreak Management Directory

A shared workspace for APK resource extraction and analysis. Read the rules, select a skill for the task, use tools, and leave records that support continuation.

| Directory | Responsibility |
| --- | --- |
| apkbreak_rules | Naming, layout, execution, and delivery rules |
| apkbreak_info | CSV files containing basic APK information |
| apkbreak_tools | Organized common tools, game-specific tools, and usage documentation |
| apkbreak_skills | SKILL.md files for 12 task types |
| apkbreak_docs | Usage guides and durable knowledge |
| apkbreak_context | Task records and templates |
| apkbreak_apks | Original installation packages and accompanying inputs |
| apkbreak_unpacked | Intermediate extraction, download, and conversion artifacts for each task |

This directory centralizes all apkbreak_* management directories. Intermediate artifacts are under apkbreak_unpacked/; game outputs remain at the parent repository root and are created as needed.

1. Read the [workflow](apkbreak_docs/workflow.md) and [execution rules](apkbreak_rules/execution.md).
2. Select a task from the [skill index](apkbreak_docs/skills.md) and provide the inputs and scope.
3. Register the APK in [info](apkbreak_info/README.md) and record results using the [context templates](apkbreak_context/README.md).

AI entry point: [AGENTS.md](../AGENTS.md). See the [tool overview](apkbreak_tools/README.md) for the tool inventory and applicability. An initial set of 10 command-line tools has been organized; real APK processing and resource-project acceptance have not been performed. External programs still need environment registration; SVN commits are at the user's discretion.

All recorded paths and tool commands remain relative to the apkbreakdown root. Return to the [repository entry point](../README.md).
