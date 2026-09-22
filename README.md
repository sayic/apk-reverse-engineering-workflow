# Game APK Reverse Engineering

[English](README.md) | [简体中文](README.zh-CN.md)

> For learning and reference purposes only.

A self-contained agent skill for structured, traceable, and resumable APK analysis. Install **one skill** to access **12 workflows** for resource extraction, code recovery, data-table analysis, and Unity asset work.

## Install

Prerequisites: Node.js with npm/npx, Git available on PATH, and a supported coding agent. The checked installer version, skills 1.7.0, requires Node.js 22.20.0 or later.

Run from the project where you want the skill available:

```bash
npx skills add sayic/game-apk-reverse-engineering --skill apk-reverse-engineering --copy
```

Follow the installer prompts when shown. To select an agent explicitly:

```bash
# Codex
npx skills add sayic/game-apk-reverse-engineering --skill apk-reverse-engineering --agent codex --copy

# Cursor
npx skills add sayic/game-apk-reverse-engineering --skill apk-reverse-engineering --agent cursor --copy

# Claude Code
npx skills add sayic/game-apk-reverse-engineering --skill apk-reverse-engineering --agent claude-code --copy
```

These commands use project-level installation. Add --global if you intentionally want a user-wide installation. --copy keeps the references with the installed skill without requiring symbolic links. You do not need to publish this repository as an npm package: npx runs the [Skills CLI](https://github.com/vercel-labs/skills), which installs the skill from GitHub.

To inspect discoverable skills without installing:

```bash
npx skills add sayic/game-apk-reverse-engineering --list
```

### Download instead

Download the repository using GitHub's **Code → Download ZIP**, extract it, and ask your coding agent to read:

```text
SKILL.md
```

Keep the complete skill folder, including references/. This manual workflow does not automatically register a skill with the agent.

## Platform compatibility

| Coding agent | Installer target | Isolated installation check | Actual agent/APK workflow |
| --- | --- | --- | --- |
| Codex | codex | Passed with Skills CLI 1.7.0, local source, project copy mode | Not validated |
| Cursor | cursor | Passed with Skills CLI 1.7.0, local source, project copy mode | Not validated |
| Claude Code | claude-code | Passed with Skills CLI 1.7.0, local source, project copy mode | Not validated |

The installation checks confirm skill discovery, copied package contents, and internal document references on Windows. They do not establish automatic activation or successful APK/Unity processing inside each application. Global and symbolic-link installation modes have not been checked. These are coding-agent integrations, not a claim that ordinary web chat interfaces can run the workflow.

See the installer's [supported agents](https://github.com/vercel-labs/skills#supported-agents) for current routing. Other platforms are not claimed as tested by this project.

## Use

Open your project in the coding agent, then provide the package or existing artifacts, a task workspace, and the scope. For example:

> Use apk-reverse-engineering to analyze the package at the path I provide. Extract code and config into my selected task workspace, reuse matching existing results, and report recovery limitations. Do not download remote resources.

For a query on existing results:

> Use apk-reverse-engineering to locate the configuration and code responsible for this behavior in my existing outputs. Keep the analysis read-only.

The entry skill selects the appropriate workflow and reads its supporting references as needed. For Unity import or repairs, also provide the target Unity project and configure a working Unity Editor/MCP connection.

## Included workflows

| Workflow | Purpose |
| --- | --- |
| [extract](references/workflows/extract.md) | Coordinate extraction of selected resource types from a supplied package |
| [container](references/workflows/container.md) | Parse containers, contained objects, offsets, and dependencies |
| [art](references/workflows/art.md) | Convert unpacked art and import into a provided Unity project |
| [code](references/workflows/code.md) | Recover readable code and identify recovery limits |
| [config](references/workflows/config.md) | Restore tables and export Excel or structured data |
| [download](references/workflows/download.md) | Download explicitly requested remote resources for a matching version |
| [mapping](references/workflows/mapping.md) | Map source data into specified target tables |
| [import](references/workflows/import.md) | Copy parsed art and dependencies into another Unity project |
| [repair](references/workflows/repair.md) | Repair specified material, texture, Shader, or effect problems |
| [query](references/workflows/query.md) | Trace values, logic, and resources in existing outputs |
| [check](references/workflows/check.md) | Check naming, layout, and index conventions |
| [context](references/workflows/context.md) | Record task facts, status, failures, and continuation points |

## Package and workspace

```text
game-apk-reverse-engineering/
├─ README.md
├─ README.zh-CN.md
├─ SKILL.md
├─ .gitignore
└─ references/
   ├─ workflows/
   ├─ rules/
   ├─ docs/
   ├─ tools/
   └─ templates/
```

All required instruction references and empty templates are inside the skill. The twelve workflows are supporting documents, not twelve independent installs. Maintain this single package; no duplicate top-level apkbreak/ workspace is required.

Keep the installed package read-only. Original packages, intermediate data, task records, tool caches, and outputs belong in a **user-selected task workspace**, not the installation directory. Existing workspace layouts are supported through explicit path mappings. See [workspace rules](references/rules/layout.md) and [task records](references/docs/task-records.md).

## External tools and limitations

This repository provides instructions, format notes, tool usage documents, and empty templates. It does **not** bundle the custom Python implementations mentioned in the documents, APKs, extracted game assets, Unity projects, or MCP servers.

Install and configure applicable external tools separately. Commands containing <configured-tool-directory> are interface examples, not ready-to-run bundled programs. The agent must confirm actual implementations, versions, dependencies, and format compatibility before execution.

Installing the skill does not guarantee complete source recovery, working Unity visuals, or compatibility with every package. Record generation, parsing, references, editor import, and runtime validation separately. See [tool documentation](references/tools/README.md) for details.
