# Workflow Index

Install or invoke the single [apk-reverse-engineering skill](../../SKILL.md). The following documents are its internal workflows; do not install them separately.

- [Extract resources from an APK](../workflows/extract.md)
- [Convert unpacked art and import into Unity](../workflows/art.md)
- [Recover unpacked code and organize projects](../workflows/code.md)
- [Restore data tables and export to Excel](../workflows/config.md)
- [Download and organize remote resources](../workflows/download.md)
- [Map tables and modify specified targets](../workflows/mapping.md)
- [Repair effects, materials, and textures](../workflows/repair.md)
- [Check naming, layout, and index conventions](../workflows/check.md)
- [Create and update task records](../workflows/context.md)
- [Query and analyze existing outputs](../workflows/query.md)
- [Parse resource containers](../workflows/container.md)
- [Import parsed art into another project](../workflows/import.md)

Example request:

> Use apk-reverse-engineering to extract code and config from the package I provide. Use my selected task workspace, reuse matching existing results, and record recovery limitations.

Supply the actual package path and workspace. If the version cannot be established from evidence, ask instead of guessing. If continuing a task, provide its existing context directory. All called workflows share one task record.
