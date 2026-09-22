# APK Reverse Engineering Workflow

> For learning and reference purposes only.

Management content is centralized under [apkbreak/](apkbreak/README.md). Game resource projects and outputs remain at the repository root.

```text
apkbreakdown/
├─ AGENTS.md
├─ README.md
├─ apkbreak/
│  ├─ README.md
│  ├─ apkbreak_apks/
│  ├─ apkbreak_unpacked/
│  ├─ apkbreak_info/
│  ├─ apkbreak_rules/
│  ├─ apkbreak_tools/
│  ├─ apkbreak_skills/
│  ├─ apkbreak_docs/
│  └─ apkbreak_context/
└─ game_id_project_version_resource_type_outputs/
```

- AI entry point: [AGENTS.md](AGENTS.md)
- Directory rules: [layout.md](apkbreak/apkbreak_rules/layout.md)
- Task entry points: [skill index](apkbreak/apkbreak_docs/skills.md)
- Tool usage: [tool overview](apkbreak/apkbreak_tools/README.md)

Task records, repository paths in CSV/JSON, and tool commands are relative to the apkbreakdown root. Markdown links remain relative to their containing documents.

Only management directories have been consolidated; existing resource projects and historical unpacked directories are not moved automatically. SVN commits are at the user's discretion.
