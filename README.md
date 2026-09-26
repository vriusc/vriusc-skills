# vriusc-skills

这里是 Skillshare 的全局技能源仓库，保存当前使用的 34 个 Skill。技能按任务加载；阶段、调用时机和示例提示词见[交互式 Skill 工作流指南](./skill-workflow-guide.html)。

## 技能索引

| 常见阶段 | Skill |
| --- | --- |
| 理解与分析 | [`show-me`](./show-me/)、[`research`](./research/)、[`diagnosing-bugs`](./diagnosing-bugs/)、[`triage`](./triage/)、[`domain-modeling`](./domain-modeling/) |
| 方案澄清 | [`grilling`](./grilling/)、[`grill-me`](./grill-me/)、[`wayfinder`](./wayfinder/)、[`to-questionnaire`](./to-questionnaire/)、[`grill-with-docs`](./grill-with-docs/)、[`codebase-design`](./codebase-design/) |
| 方案验证 | [`to-spec`](./to-spec/)、[`to-tickets`](./to-tickets/)、[`archify`](./archify/)、[`prototype`](./prototype/) |
| 编码实现 | [`frontend-design`](./frontend-design/)、[`ui-ux-pro-max`](./ui-ux-pro-max/)、[`implement`](./implement/)、[`tdd`](./tdd/)、[`wizard`](./wizard/)、[`resolving-merge-conflicts`](./resolving-merge-conflicts/) |
| 验证与审查 | [`web-design-guidelines`](./web-design-guidelines/)、[`code-review`](./code-review/)、[`improve-codebase-architecture`](./improve-codebase-architecture/) |
| 交接与延续 | [`pr`](./pr/)、[`handoff`](./handoff/) |
| 学习与维护 | [`skillshare`](./skillshare/)、[`ask-matt`](./ask-matt/)、[`setup-matt-pocock-skills`](./setup-matt-pocock-skills/)、[`teach`](./teach/)、[`retro`](./retro/)、[`scaffold-exercises`](./scaffold-exercises/)、[`wait-what`](./wait-what/)、[`writing-for-agents`](./writing-for-agents/) |

## 来源与追踪

| 上游 | 数量 | Skill |
| --- | ---: | --- |
| [Matt Pocock](https://github.com/mattpocock/skills) | 28 | 工程与生产力流程；最初按 `c55ee46073ed923f86ce59a5eb3b6d895095d1b7` 快照安装。 |
| [Archify](https://github.com/tt-a1i/archify) | 1 | `archify`，跟踪 `main`。 |
| [Anthropic](https://github.com/anthropics/skills) | 1 | `frontend-design`。 |
| [HumanLayer](https://github.com/humanlayer/skills) | 1 | `show-me`。 |
| [Skillshare](https://github.com/runkids/skillshare) | 1 | `skillshare`。 |
| [UI UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) | 1 | `ui-ux-pro-max`。 |
| [Vercel](https://github.com/vercel-labs/agent-skills) | 1 | `web-design-guidelines`。 |

全部 34 项都在 [`.metadata.json`](./.metadata.json) 中记录了各自的 GitHub 来源和安装版本。Skillshare 将它们作为独立技能管理；`tracked_repos` 为空是正常状态，无需再用 `--track` 克隆整仓库。本仓库自身的 `main` 分支追踪 `origin/main`，用于保存选用的技能版本、来源记录和本指南。各技能版权及许可遵循其上游项目。

## 日常维护

Skillshare 从本仓库同步到 `~/.agents/skills`（Codex 通用目录）和 `~/.omp/agent/skills`（OMP），两个目标都使用 `merge` 模式。修改源目录后同步；同步只分发本地版本，不会自动拉取上游更新。

```bash
skillshare status -g --json             # 查看源与目标
skillshare check -g                     # 查看上游更新
skillshare update code-review -g -n    # 示例：预览单项更新
skillshare update code-review -g --diff
skillshare sync -g                      # 分发到两个目标
skillshare doctor --json                # 检查完整性与同步状态
```

更新前检查上游差异；按名称更新，避免一次覆盖所有技能。`skillshare update` 使用来源记录重新安装技能，可能替换对该技能所做的本地修改。暂存、提交和推送是独立的 Git 操作，只在本次任务明确授权时执行。
