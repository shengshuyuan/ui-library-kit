# Kibo UI
核实日期：2026-11-05。适合复杂业务组件，如甘特图、颜色选择、上传区域、代码展示等；优先用于已有 shadcn 的项目。

- 官网与组件目录：https://www.kibo-ui.com/
- 安装：https://www.kibo-ui.com/docs/setup
- 介绍：https://www.kibo-ui.com/docs

官方安装页说明依赖 React 和已初始化的 shadcn/ui，主题要求 CSS Variables 模式。示例为 `npx kibo-ui add gantt`；也可按具体条目使用 shadcn CLI。CLI 会添加源码与相关依赖，实际输出目录以项目 components 配置为准。

没有 shadcn 时先比较已有组件能否实现需求，不为了一个控件自动初始化整套设计系统。甘特图等组件还需核对日期、时区、键盘和窄屏行为；示例画面不能证明真实业务数据适配。

官网声明免费开源；安装前通过官网仓库入口核对当前 LICENSE 与条目依赖，不收录第三方源码到技能包。

验证程度：官方安装说明及组件目录已核实；未接入真实业务组件。
