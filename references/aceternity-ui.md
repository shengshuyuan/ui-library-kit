# Aceternity UI
核实日期：2026-11-05。适合 Hero、卡片、滚动/视差和营销展示区块，免费条目与 All-Access/Pro 内容并存。

- 官网：https://ui.aceternity.com/
- CLI：https://ui.aceternity.com/docs/cli
- 框架安装说明：https://ui.aceternity.com/docs/install-nextjs

具体条目可用 shadcn CLI 添加 registry JSON，官方示例：`npx shadcn@latest add https://ui.aceternity.com/registry/bento-grid.json`。命名空间方式依赖相应 registry 配置，先查看当前 components.json，不覆盖现有配置。

框架教程包含 create-next-app 步骤，只适用于用户要求的新项目；优化现有项目不得照抄重新创建。逐项核实 Tailwind/Motion 版本、client 边界、依赖及许可。公开预览不等于有权取用所有区块与模板，不自动购买。

重视觉组件不默认进入数据密集后台；检查滚动性能、文字可读性、键盘与触屏替代行为。

验证程度：Bento Grid 已通过官方 registry CLI 安装，完成 React 19 / Tailwind 4 示例的类型检查、构建、三列/单列响应式与深浅主题验证；不代表所有区块、模板或 SSR 已验证。
