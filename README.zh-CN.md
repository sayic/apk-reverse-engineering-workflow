# 游戏 APK 逆向分析

[English](README.md) | [简体中文](README.zh-CN.md)

> 仅供学习和参考使用。

一套配套内容完整的 AI 技能，让 APK 分析过程有章可循、可追溯、可接续。安装 **1 个技能**，即可使用 **12 个工作流程**，涵盖资源提取、代码还原、数据表分析和 Unity 资源处理。

## 安装

前置条件：安装包含 npm/npx 的 Node.js，将 Git 加入 PATH，并使用支持的 AI 编程工具。本次检查使用的安装器版本为 skills 1.7.0，需要 Node.js 22.20.0 或更高版本。

在需要使用该技能的项目目录中运行：

```bash
npx skills add sayic/game-apk-reverse-engineering --skill game-apk-reverse-engineering --copy
```

出现安装提示时，按提示操作。也可以明确指定安装到哪个 AI 工具：

```bash
# Codex
npx skills add sayic/game-apk-reverse-engineering --skill game-apk-reverse-engineering --agent codex --copy

# Cursor
npx skills add sayic/game-apk-reverse-engineering --skill game-apk-reverse-engineering --agent cursor --copy

# Claude Code
npx skills add sayic/game-apk-reverse-engineering --skill game-apk-reverse-engineering --agent claude-code --copy
```

以上命令安装到当前项目。如果希望在当前用户的多个项目中使用，可以主动添加 `--global`。`--copy` 会将配套文档一并复制到技能目录，不依赖符号链接。

不需要把这个仓库发布成 npm 包：npx 运行的是 [Skills CLI 安装器](https://github.com/vercel-labs/skills)，由它从 GitHub 安装技能。

只查看可以发现的技能，不执行安装：

```bash
npx skills add sayic/game-apk-reverse-engineering --list
```

### 直接下载使用

在 GitHub 点击 **Code → Download ZIP**，解压后，让 AI 编程工具读取：

```text
SKILL.md
```

保留完整技能文件夹，包括 `references/`。这种手动使用方式不会自动把技能注册到 AI 工具中。

## 平台兼容性

| AI 编程工具 | 安装目标参数 | 隔离环境安装检查 | AI 工具内的实际 APK 工作流 |
| --- | --- | --- | --- |
| Codex | codex | 使用 Skills CLI 1.7.0，从本地来源按项目复制安装，通过 | 未验证 |
| Cursor | cursor | 使用 Skills CLI 1.7.0，从本地来源按项目复制安装，通过 | 未验证 |
| Claude Code | claude-code | 使用 Skills CLI 1.7.0，从本地来源按项目复制安装，通过 | 未验证 |

上述安装检查确认了 Windows 环境中的技能发现、文件复制完整性和内部文档引用。它们不代表已经验证各应用中的自动触发，也不代表实际 APK 处理或 Unity 操作已经跑通。全局安装和符号链接安装模式尚未检查。

这里所指的是 AI 编程工具集成，不表示普通网页聊天界面能够直接运行整套流程。

安装器当前支持的平台见 [支持的 AI 工具](https://github.com/vercel-labs/skills#supported-agents)。本项目不宣称已测试其他平台。

## 使用方法

在 AI 编程工具中打开项目，提供安装包或已有分析产物、任务工作目录和处理范围。例如：

> 使用 game-apk-reverse-engineering 分析我提供路径下的安装包。将 code 和 config 提取到我指定的任务工作目录，复用已有且匹配的结果，并说明还原限制。不要下载远程资源。

查询已有结果时，可以这样说：

> 使用 game-apk-reverse-engineering，在我已有的 outputs 中查找与这个行为有关的配置和代码。只读分析，不修改文件。

总入口会选择合适的工作流程，并按需读取配套说明。进行 Unity 资源导入或修复时，还需要提供目标 Unity 工程，并配置可用的 Unity Editor/MCP 连接。

## 包含的工作流程

| 工作流程 | 用途 |
| --- | --- |
| [extract](references/workflows/extract.md) | 统筹从指定安装包中提取所选类型的资源 |
| [container](references/workflows/container.md) | 解析资源容器、内部对象、偏移和依赖关系 |
| [art](references/workflows/art.md) | 转换已提取的美术资源，并导入用户提供的 Unity 工程 |
| [code](references/workflows/code.md) | 还原可阅读代码，并说明还原程度与限制 |
| [config](references/workflows/config.md) | 还原数据表，导出 Excel 或结构化数据 |
| [download](references/workflows/download.md) | 下载用户明确要求且版本匹配的远程资源 |
| [mapping](references/workflows/mapping.md) | 将来源数据映射并写入指定目标表 |
| [import](references/workflows/import.md) | 将已解析的美术资源及依赖复制到其他 Unity 工程 |
| [repair](references/workflows/repair.md) | 修复指定的材质、贴图、Shader 或特效问题 |
| [query](references/workflows/query.md) | 在已有 outputs 中追踪数值、逻辑和资源 |
| [check](references/workflows/check.md) | 检查命名、目录布局和索引是否符合约定 |
| [context](references/workflows/context.md) | 记录任务事实、进度、失败原因和接续位置 |

## 技能包与任务工作目录

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

所需的操作说明和空模板全部保存在技能包内。12 个工作流程属于配套文档，不需要分别安装。只维护这一套技能包，不再额外保留顶层的 `apkbreak/` 工作区。

已安装的技能包保持只读。原始安装包、中间数据、任务记录、工具缓存和处理结果，都保存在**用户指定的任务工作目录**中，不写入技能安装目录。

已有工作目录可以通过明确的路径映射继续使用。详见 [工作目录规则](references/rules/layout.md) 和 [任务记录说明](references/docs/task-records.md)。

## 外部工具与限制

本仓库提供操作说明、格式笔记、工具用法文档和空模板，**不包含**文档中提到的自定义 Python 工具实现、APK、提取出的游戏资源、Unity 工程或 MCP 服务。

适用的外部工具需要另行安装和配置。包含 `<configured-tool-directory>` 的命令是接口使用示例，不是随包提供、可以直接运行的程序。执行前，AI 工具需要确认真实工具实现、版本、依赖和格式兼容性。

安装技能不代表一定能够完整恢复源码、还原 Unity 显示效果，或兼容所有安装包。文件生成、格式解析、引用关系、编辑器导入和运行验证需要分别记录。详见 [工具说明](references/tools/README.md)。
