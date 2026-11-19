# 支持程度与维护

状态日期：2026-11-19。区分目录/文档、源码、安装构建、浏览器与业务验收。下表的“本轮实测”仅指隔离验收夹具，不是对全部组件或生产服务的保证。11个来源均已有代表组件、组合方法和接线验收说明；不要求每次任务遍历所有网站。

| 来源 | 本轮吸收与核实 | 实际验证与边界 |
| --- | --- | --- |
| BoardUI | Agent Chat/Thinking/Log、Data Table/Grid、Notification Center与KPI组合；已读Agent Thinking源码与MIT | 本轮未安装；按条目免费/Pro和基础主题依赖核实 |
| Bencho | 48项免费目录、代表API、Like/Upload/OTP/Compare源码与registry；MIT免费/Pro分开 | 本轮Like受控适配、构建、连点/键盘、持久化及reduce浏览器检查通过；不覆盖所有条目 |
| Dissolve | 29项组件到Web模块映射，骨架/按钮/输入/核心源码、MIT与RN边界 | 本轮仅使用按钮触感/状态连续性的方法；没有移植或安装粒子引擎，不能报告官方Web组件接入通过 |
| HeroUI | v3 Table/TextField/Form实际源码，日期/选择/浮层组合；v3 Apache-2.0 | 本轮未安装；v2/v3及React/RN分开，当前版本约束按元数据核实 |
| Libraries.dev | Beam、Orbs、Voice Glow/Ring、Metal、Image等状态包覆方法和API | 历史border-beam@1.4.1示例构建/浏览器通过；本轮未重跑全部包，后续目录许可与WebGL降级逐项核实 |
| Watermelon UI | Catalog与5项区块路径，auth-01 registry内嵌源码、repo MIT；认证接线与标签缺口 | 本轮未安装；官方源码文件与外部素材分别核许可，不把界面当认证服务 |
| React Bits | 标题/内容/背景组合；CountUp源码/registry、Motion依赖、初始reduce分支；MIT+Commons Clause | 历史CountUp示例安装/构建/渲染通过；本轮源码复核，未覆盖其他条目 |
| Animate UI | Accordion/Dialog/Navigation Menu方法与当前v2文档；v1 Accordion源码作实现样例 | 本轮未运行转换；v1源码不能代表v2发布实现，CLI先单项dry-run；MIT+Commons Clause |
| Kibo UI | Gantt/Dropzone/Color Picker/Code Block组合，Dropzone源码/包依赖与MIT全文 | 本轮未安装；已有shadcn/CSS变量优先，Dropzone只是文件输入，不是上传后端 |
| Magic UI | Bento、TextAnimate、AnimatedList等方法，NumberTicker/Bento源码与MIT | 本轮Bento源码适配构建、桌面/320px渲染与显式CTA路由检查；其他组件不外推 |
| Aceternity UI | Bento/Hero Parallax/Moving Border实际registry源码，营销页组合、尺寸和运动风险 | 历史Bento示例验收记录保留；本轮逐项源码授权仍需确认，不自动复制许可未闭合项；可用构图方法或MIT替代 |

Vibe Design Tips 是设计方法参考，已读加载、连续性、中英文排版三篇正文；其他文章作为按需阅读入口，不计作“全部文章已吸收”。

## 证据和客户端范围

0.8.0新增页面组合配方、代表源码路径与临时React验收。独立文档推演与真实UI检查分开记录，详见 [results-0.8.0.md](../evals/results-0.8.0.md)。格式和构建通过不能替代视觉/交互结论。

历史记录：Codex、Claude Code本机新进程发现/调用，Cursor会话附加及公开GitHub的Skills CLI安装曾通过。其他客户端、Windows/Linux未验证；本轮没有重跑全部客户端安装。

## 维护规则

- 接入时按项目实际版本读取具体组件源码、registry依赖闭包、样式与许可，不只检查入口HTTP200。
- 报告写明直接复用、源码适配或方法参考；先确认真实数据状态，再确认动画。
- 新实现实测后才提升支持状态，并保留版本、环境与具体组件范围。付费与素材授权不随免费仓库声明外推。
- 一个入口失败可查官方源码或版本文档；仍无证据则标明缺口、使用已有方案，不猜API。
- 发布前检查引用、技能结构、行为案例和实际页面；不把独立推演当实际运行，也不把一张截图当全面验收。

## 暂不加入默认来源的候选

AI Elements、Prompt Kit 保留历史候选记录；本轮未重新研究，不用过期迁移公告或访问失败推断当前状态。若未来任务需要，先读取当时官方入口再决定。
