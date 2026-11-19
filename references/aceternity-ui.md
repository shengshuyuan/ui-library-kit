# Aceternity UI — visual section 组件方法与接入

核实日期：2026-11-19。核实官网 CLI / Next.js docs 与可读的官方 registry JSON；没有安装和运行示例，源码级验收不代表目标项目通过。

## 入口、许可和方法

- [CLI 文档](https://ui.aceternity.com/docs/cli)按 Components、Blocks、Pages、Interactions、Templates 分类；其菜单能映射 Hero、Bento、FAQ、Pricing、Nav、Footer、Dashboard cards、动画等页面需求。
- [Next.js 安装文档](https://ui.aceternity.com/docs/install-nextjs)说明 Next / Tailwind 前置设置。现有项目应只使用 CLI 加选定 registry entry，不能重跑 create-next-app。
- 官方示例通过 `npx shadcn@latest add https://ui.aceternity.com/registry/bento-grid.json` 接入。组件源通常复制进项目，因此是可以审查/改造的代码，而不是运行时服务。
- [Pricing](https://ui.aceternity.com/pricing) 当前将 100+ components 标为 Free，并把 premium blocks、templates、full pages 与 interactions 列入付费许可范围。逐项源码 JSON 同样未带 license 字段。官方 [Licence](https://ui.aceternity.com/licence) 明示是针对购买或下载 item 的 Aceternity License，允许将 item 用于终端产品并禁止重新分发源码；另 [Terms](https://ui.aceternity.com/terms) 写明除另有说明外站内 material 知识产权保留，禁止复制/再分发。公开免费组件与站内版权条款之间未看到明确的免费组件许可授权条款；本次三个 JSON 也没有逐项 license 字段。因此只记录为“免费可获取”，不能据此断言 MIT/开源或推广到商业再分发。下载/商用前需核对对应 item license 或向权利人确认；第三方组件继续服从各自许可。

## 页面问题 → 组件组合 → 源码入口 / 风险

| 页面问题 | 组合与设计方法 | 已读源码与风险 |
|---|---|---|
| 多项能力需要密度高但层级清楚：Bento Grid | `BentoGrid` 管网格，`BentoGridItem` 负责一项标题、描述、icon、header；默认小屏单列、md 起三列。让最重要项目用 grid span 拉开层级，控制卡片数量。 | [Registry JSON/source](https://ui.aceternity.com/registry/bento-grid.json)，依赖 `@tabler/icons-react`，内部依赖项目 `@/lib/utils` 的 `cn`。检查项目 alias 与 Tailwind token；窄屏阅读顺序与语义层级。 |
| 营销页首屏需要纵深与产品画廊：Hero Parallax | 标题/描述先给价值主张；下面最多 15 项产品拆三行，滚动位移方向交错，卡片链接到产品详情。 | [Registry JSON/source](https://ui.aceternity.com/registry/hero-parallax.json)，依赖 `motion`，有 `"use client"`，产品要提供 title/link/thumbnail。原源码固定 `h-[300vh]`、卡片 `w-[30rem]`，横向溢出/触屏/ reduced-motion 必须改造实测；Next SSR 应限制 client boundary。 |
| CTA 需显眼边界动画：Moving Border | 把动边框放在已存在的按钮/卡片外壳，子层继续承载真实 label 和 button 交互。 | [Registry JSON/source](https://ui.aceternity.com/registry/moving-border.json)，依赖 `motion` 和项目 `cn` utility；DOM/SVG 每帧按 path length 更新。检查动画暂停、focus ring、点击目标和 reduced-motion。 |
| 卡片信息很多，需提高可扫读性：从 CLI 的 Cards / Feature Sections / Stats Sections 选一组组合 | 先定信息优先级和每卡片 action，再确定列表/网格；不要把 hover 才出现的内容作为唯一信息。 | CLI 文档分类能确认条目目录；本次未找到并读取该类 registry source，作为导航层证据，不能认定已核实源码。 |
| FAQ 或价格对比需要营销页收束：从 FAQs / Pricing Sections / CTA Sections 组合 | 先用对比表回答决策，再用短 CTA；FAQ 用原生可访问 disclosure，避免无必要弹跳。 | [CLI 分类](https://ui.aceternity.com/docs/cli)列出这些组件类别；具体区块和 license 要先查 registry JSON 与该条款。 |

## 接线与风险检查

- Registry JSON 只读成功：Bento Grid、Hero Parallax、Moving Border；对三者记下的依赖来自实际 JSON。已查当前 Pricing、Licence、Terms：明确 free 与 premium 分类及 Pro License 限制，但对 free source 的授权范围仍不清楚，不能称其为 MIT/open-source。
- `@/lib/utils` 不是 CLI 可替代配置，需确认项目已有 `cn` 和 Tailwind utilities；若没有，按本项目模式补齐，不覆盖 `components.json` namespace。
- Hero Parallax 直接读取 thumbnail URL，必须使用允许的资产并提供 alt/空态；宽 30rem 固定卡片、300vh 滚动舞台对手机与键盘用户风险很高。
- 仅用视觉浏览器判断通过不够：核对 client bundle、构建、窄屏、键盘焦点、深浅主题和 reduced motion。当前没有任何这些环境验收记录。
- 页面变得更有动势不等于可用性提高。优先用于营销首屏和作品展示，不默认添加到数据密集型后台。
