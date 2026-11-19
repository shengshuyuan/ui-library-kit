# UI Library Kit

> An Agent Skill that diagnoses visual/interaction issues on an existing frontend page first, then picks, integrates and verifies free, clearly-licensed components from 11 curated UI sources — with a choice of light polish, targeted upgrades or a full visual redesign on your existing stack. Works with Codex, Claude Code and Cursor. Chinese docs below.

为现有前端页面诊断视觉和交互问题，再从 11 个 UI 来源中选择、接入与验证合适组件。支持保留现有效果精修、局部升级和整体视觉改版，沿用现有技术栈。

这是独立的 Agent Skill，不是组件合集，也不是这些来源的官方产品。无需预先配置 MCP；不捆绑第三方组件源码，不保证这些来源所有组件都能直接安装。

当前版本：**0.7.0**。新增来源按需读取，保持现有项目优先，不会每次遍历所有网站。

最简单的用法是一句话：

> 用 ui-library-kit 优化这个页面。

先选择改动幅度，技能再独立判断实现成本：

| 优化幅度 | 适用诉求 |
| --- | --- |
| 轻量优化 | 保留现有效果，精修字阶、留白、对齐和状态细节 |
| 局部升级 | 保持全局风格，升级指定区块与交互 |
| 整体改版 | 保留业务和技术栈，重新组织目标页面的布局与视觉语言 |

已指定幅度时直接沿用；没有指定时给出三个选项。优先复用项目组件、已安装库和兼容的免费区块，再补写缺口。A/B/C 表示实现成本，不表示视觉变化大小：整体改版也可以只用已有组件和 CSS 完成 A 级实现，框架迁移仍是独立决策。

新增 [设计方法参考](references/design-methods.md)，把信息层级、空间节奏和视觉一致性落到组件与验收；结合 Vibe Design Tips 已读取的加载状态、界面连续性、中英文排版文章提炼细节方法。装饰特效按需采用，不默认堆叠动效。

例如：

> 用 ui-library-kit 保留现有效果精修这个页面。
>
> 用 ui-library-kit 升级表单区块，尽可能复用现有组件。
>
> 用 ui-library-kit 整体改版这个落地页，保留业务功能和技术栈。


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
2. **选择幅度与设计方向**：轻量优化、局部升级或整体改版；再独立判断 A 现有能力实现、B 框架内升级、C 迁移建议。
3. **复用与选型**：先查项目已有实现和已安装库，需要外部组件时才查一两个来源，核实文档、版本与许可。
4. **接入**：先说明位置、理由、来源和依赖，再用最小依赖接入，复用现有主题与真实业务状态。
5. **验证**：构建、桌面/窄屏渲染、键盘与 reduced-motion、深浅色；有条件时在同一视口和状态比较前后页面。

适用于现有项目的视觉优化、整体页面改版和组件接入。后端实现与框架迁移不是本技能的核心职责。非 React 项目先核实兼容实现，不自动引入 React。

仅使用各站免费且许可明确的内容，不接入付费模板或 Pro 授权。Bencho 官方免费仓库为 MIT，Pro 条目不纳入；Dissolve 的网页实现按官方 Web 源码适配，不在网页项目安装 React Native 主包。

## 来源与证据范围

| 来源 | 适用方向 | 当前状态 |
| --- | --- | --- |
| [BoardUI](https://www.boardui.com/) | 后台、图表、Agent 界面 | 官方入口已核实，组件接入未实测 |
| [Bencho](https://bencho.dev/) | 标签、按钮反馈、上传和微交互 | 官方 MIT 免费仓库与 registry 已核实；Pro 排除，接入未实测 |
| [Dissolve](https://dissolve.rselmi.com/) | 29 项状态/手势/触感组件与粒子效果 | 官方 MIT、组件索引与代表 Web 源码已核实；移植未实测 |
| [HeroUI](https://heroui.com/) | 表单等基础组件 | 官方入口已核实，组件接入未实测 |
| [Libraries.dev](https://libraries.dev/) | AI 状态和视觉效果 | 仅 border-beam@1.4.1 完成示例接入验证 |
| [Watermelon UI](https://ui.watermelon.sh/) | 区块、仪表盘、模板 | 官方入口已核实，组件接入未实测 |
| [React Bits](https://www.reactbits.dev/) | 文本动画、创意背景 | CountUp 示例接入通过；MIT + Commons Clause / Pro 分开 |
| [Animate UI](https://animate-ui.com/docs/installation) | React/Vue 组件动画 | 文档已核实，未实测；MIT + Commons Clause；先 dry-run |
| [Kibo UI](https://www.kibo-ui.com/docs/setup) | 复杂业务组件 | 文档已核实，未实测；依赖 shadcn 与变量主题 |
| [Magic UI](https://magicui.design/docs/installation) | 展示区块与动效 | Number Ticker 示例接入通过；免费与 Pro 分开 |
| [Aceternity UI](https://ui.aceternity.com/docs/cli) | 营销区块与视差卡片 | Bento Grid 示例接入通过；按条目核实授权 |

内容核实日期：2026-11-05；入口 URL 于 2026-11-07 复检可达。具体证据及维护规则见 [支持说明](references/support.md)。Codex、Claude Code、Cursor 在本机验证过发现/调用；其他客户端及系统没有全面验证。

风格判断与分级的评测见 [0.5.0 记录](evals/results-0.5.0.md) 与 [0.5.1 记录](evals/results-0.5.1.md)；结构重构与验收见 [0.4.0 验收报告](evals/release-0.4.0.md)；免费范围调整见 [0.3.2 验收报告](evals/release-0.3.2.md)；实际组件接入与浏览器证据见 [0.3.1 验收报告](evals/release-0.3.1.md)。

## 示例与验收

[选型示例](references/examples.md)展示建议如何表达，不能当作实际产品前后效果证明。

[评测案例](evals/cases.json)覆盖组件复用、Next.js 接入、Vue、付费内容范围、无网络、只读请求、无关任务、新增来源的转换范围、同名组件、许可边界，0.4.0 新增的英文触发、包管理器换算和深浅色回归，0.4.1 新增的无组件库项目、整站路由审查和自动化检查误报，0.5.0 新增的一句话模式、风格自动判断和 C 级换方向建议，以及 0.5.1 新增的落地页、阅读页、AI 对话页三类风格案例。[评测方法](evals/README.md)说明如何对比旧版本、保存证据及判断结果。评测结论见 [0.2.0 记录](evals/results.md)、[0.3.0 记录](evals/results-0.3.0.md)、[0.4.0 记录](evals/results-0.4.0.md)（新增三案例 + 两回归案例通过）、[0.4.1 记录](evals/results-0.4.1.md) 、[0.5.0 记录](evals/results-0.5.0.md) 与 [0.5.1 记录](evals/results-0.5.1.md)。行为评测与真实 UI 验收分开记录，不声称能自动保证美观。

## 许可与贡献

本技能原创指令和文档采用 [MIT](LICENSE)，作者为 shengshuyuan。这些来源的商标、代码、模板及素材属于各自权利人，使用时另行核实其许可；本技能不接入第三方付费内容。React Bits、Animate UI 的 Commons Clause 等额外条件仍需遵守，不能把"免费"当作"无限制"。

反馈时提供客户端、技能版本、框架/组件库版本、复现提示词与预期行为。请使用脱敏样例，不提交密钥、账号信息、私有业务源码或未授权截图。改动需保持主文件简短，来源细节放 references；不要无目的扩大组件库数量。

0.6.0 已完成技能格式、引用检查与三种请求的独立只读推演，并修正范围和分级冲突；尚未用真实页面验证本版优化效果，历史评测不代表新版端到端验收。

0.7.0 深化 [Bencho](references/bencho.md) 的免费组件接入与 [Dissolve](references/dissolve.md) 的组件/源码映射。选择时说明真实页面问题、复用路径、状态接线与验收范围，不把上游演示效果等同于已完成项目接入。

本版研究范围、独立推演与限制见 [0.7.0 验证记录](evals/results-0.7.0.md)。
