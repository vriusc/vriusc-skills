# vriusc-skills

`vriusc-skills` 是一个面向 AI 编码代理的可复用技能集合，用于沉淀软件设计、实现、评审、调研与交付流程中的专业工作方法。仓库中的每个顶层技能目录均提供相应的 `SKILL.md`，供兼容的代理按任务需要加载和使用。

## Skills

以下简介根据各目录 `SKILL.md` 的 `description` 整理：

| Skill | 用途 |
| --- | --- |
| [`archify`](./archify/) | 创建并校验架构图、工作流图、时序图、数据流图和生命周期图，输出可探索的独立 HTML，并支持多种图片或视频格式导出。 |
| [`architecture-review`](./architecture-review/) | 对 PR、分支、命名空间或整个项目进行系统级架构审查，检查边界、依赖、分层、ADR 合规性和模块内聚性。 |
| [`architecture-review-greenfield`](./architecture-review-greenfield/) | 在增加重大功能前进行深度架构审查，重新审视现有技术栈并识别关键约束与驱动力。 |
| [`code-review`](./code-review/) | 按 Standards 与 Spec 两个维度审查未提交改动，或相对于分支、标签、commit、merge-base 的代码变更。 |
| [`codebase-design`](./codebase-design/) | 提供深模块设计的共享词汇，用于改进接口、确定 seam、提升可测试性和 AI 可导航性。 |
| [`diagnosing-bugs`](./diagnosing-bugs/) | 为疑难缺陷和性能回退提供结构化诊断循环。 |
| [`domain-modeling`](./domain-modeling/) | 构建和完善项目领域模型，统一领域术语，并记录架构决策。 |
| [`frontend-design`](./frontend-design/) | 为新建或重塑界面提供有辨识度的视觉设计指导，涵盖审美方向、字体和其他关键设计选择。 |
| [`grill-with-docs`](./grill-with-docs/) | 通过持续追问打磨计划或设计，并在过程中产出 ADR 和术语表。 |
| [`grilling`](./grilling/) | 通过高强度追问对计划、决策或想法进行压力测试。 |
| [`handoff`](./handoff/) | 将当前对话压缩为可供其他代理继续工作的交接文档。 |
| [`implement`](./implement/) | 实现明确的编码任务；对可执行行为变更采用测试优先方式，完成验证和最终 diff 检查，并保留未提交改动。 |
| [`improve-codebase-architecture`](./improve-codebase-architecture/) | 扫描代码库中的深模块改进机会，以可视化 HTML 报告呈现，并通过追问继续细化选定方案。 |
| [`pr-review-canvas`](./pr-review-canvas/) | 将 PR diff 渲染为独立 HTML 审查画布，按审查重要性组织变更，并区分样板代码、核心逻辑和复杂改动。 |
| [`prototype`](./prototype/) | 构建一次性最小原型，用于验证逻辑、状态模型、集成可行性或 UI 方向等不确定设计问题。 |
| [`research`](./research/) | 使用官方文档、规范、源码、论文和一方 API 等高可信一手资料研究技术或事实问题，并综合证据。 |
| [`resolving-merge-conflicts`](./resolving-merge-conflicts/) | 处理正在进行的 Git merge 或 rebase 冲突。 |
| [`review-me`](./review-me/) | 在用户显式调用时，以一次一个问题的只读决策树方式审查计划、设计或重要决策；代码 diff 缺陷应使用 `code-review`。 |
| [`skillshare`](./skillshare/) | 从单一来源管理并同步多个 AI 工具中的 skills 和 agents，支持安装、更新、审计、同步、备份以及项目或全局配置。 |
| [`solution-review`](./solution-review/) | 在实现前系统审查代码、配置、流程、工具或架构方案，覆盖有效性、风险、可行性、规范符合度及战略层面的决策质量。 |
| [`tdd`](./tdd/) | 指导测试驱动开发，包括红—绿—重构流程和集成测试。 |
| [`teach`](./teach/) | 在当前工作区内教授新的技能或概念。 |
| [`thermos`](./thermos/) | 在用户显式调用时，对指定 diff 进行综合审查，覆盖正确性、安全性、破坏性变更、开发体验、实现质量、可维护性和结构复杂度。 |
| [`to-questionnaire`](./to-questionnaire/) | 将自己无法完整回答的决策问题整理成问卷，交由掌握相关知识的人补充。 |
| [`track-comment`](./track-comment/) | 拉取 GitHub 或阿里云 CodeUp 的审查评论，与本地检出代码核对，并转化为有证据的具体问题和修改计划。 |
| [`u9-purchase-delivery-date`](./u9-purchase-delivery-date/) | 为 U9C 中精确指定的采购订单行更新“采购交期（内部）”，并在输入不完整时先执行必要检查。 |
| [`ui-ux-pro-max`](./ui-ux-pro-max/) | 为 Web、移动端和桌面端的 UI/UX 设计、构建、审查与修复提供设计情报，覆盖组件、设计系统、无障碍、交互、响应式布局、字体、色彩和图表等主题。 |
| [`wait-what`](./wait-what/) | 当上一条信息未能清楚传达时，暂停当前讨论并要求对方重新说明。 |
| [`web-design-guidelines`](./web-design-guidelines/) | 按 Web 界面指南审查 UI 代码，检查无障碍、UX 和通用最佳实践。 |
| [`writing-for-agents`](./writing-for-agents/) | 指导面向代理的文档写作，包括 skills、`AGENTS.md` 和 `CLAUDE.md`。 |
