# plan-first

为 AI 编码助手提供需求澄清、可行性评审和按计划实施的工作流程。先说明项目给谁用、解决什么问题、从哪里使用、交付什么，再用验收证据推进工程任务。

[v0.5.0 发布包](https://github.com/ztian11/plan-first/releases/tag/v0.5.0) · [文档模板](./references/plan-template.md) · [完整示例](./examples/example-plan.md) · [变更记录](./CHANGELOG.md) · [MIT](./LICENSE)

**English overview** — plan-first is an instruction-based skill for coding agents. It produces a concise product requirements document and an engineering appendix, verifies prerequisites, tracks confirmed scope, and resumes work using actual results and acceptance evidence. Plans are saved under the project's `plans/` directory by default. Templates and examples are written in Chinese.

## 快速开始

1. 从发布页下载 `plan-first.zip` 并解压。
2. 按下方安装步骤，把包含 `SKILL.md` 的目录安装到宿主的 Skill 目录。
3. 在目标工程中调用 `plan-first`，给出要完成的事情和已有约束。
4. 查看生成的需求正文、待确认事项和计划文件路径，再按已确认范围实施。

在支持 `$` 调用的 Codex 客户端中，可以这样开始。

```text
$plan-first 为博客增加账号登录和评论功能。
面向博客读者，支持桌面 Chrome，先在开发环境验收。
请先给需求文档和实施计划，保存到 plans/。
```

## 安装与升级

当前 Codex 官方文档列出的用户级 Skill 目录是 `~/.agents/skills/`，项目级目录是 `.agents/skills/`。以下命令使用用户级目录。已有安装应沿用实际加载路径进行升级，避免产生同名副本。其他宿主使用各自的 Skill 发现目录，并核对模板可以读取。[Codex Skill 文档](https://learn.chatgpt.com/docs/build-skills)

解压后，进入包含 `SKILL.md` 的 `plan-first` 目录，再执行对应系统的命令。

### Windows PowerShell

```powershell
$ErrorActionPreference = 'Stop'
$skillSource = (Get-Location).Path
$skillTarget = Join-Path $env:USERPROFILE '.agents\skills\plan-first'

if (-not (Test-Path -LiteralPath (Join-Path $skillSource 'SKILL.md'))) {
    throw '请先进入包含 SKILL.md 的 plan-first 目录。'
}
if (Test-Path -LiteralPath $skillTarget) {
    throw '目标已存在，请按下方升级说明先比较本地改动。'
}

New-Item -ItemType Directory -Path $skillTarget -Force | Out-Null
foreach ($name in @('SKILL.md', 'references', 'examples', 'LICENSE')) {
    Copy-Item -LiteralPath (Join-Path $skillSource $name) -Destination $skillTarget -Recurse
}
```

### macOS / Linux

```bash
(
    set -eu
    skill_source="$PWD"
    skill_target="$HOME/.agents/skills/plan-first"

    if [ ! -f "$skill_source/SKILL.md" ]; then
        printf '%s\n' '请先进入包含 SKILL.md 的 plan-first 目录。' >&2
        exit 1
    fi
    if [ -e "$skill_target" ] || [ -L "$skill_target" ]; then
        printf '%s\n' '目标已存在，请按下方升级说明先比较本地改动。' >&2
        exit 1
    fi

    mkdir -p "$skill_target"
    cp "$skill_source/SKILL.md" "$skill_source/LICENSE" "$skill_target/"
    cp -R "$skill_source/references" "$skill_source/examples" "$skill_target/"
)
```

安装后检查目标目录直接包含 `SKILL.md` 和 `references/plan-template.md`。在宿主中确认已发现 `plan-first`，更新未出现时重新启动宿主。[Codex Skill 文档](https://learn.chatgpt.com/docs/build-skills)

### 升级已有安装

先下载并解压新版本，查看变更记录，再比较新旧 `SKILL.md`、`references/` 和 `examples/`。本地定制内容先备份或合并，再更新同一安装目录中的这些文件。`LICENSE` 随版本一并更新。

本项目早期安装说明使用过 `~/.codex/skills/plan-first/`。升级时以宿主显示的实际加载路径为准，检查新模板和主指令属于同一版本。将备用副本放在 Skill 扫描目录之外。

## 产出内容

完整规划是一份 Markdown 文件，由六节需求正文和工程附录组成。

| 部分 | 内容 |
|------|------|
| 1. 项目概览 | 当前问题、项目目标和预期结果 |
| 2. 目标用户与使用场景 | 谁使用、关键操作路径、重要例外 |
| 3. 呈现方式与支持终端 | 网页、App、API、CLI 等交付形态，使用入口、客户端和交付环境 |
| 4. 本期需求与范围 | 带编号的需求、业务规则及本期边界 |
| 5. 交付物与验收 | 最终可使用或接入的成果，以及可判定的验收条件 |
| 6. 待确认事项 | 未决问题、暂用假设及其影响 |
| 工程附录 | 调研证据、任务依赖、实施顺序、风险、评审、建议与变更记录 |

需求编号关联验收项和实施任务。API、库和批处理按调用方、入口和输入输出描述。工程附录只保留适用内容，已有结论通过编号引用。[模板](./references/plan-template.md) · [博客登录示例](./examples/example-plan.md)

## plan 保存在哪里

保存位置依次采用用户指定路径、项目明确约定的目录、项目根目录下的 `plans/`。新计划使用日期和任务名命名。

```text
your-project/
└── plans/
    └── 2026-11-14-blog-login.md
```

单仓库以 Git 仓库根目录为基准，monorepo 默认共用仓库根目录的 `plans/`。无 Git 的项目使用已明确的项目目录。跨仓库任务选定一个主保存位置，并在文档中记录涉及的仓库。

同一任务持续更新原文件，在文档内记录版本。新的同名任务使用文件名后缀。每次创建或实质更新后，助手返回计划的绝对路径，方便打开和交接。

恢复任务时，优先使用用户给出的文件或交接路径，其次在项目规划目录中查找与任务匹配的计划。候选有多份时确认目标计划，再核对成果与验收记录。已有计划保留原路径，新计划默认使用 `plans/`。

| 请求类型 | 交付与保存方式 |
|----------|----------------|
| 完整规划与实施 | 保存需求正文和工程附录，记录执行进度 |
| 只写 PRD | 默认在会话中交付正文，要求文件时保存 |
| 轻量任务 | 会话中给简要做法，需要跨会话交接时保存摘要 |
| 没有可写位置 | 会话中交付，并说明保存状态及恢复所需材料 |

## 更多使用方式

**只写需求草稿**

```text
$plan-first 只写需求文档。我们要给售后和仓储系统提供批量查询订单状态的 API，
交付测试环境。查询上限和权限模型待定，先出一份可评审的草稿。
```

**接着已有计划实施**

```text
$plan-first 继续 plans/2026-11-14-blog-login.md。
先核对已完成任务的成果和验收记录，再推进依赖已满足的任务。
```

**限定本期范围**

```text
$plan-first 按已确认的 v2 方案实施。终端和交付环境沿用该版本，
新增范围先更新需求和评审结论，再让我确认。
```

低风险单点小修可以直接处理。完整流程适用于多步骤或高影响工作，已明确的实施授权可以沿用。前置验证未通过时，受影响任务等待验证结果。具体执行规则见主指令。[SKILL.md](./SKILL.md)

## 仓库结构与版本

```text
plan-first/
├── SKILL.md
├── references/plan-template.md
├── examples/example-plan.md
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
└── LICENSE
```

`main` 保存开发中的最新内容，带版本标签的发布页提供固定版本和安装 ZIP。v0.5.0 引入需求正文与工程附录，默认保存到 `plans/`，并补齐项目根目录、文件更新和恢复定位规则。[发布记录](https://github.com/ztian11/plan-first/releases) · [变更记录](./CHANGELOG.md)

## 反馈与贡献

反馈时请附上 Skill 版本、宿主、最小请求、预期行为和实际结果。涉及保存路径时附上脱敏后的目录结构，便于复现。欢迎用真实场景改进触发、规划和恢复规则。[提交 Issue](https://github.com/ztian11/plan-first/issues/new) · [贡献说明](./CONTRIBUTING.md)

本项目采用 MIT 许可证。[LICENSE](./LICENSE)
