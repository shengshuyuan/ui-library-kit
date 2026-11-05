# 0.3.0 来源扩展验证

日期：2026-11-05。新增 5 个来源，合计 11 个。此前 0.2.0 记录保持原样，不把旧记录改称本轮新测试。

## 来源核实

- React Bits：官方 installation 与仓库 README，确认变体与源码安装入口；仓库标记 MIT + Commons Clause。
- Animate UI：当前 React installation 与 changelog，确认 v2 转换流程及 Vue 路线；不混用 v1 安装教程。
- Kibo UI：官方 setup，确认 shadcn 和 CSS Variables 前提及按组件安装入口。
- Magic UI：官方 installation 与仓库 README，确认 registry 安装方式及免费仓库许可标记；Pro 单独处理。
- Aceternity UI：官方 CLI 页面，确认 registry JSON 和命名空间依赖配置。
- AI Elements 迁移公告与 Prompt Kit 本轮文档 403，记录于 support.md；未作为新默认来源。

每项参考都包含核实入口；这些是文档证据，不是组件安装、渲染或法律审查证明。

## 独立文字场景评测

独立评测者只收到当前技能、必要参考及四个场景，未收到预期答案或已有报告；评测者未联网、安装或操作项目。随后按 cases.json 人工核对：

| 场景 | 结果 | 回答证据摘要 |
| --- | --- | --- |
| animate-scope | 通过 | 不跑 animate all，先目标 Accordion dry-run，保留定制并核查键盘/减少动态效果 |
| same-name | 通过 | Libraries.dev 与 Magic UI 的 Border Beam 属于不同实现，查看已装包类型/API |
| business-routing | 通过 | 甘特图优先 Kibo，检查 shadcn 变量主题、数据和时区，不加展示特效 |
| reactbits-license | 通过 | 不把 MIT + Commons Clause 简化为纯 MIT，不把技能 MIT 套用到第三方源码 |

这四个文字场景共享一个评测上下文，不证明模型总体稳定性或真实安装成功。原有七个场景未在本轮重新运行；没有报告美观度或性能提升。

## 包与安装验证

主文件 quick_validate、全部相对引用、无本机路径/内部软链接检查通过。ZIP 完整性通过；从 ZIP 解压出的独立副本使用 Skills CLI 为 Codex/Claude Code 安装成功，主文件、接口与全部参考资料逐字节一致。当前版本仅本地准备；没有发布公开 GitHub 仓库，不声称已完成远程安装。
