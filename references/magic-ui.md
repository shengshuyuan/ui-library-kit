# Magic UI
核实日期：2026-11-05。适合产品展示页、Bento 布局、数字/文本动画与局部展示效果。

- 官网：https://magicui.design/
- 安装：https://magicui.design/docs/installation
- 源码与许可：https://github.com/magicuidesign/magicui

沿用 shadcn/ui 的源码安装方式。官方示例：`pnpm dlx shadcn@latest add @magicui/globe`。按项目包管理器和具体条目选择命令，不照着安装页在已有项目重新 init；源码导入路径依项目 aliases 确定。

公开仓库标注 MIT；Magic UI Pro 的区块/模板不在本技能范围内，仅使用许可明确的免费组件。本站也有 Border Beam，它与 Libraries.dev 的 border-beam 包不是同一实现，不混用属性或安装方式。

优先少量、必要的展示组件，核查性能、触屏、深浅主题与 reduced-motion。

Number Ticker 当前源码未自行处理减少动态效果偏好；项目层应提供静态数值替代，并检查偏好改变时的行为。

验证程度：Number Ticker 已通过官方 registry CLI 安装，完成 React 19 / Tailwind 4 示例的类型检查、构建、状态更新与浏览器验证；静态降级在示例应用层处理，不是上游组件内置保证。
