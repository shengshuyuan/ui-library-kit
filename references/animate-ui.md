# Animate UI
核实日期：2026-11-05。适合给已有 shadcn 组件补充有节制的交互动画；当前有 React 与 Vue 两条文档路线。

- React 安装：https://animate-ui.com/docs/installation
- 当前许可：https://github.com/imskyleen/animate-ui/blob/main/LICENSE.md
- 版本变化：https://animate-ui.com/docs/changelog
- Vue 入口：从上述文档导航选择 Vue，再读取其安装说明，不能照搬 React 包。

React 当前文档使用 `@animate-ui/react` 与 `motion`，复用项目已有 radix-ui、@base-ui/react 或 react-aria-components。v1 文档与当前 v2 的安装方式不同，先核对项目版本。

CLI 会根据 components.json 替换 primitive imports 并移除被 Motion 替代的 CSS 动画类，是对现有源码的修改。先查看单个目标的 dry-run：`npx @animate-ui/cli@latest animate accordion --dry-run`。只转换任务需要的组件，检查 diff 和原有定制，不默认运行 animate all。

React/Vue API 和依赖分别核实，遵循各自项目已有模式。当前官方仓库标记 MIT + Commons Clause，不按纯 MIT 处理；接入前查看当前 LICENSE 与目标条目。本技能 MIT 不覆盖其源码，不把组件源码收入技能分享包。

验证程度：当前安装文档和 changelog 已核实；未运行转换 CLI 或渲染组件。
