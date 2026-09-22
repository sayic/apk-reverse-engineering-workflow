---
name: check
description: "The user requests checking project directories, filenames, and storage locations against apkbreak_rules, or explicitly requests convention checks before delivery or submission."
---

# Check File and Directory Conventions

Before execution, read the [naming rules](../../apkbreak_rules/naming.md), [layout rules](../../apkbreak_rules/layout.md), [execution rules](../../apkbreak_rules/execution.md), and [delivery rules](../../apkbreak_rules/delivery.md). The user's explicit instructions take precedence over default conventions.

When calling another skill, read its entry point through the [skill index](../../apkbreak_docs/skills.md). The main workflow and subflows share one task record. Use the [record templates](../../apkbreak_context/README.md), and confirm available tools through the [tool inventory](../../apkbreak_tools/README.md).

- Path base: the repository root is apkbreakdown; management directories are under apkbreak/, intermediates under apkbreak/apkbreak_unpacked/, and outputs remain at the repository root. Recorded paths are relative to the repository root.

- Use case: the user requests checking directories, filenames, and storage locations against apkbreak_rules, or explicitly requests convention checks before delivery or submission.
- Required inputs:
  - Directory and scope to check.
  - Applicable naming, layout, and storage rules.
  - Game and project version, obtainable from info.
- Outputs:
  - A report containing rules, paths, issue types, and suggested actions.
  - Checked, unchecked, and indeterminate scope.
- Boundaries:
  - Check only by default; do not rename, move, delete, or change references automatically.
  - Apply only established rules; mark undefined rules as requiring confirmation rather than inventing standards.
  - Distinguish management files from original resources. Apply documented exceptions to Unity fixed directories, original resource names, classes, and fields.
  - Naming compliance does not establish completeness or runtime usability. Content parsing, compilation, and runtime acceptance are outside this skill.
  - Do not commit to SVN on your own.
- Steps:
  1. Read apkbreak_rules to establish targets, applicable rules, and exceptions. Record rule conflicts for confirmation rather than choosing arbitrarily.
  2. Prefer existing indexes, then enumerate the relevant directories. Exclude caches, version-control metadata, and generated directories by default; avoid whole-repository scans.
  3. Use tools to check names, locations, required files, and version identifiers. Check that paths referenced by info and indexes exist in the agreed locations.
  4. Distinguish definite violations, allowed exceptions, indeterminate items, and failed checks. Include exact paths and rules.
  5. Suggest names or locations. For Unity resources, project files, or code references, list affected references instead of renaming in bulk.
- Completion criteria:
  - The specified scope has been checked against established rules and a report produced.
  - Issues can be located by file and corresponding rule.
  - Distinguish "checks complete" from "fully compliant"; violations do not mean the checking task failed.
  - Identify inaccessible directories or inapplicable checks and mark checking incomplete.
- Exceptions:
  - Missing/conflicting rules: record questions and continue unaffected checks.
  - Unreadable files or changes during checking: record the actual situation rather than marking compliant.
  - Broken index paths: report without deleting indexes or migrating files.
  - Unsupported checks: list them as unchecked; normal tool exit is insufficient.
  - If the user requests fixes, establish the modification scope separately and follow continuation, conflict, and reference-protection rules.
