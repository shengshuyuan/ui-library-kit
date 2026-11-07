# 支持程度与维护

状态记录日期：2026-11-05；2026-11-07 复检 10 个来源的主要入口 URL 均返回 HTTP 200（仅确认可达，未逐页重读内容）。入口核实不是端到端安装证明；下列状态针对本技能的证据，不是对上游质量评分。

| 来源 | 本技能验证程度 | 接入条件 |
| --- | --- | --- |
| BoardUI | 官方 skill/CLI/MCP 文档已核实；尚无组件接入实测 | 核实框架及样式版本，免费与 Pro 分开 |
| Bencho | 公开交互目录已核实；源码获取与许可未确认 | 未确认前仅作视觉参考 |
| HeroUI | 官方索引、组件和 MCP 文档已核实；尚无组件接入实测 | 核对主版本，不混用 v2/v3 API |
| Libraries.dev | border-beam@1.4.1 在临时 React 示例完成构建与浏览器交互验证 | 此证据不覆盖所有效果或所有框架 |
| Watermelon UI | 官方目录与 MCP 文档已核实；尚无组件接入实测 | 逐条核实来源、依赖和许可 |
| React Bits | CountUp-TS-CSS 完成 CLI 安装、类型检查、构建与浏览器渲染 | JS/TS 与 CSS/TW 版本分开，注意 Commons Clause 与 Pro |
| Animate UI | 当前 React 安装文档与 Vue 更新记录已核实；转换未实测 | 区分 v1/v2、React/Vue；先单项 dry-run；MIT + Commons Clause 单独核实 |
| Kibo UI | 官方 setup 与组件目录已核实；组件未实测 | 已有 shadcn 与 CSS Variables 主题优先 |
| Magic UI | Number Ticker 完成 CLI 安装、类型检查、构建、状态更新与浏览器验证 | 免费与 Pro 分开，不混用同名 Border Beam API |
| Aceternity UI | Bento Grid 完成 CLI 安装、类型检查、构建与响应式/主题验证 | 免费与 All-Access 分开，避免误跑新项目初始化 |

Codex、Claude Code 已在本机新进程验证发现/调用；Cursor 会话中已验证可从 `~/.claude/skills` 发现并附加该技能。公开 GitHub 仓库已通过 Skills CLI 在临时项目安装到 Codex 与 Claude Code 目录，并逐文件比对。其他客户端、Windows/Linux 尚未验证。公开文档不能把这些写成全面兼容。

当前只使用上述来源的免费且许可明确内容；Pro/All-Access 内容不在接入范围。纯付费 beUI Pro 已从当前来源移除，历史报告保留原验证范围。

## 更新规则

- 每次用到某来源时按实际项目版本核实相关页面，不每次抓取各来源整站。
- 入口变化时只更新对应参考并记录日期；真实接入后再提升支持状态，保留版本和验证范围。
- 同一入口失败后可尝试其官方仓库或版本文档；仍无证据则说明缺口，使用本地已有方案，不猜 API。
- 发布前检查相关链接、包结构、安装和行为案例。过期参考只提供检索线索，不能当作当前行为证明。

## 本轮未纳入默认来源的候选

AI Elements 官网检索结果在 2026-11-05 显示迁往 shadcn/ui，并计划于 2026 年 11 月底弃用旧项目。新项目先核实迁移后的官方组件入口，不把旧 ai-elements 全量安装命令加入默认流程。证据入口：https://elements.ai-sdk.dev/ （使用前重查公告）。

Prompt Kit 文档本轮读取返回 403，尚无足够的当前安装证据，因此未加入默认来源。这是访问缺口，不代表项目不可用。
