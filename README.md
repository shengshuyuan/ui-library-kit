# UI Library Kit

> An Agent Skill that diagnoses visual/interaction issues on an existing frontend page first, then picks, integrates and verifies free, clearly-licensed components from 10 curated UI sources — without replacing your stack or theme. Works with Codex, Claude Code and Cursor. Chinese docs below.

为现有前端页面诊断视觉和交互问题，再从 10 个 UI 来源中选择、接入与验证合适组件。适合希望保留现有技术栈和主题的开发者。

这是独立的 Agent Skill，不是组件合集，也不是这些来源的官方产品。无需预先配置 MCP；不捆绑第三方组件源码，不保证这些来源所有组件都能直接安装。

当前版本：**0.5.0**。新增来源按需读取，保持现有项目优先，不会每次遍历所有网站。

最简单的用法是一句话：

> 用 ui-library-kit 优化这个页面。

技能会自己判断页面类型（后台 / 落地页 / 表单 / 阅读 / AI 对话）和适合的风格基调，读出项目当前风格，然后决定走哪一级：**A 调排版**（不加依赖，直接做）、**B 在现有框架内补 1–2 个免费组件**（小依赖直接做，大依赖先问）、**C 现有框架撑不住**（只给迁移方向和路径，不自动迁移，但仍先把 A 级能改的改掉）。报告里会写明"风格判断"和"方案等级"，让你知道它为什么这样做。

## 快速开始

把完整的 `ui-library-kit` 文件夹放进你的客户端技能目录：

| 客户端 | 用户级目录 | 显式调用 |
| --- | --- | --- |
| Codex | `~/.codex/skills/ui-library-kit/` | `$ui-library-kit` |
| Claude Code | `~/.claude/skills/ui-library-kit/` | `/ui-library-kit` |
| Cursor | `~/.cursor/skills/ui-library-kit/` | 在对话中附加或点名该技能 |

新开会话后说：

> 用 ui-library-kit 优化当前页面，先诊断问题，只用免费组件，沿用现有技术栈和配色。

也可以只要建议：

> 用 ui-library-kit 只读检查这个表单，说明最值得调整的位置，不修改文件。

或指定具体效果：

> 用 ui-library-kit 给 AI 输入框增加生成反馈，先复用现有组件，需要新依赖时说明理由。

从 GitHub 安装：

```sh
npx skills add shengshuyuan/ui-library-kit --skill ui-library-kit
```

完整安装、更新、移除和排错见 [安装说明](references/installation.md)。CLI 安装需要其支持的 Node.js；手动复制不需要。读取最新文档需要网络，构建/视觉检查使用项目已有工具。

## 它怎样工作

1. **诊断**：检查项目约束与页面，找信息层级、密度、分组和反馈问题。
2. **风格判断与分级**：识别页面类型定基调，读出当前风格与不一致处；按 A 排版调优 → B 框架内升级 → C 换方向建议的顺序选等级，停在第一个成立的。
3. **选型**：按路由表定位一两个来源，核实当前官方文档、版本与许可。
4. **接入**：先说明位置、理由、来源和依赖，再用最小依赖接入，复用现有主题与真实业务状态。
5. **验证**：构建、桌面/窄屏渲染、键盘与 reduced-motion、深浅色；有条件时在同一视口和状态比较前后页面。

适用于现有项目的组件选型与局部界面优化。新产品完整视觉设计、后端实现、框架迁移不是本技能的核心职责。非 React 项目先核实兼容实现，不自动引入 React。

仅使用各站免费且许可明确的内容，不接入付费模板或 Pro 授权。Bencho 在源码和许可未确认前仅作灵感参考。

## 来源与证据范围

| 来源 | 适用方向 | 当前状态 |
| --- | --- | --- |
| [BoardUI](https://www.boardui.com/) | 后台、图表、Agent 界面 | 官方入口已核实，组件接入未实测 |
| [Bencho](https://bencho.dev/) | 微交互灵感 | 源码/许可未确认前仅作参考 |
| [HeroUI](https://heroui.com/) | 表单等基础组件 | 官方入口已核实，组件接入未实测 |
| [Libraries.dev](https://libraries.dev/) | AI 状态和视觉效果 | 仅 border-beam@1.4.1 完成示例接入验证 |
| [Watermelon UI](https://ui.watermelon.sh/) | 区块、仪表盘、模板 | 官方入口已核实，组件接入未实测 |
| [React Bits](https://www.reactbits.dev/) | 文本动画、创意背景 | CountUp 示例接入通过；MIT + Commons Clause / Pro 分开 |
| [Animate UI](https://animate-ui.com/docs/installation) | React/Vue 组件动画 | 文档已核实，未实测；MIT + Commons Clause；先 dry-run |
| [Kibo UI](https://www.kibo-ui.com/docs/setup) | 复杂业务组件 | 文档已核实，未实测；依赖 shadcn 与变量主题 |
| [Magic UI](https://magicui.design/docs/installation) | 展示区块与动效 | Number Ticker 示例接入通过；免费与 Pro 分开 |
| [Aceternity UI](https://ui.aceternity.com/docs/cli) | 营销区块与视差卡片 | Bento Grid 示例接入通过；按条目核实授权 |

内容核实日期：2026-11-05；入口 URL 于 2026-11-07 复检可达。具体证据及维护规则见 [支持说明](references/support.md)。Codex、Claude Code、Cursor 在本机验证过发现/调用；其他客户端及系统没有全面验证。

本版风格判断与分级的评测见 [0.5.0 记录](evals/results-0.5.0.md)；结构重构与验收见 [0.4.0 验收报告](evals/release-0.4.0.md)；免费范围调整见 [0.3.2 验收报告](evals/release-0.3.2.md)；实际组件接入与浏览器证据见 [0.3.1 验收报告](evals/release-0.3.1.md)。

## 示例与验收

[选型示例](references/examples.md)展示建议如何表达，不能当作实际产品前后效果证明。

[评测案例](evals/cases.json)覆盖组件复用、Next.js 接入、Vue、付费内容范围、无网络、只读请求、无关任务、新增来源的转换范围、同名组件、许可边界，0.4.0 新增的英文触发、包管理器换算和深浅色回归，0.4.1 新增的无组件库项目、整站路由审查和自动化检查误报，以及 0.5.0 新增的一句话模式、风格自动判断和 C 级换方向建议。[评测方法](evals/README.md)说明如何对比旧版本、保存证据及判断结果。评测结论见 [0.2.0 记录](evals/results.md)、[0.3.0 记录](evals/results-0.3.0.md)、[0.4.0 记录](evals/results-0.4.0.md)（新增三案例 + 两回归案例通过）、[0.4.1 记录](evals/results-0.4.1.md) 与 [0.5.0 记录](evals/results-0.5.0.md)。行为评测与真实 UI 验收分开记录，不声称能自动保证美观。

## 许可与贡献

本技能原创指令和文档采用 [MIT](LICENSE)，作者为 shengshuyuan。这些来源的商标、代码、模板及素材属于各自权利人，使用时另行核实其许可；本技能不接入第三方付费内容。React Bits、Animate UI 的 Commons Clause 等额外条件仍需遵守，不能把"免费"当作"无限制"。

反馈时提供客户端、技能版本、框架/组件库版本、复现提示词与预期行为。请使用脱敏样例，不提交密钥、账号信息、私有业务源码或未授权截图。改动需保持主文件简短，来源细节放 references；不要无目的扩大组件库数量。
