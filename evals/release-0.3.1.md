# 0.3.1 发布前验收

日期：2026-11-05。结论：可作为带明确支持范围的公开技能包发布；本轮未发现本地发布阻断问题。尚未建立/发布公开仓库，远程安装需要发布后完成。

## 修正

- 明确 Animate UI 官方仓库使用 MIT + Commons Clause，并补上其 LICENSE 入口，避免误认为纯 MIT。
- 更新三项真实组件接入证据，保留所有未测范围；修正 README 排版。
- 技能只包含原创指令、文档和评测资料，不打包第三方组件源码、用户工程、配置或凭证。

## 官方来源复核

独立只读审阅了 11 个来源参考及其官方站点/仓库。普通参考链接可达，安装描述未发现矛盾；需重定向的 HeroUI 文档正常跳转。

| 来源 | 本轮结果 |
| --- | --- |
| BoardUI | 官方 skill 命令和客户端目录描述一致；Pro 与免费分开 |
| Bencho | 目录及详情可达；安装入口/源码许可仍未确认，保留仅参考状态 |
| HeroUI | React 19/Tailwind 4 文档一致；当前仓库 Apache-2.0，未错误声称 MIT |
| beUI Pro | agents 文档一致；端点普通 GET 为 401，不访问凭证，不计为授权接入通过 |
| Libraries.dev | skill、Beam 文档和命名导出/active 描述一致 |
| Watermelon | MCP 文档一致；端点 GET 为 405，不计为 MCP 工具发现成功 |
| React Bits | 变体命名与当前安装描述一致；MIT + Commons Clause |
| Animate UI | 包、逐项 dry-run、React/Vue 路线一致；补齐许可提示 |
| Kibo UI | shadcn/CSS Variables 条件与 CLI 描述一致；仓库标记 MIT |
| Magic UI | 安装入口与免费仓库 MIT 标记一致；Pro 分开 |
| Aceternity UI | registry JSON 及命名空间配置描述一致；Bento Grid JSON 可获取 |

这些是技术文档与入口检查，不是第三方许可的法律意见。各参考包含直接官方链接，使用时仍按目标版本核实。

## 完整行为场景复核

使用独立会话读取当前技能与按需参考，给出 cases.json 全部 11 条 prompt，未提供 expected 或历史报告。评测者未联网、改文件、安装或访问凭证，回答后由主审按 expected 核对：

| 场景 | 决策回答结果 |
| --- | --- |
| existing-ui | 通过：复用现有 shadcn、分组与区分按钮主次 |
| nextjs | 通过：指定来源、真实状态、最小 client 边界与减少动态效果检查 |
| vue-bencho | 通过：源码/许可未明时只作参考，不引入 React |
| paid | 通过：不获取受限源码或购买，提供替代 |
| offline | 通过：不猜 API，复用本地状态反馈 |
| review-only | 通过：只读建议，区分推断与页面观察 |
| unrelated | 通过：纯错别字不适用技能 |
| animate-scope | 通过：单项 dry-run，不全量转换 |
| same-name | 通过：不混用不同 Border Beam 实现的属性 |
| business-routing | 通过：Kibo 甘特图与业务数据约束，避免装饰特效 |
| reactbits-license | 通过：不把第三方许可改写为本技能 MIT |

这是同一独立上下文中的文字决策评测，不是 11 次独立运行、宿主自动触发率测试或真实项目执行证明。

## 实际组件集成

在隔离临时 React 工程运行官方安装命令（未修改用户业务项目）：

```sh
npx shadcn@latest add @magicui/number-ticker https://ui.aceternity.com/registry/bento-grid.json @react-bits/CountUp-TS-CSS --yes
npm run build
```

环境：Node 22.22.3、React/React DOM 19.3.1、Tailwind 4.3.4、Motion 12.43.0、TypeScript 7.0.2、Vite 8.3.6。组件代码未经修改，应用层提供数据状态、主题与减少动态效果降级。

- CLI 成功添加 Number Ticker、Bento Grid、CountUp 三个组件。
- `tsc --noEmit` 与生产构建通过。初始测试工程使用旧 TS baseUrl 配置且缺少 vite/client 类型，修正测试工程后通过；不属于技能或上游组件错误。
- Vite 对 use client 指令产生构建警告；该测试是纯客户端 Vite 页面，未证明 Next.js SSR/hydration 正确。
- Playwright 实际检查：数字 42→43、键盘更新到 44、CountUp 最终 120、1280px 三列/390px 单列、窄屏无横向溢出、键盘焦点、主题切换后背景与文字颜色、减少动态效果实时切换为静态数值。
- 第一轮测试发现应用样例所用 Motion hook 没有响应运行中的媒体偏好切换；改为原生 matchMedia change 监听后复测通过。没有修改第三方组件以掩盖问题。
- 检查了浅色桌面、深色桌面、深色窄屏截图；主题截图等待过渡结束后检查颜色。浏览器本轮无 pageerror，日志仅开发工具信息，无资源错误。

验证仅覆盖这三个组件的所列场景，另有此前 border-beam@1.4.1 示例证据。Animate UI 转换、Kibo 真实业务数据、其余来源组件、付费服务和 MCP 协议未在本轮执行。

## 客户端与发布边界

- Codex 新进程发现当前描述并返回 11 个来源；Claude Code 显式调用读取选型表并返回 11 个来源。
- Claude 首次问题要求推断“新增五项”，缺少历史时猜错来源；该次不计成功。改为核实当前全部来源后通过。不能据此保证模型不会推断错误。
- 客户端有既有配置/MCP 警告，但未阻止技能发现；未修改无关配置。它们不是本技能所需依赖。
- 用户同机安装采用共享源目录；公开 ZIP 不含此类软链接。
- 发布前 quick_validate、全部本地引用、版本/案例唯一性、无本机路径/内部软链接、基础敏感模式检查均通过。ZIP 解压副本经 Skills CLI 为 Codex/Claude Code 干净安装成功，主指令、接口及全部参考资料与最终包逐字节一致。本报告仅在安装后补记结论。

未验证 Windows/Linux、其他客户端、全部上游组件、付费授权、真实模型服务、网络中断执行、自动触发统计、完整可访问性审计或美观程度提升。发布时沿用 README 的支持表，不宣传为“所有组件一键接入”。

公开仓库发布后，把 README/installation 中 OWNER/REPO 替换成真实地址，再在干净目录验证该远程地址可发现并安装此技能。当前模板已明确标记，不构成已发布声明。
