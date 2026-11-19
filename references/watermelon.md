# Watermelon UI — block-driven 组件方法与接入

核实日期：2026-11-19。已读官网机器索引、MCP 文档和只读 Catalog API。这里只读来源，没有连接其托管 MCP，没有安装条目，也没有构建验收。

## 目录、来源、许可

- [目录/API](https://ui.watermelon.sh/llms.txt)列出主页、Animated Components、Blocks、Dashboards、Templates、developer docs、OpenAPI 与只读 catalog endpoints；宣称无密钥读取公开目录。
- [当前 Catalog Summary](https://ui.watermelon.sh/api/catalog/summary)返回 849 entries：516 components、131 animated-components、189 blocks、12 dashboards、1 template。计数随目录变化，应每次现查。
- [MCP 文档](https://ui.watermelon.sh/developers/mcp)列出 `search`、`get_component`、`get_inspiration`、`compose_page`、`list_categories`；本轮未连接/调用，故仅记录公开文档声明。
- 条目 API 返回 `previewUrl`、`registryUrl`、`installCommand`、dependencies。平台仓库是 [watermelon-platform](https://github.com/WatermelonCorp/watermelon-platform)，条目 registry 是 [watermellon-registry](https://github.com/WatermelonCorp/watermellon-registry)。Registry repo 顶层 [LICENSE](https://github.com/WatermelonCorp/watermellon-registry/blob/main/LICENSE) 是 MIT（读取确认）。但它只证明该仓库标注的代码许可；不能推出外部图片或第三方资产同许可。 [AI_POLICY.md](https://github.com/WatermelonCorp/watermellon-registry/blob/main/AI_POLICY.md) 要求核实正确 API、安全性、项目规范与版权许可。auth-01 的内嵌源码已从同一MIT仓库取得，代码保留该许可；其它条目和外部素材仍需逐项核实。

## 从页面问题选真实区块

下列记录来自 [Blocks Catalog API](https://ui.watermelon.sh/api/catalog/entries?kind=blocks)，API 本次返回前 20 个条目。对 `auth-01` 已通过 Cindy GitHub 插件读取 registry 仓库中的 JSON 源文件（不是网页预览的猜测）；该 JSON 在仓库内的路径为 `public/r/auth-01.json`。其他条目保留目录/API 级证据，不冒充源码审阅。

| 页面问题与区块 | 组合与方法 | 实际入口 / 风险 |
|---|---|---|
| 需要公告但不能遮挡主任务：Announcement 1 | 把可关闭或有时效的提示作为页面顶层轻区块，链接动作引到变更详情。 | [预览](https://ui.watermelon.sh/block/announcement-1) · [registry JSON](https://registry.watermelon.sh/r/announcement-1.json) · `npx shadcn@latest add https://registry.watermelon.sh/r/announcement-1.json`。Catalog 标注 react-icons、clsx、tailwind-merge。要核对 dismiss 持久化、公告语义和依赖实际版本。 |
| 登录页需要邮箱/密码、OAuth 入口和邀请入口：auth-01 | `Auth1` 是单卡登录展示区：受控邮箱/密码输入、显示密码切换、提交、三个 OAuth provider slots、底部 invite callback。品牌名/描述、badge、forgot-password props 虽在接口定义，但当前渲染实现未使用；设计稿接入时先核对 props 真正接线。 | [源码文件](https://github.com/WatermelonCorp/watermellon-registry/blob/main/public/r/auth-01.json)（读取了完整 registry JSON）。条目文件目标 `components/ui/auth-01.tsx`；依赖 `lucide-react`、`react-icons` 和 registryDependencies `button/card/input/separator`。源码提交只调用 `onSubmit(email,password)`，没有认证、服务端校验或错误反馈。输入主要靠 placeholder 而非可见 label，需补表单 label、错误与忙碌状态；OAuth callback 也由宿主接线。该源码包含 `"use client"` 与 `useState`，只能放在 client boundary。
| 登录入口需要另一种视觉节奏：auth-02 | 与 auth-01 作视觉比较，复用品牌与验证流程，不把示例表单逻辑当安全认证。 | [预览](https://ui.watermelon.sh/block/auth-02) · [registry JSON](https://registry.watermelon.sh/r/auth-02.json)。先读当前项目表单体系，检查标签、错误反馈、密码管理器与 focus 顺序。 |
| 公告需要与产品介绍并排：Announcement 2 | 用较小通知区块连接主内容 section；不把短期促销混进长期导航。 | [预览](https://ui.watermelon.sh/block/announcement-2) · [registry JSON](https://registry.watermelon.sh/r/announcement-2.json)。来源 catalog 同样列 react-icons/clsx/tailwind-merge；需查看 registry 源码和具体许可后再接入。 |
| 有临时高优先级通知：Announcement 3 | 在页面内设置有限上下文的 notice，明确关闭与重新查看路径；若必须行动，按产品规则使用独立 dialog 而非误用普通公告。 | [预览](https://ui.watermelon.sh/block/announcement-3) · [registry JSON](https://registry.watermelon.sh/r/announcement-3.json)。关闭、持久状态与高对比度均须源码检查。 |

以上 5 个条目都由实时 Catalog API 证实；`auth-01` 已建立端到端目录 → registry JSON → TSX 源码路径，其他条目仍只有目录证据。获取单个 registry 源文件时，路径是仓库的 `public/r/auth-01.json`，不是预期的 `src/data/contents/...`。

## 访问缺口与建议验收

- 搜索目录、计数和组件条目 API 成功；网页抓取器不能访问 registry host。不过 GitHub 插件可读取镜像仓库完整源文件，已核实 `auth-01`。
- MCP 工具名和能力取自官方说明，没有当前 MCP 连接凭证/客户端回执，不能报告已用 MCP。
- 推荐把 Watermelon 用于“区块构图与成页启发”：先用 `compose_page` 产生 section 组合建议，再逐条 `get_component` 核对文件、依赖与入口，最后在本地项目验证响应式、主题和辅助技术。
- GitHub registry 仓库顶层 LICENSE 为 MIT；针对 auth-01 的 JSON 与内嵌代码由该仓库分发，仍需尊重 MIT 声明。条目作者署名未见于该 JSON，仓库许可证不能覆盖 react-icons / lucide-react 等依赖。登录条目只负责展示：密码、会话、验证与错误状态必须走应用现有安全边界。
