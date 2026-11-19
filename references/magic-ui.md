# Magic UI 3
读取日期：2026-11-19（Asia/Shanghai）

## 核验结论
- 当前目录列出 150+ 免费组件，主要面向落地页和产品展示；源码使用 shadcn registry 安装。[组件目录](https://magicui.design/docs/components) · [官网](https://magicui.design/)
- GitHub 当前仓库根含 registry.json 与 LICENSE.md；LICENSE 标注 MIT。[仓库](https://github.com/magicuidesign/magicui) · [LICENSE](https://github.com/magicuidesign/magicui/blob/main/LICENSE.md)
- 已由 GitHub 插件读取仓库根与 LICENSE。仓库路径存在 registry 与 monorepo apps；已取Number Ticker与Bento源码；其他条目仍按需读取。

## 代表条目与页面方法
1. **Bento Grid**：适合把产品功能组织为可扫读的卡片矩阵；每卡一个价值点，背景演示只做局部、低强度 hover。文档示例以 `BentoCard` 数据数组描述 Icon、标题、说明、链接、背景与响应式 grid span。[文档](https://magicui.design/docs/components/bento-grid)。背景演示自身依赖应逐项核对。
2. **Animated List**：适合通知/事件样例或小型活动流，按序延迟出现；不用于真实事件流或不断增加的生产列表，以免阅读延迟和重播。[文档](https://magicui.design/docs/components/animated-list)。默认 delay 为 1000ms；检查一次性展示和容器高度。
3. **Text Animate**：适合标题逐词模糊/淡入；官方 API 支持 animation、by、delay、duration、startOnView、once、accessible。[文档](https://magicui.design/docs/components/text-animate)。对读屏使用 accessible 文本输出；正文和导航标签避免拆字动画。
4. **Number Ticker**：适合已定值的 KPI 展示，进入视口时强调数值；使用静态结果保障无动画或脚本失败场景。技能记录指出当前源码没有自行处理减少动态效果偏好，需要由产品层提供静态模式并检查偏好变化。入口：[目录](https://magicui.design/docs/components)（定位 Number Ticker）。
5. **Border Beam**：适合一个强调卡片的边缘光带；限制于选中 CTA/升级卡片，避免多卡无限循环造成视觉噪声。入口：[目录](https://magicui.design/docs/components)（定位 Border Beam）。与 Libraries.dev 同名包不是同实现，不要混用属性或 registry id。

## 可复用的设计方法
- 先定页面阅读顺序和网格，再选择一项动效；背景装饰不可争夺标题和 CTA 的对比度。
- Bento 内容来自语义数据结构，各卡共享布局、各自只改变展示；演示组件放背景插槽并检查窄屏 span。
- 动画按视口触发并使用 once；为重复进入、禁用动态效果及 hydration 提供静态结果。
- 组件是复制进项目的源文件，不是黑盒包；对导入 alias、Tailwind content/source 扫描与依赖声明逐项适配。

## 依赖、SSR、动态偏好与性能
- 文档安装走 `shadcn add @magicui/<component>`，源文件按项目 aliases 导入；组件依赖不是统一的，需读 registry item 的 files/dependencies。
- React + Motion 的客户端动画在 SSR 页面要放在最小 client boundary；数字、文本初始输出必须服务端/客户端一致，避免 hydration mismatch。
- 减少动态效果：Number Ticker 已知没有内置降级；需自行静态呈现。其他条目也不能假定自动支持，按源码检查 prefers-reduced-motion。
- Beam、粒子、globe、video 背景有持续帧成本；评估 GPU、移动端滚动、触屏缺少 hover、低端机功耗。
- MIT 适用范围以具体仓库版本为准；Pro 区块不视作免费开源组件，单独核实素材/字体许可。

## 读取证据与缺口
- 文件：技能 references/magic-ui.md；GitHub 插件读取 magicuidesign/magicui 根与 LICENSE；官网组件目录、Bento Grid、Animated List、Text Animate。
- 本轮已读取 Number Ticker 与 Bento Grid 源码，因此 Number Ticker 无内建 reduced-motion 降级是源码观察；Border Beam 源码仍未读取。没有运行registry CLI或性能基准；Bento适配的构建与浏览器检查见末节。
- 接线时再核对官方 registry JSON 与实际复制的源文件版本，确认每条依赖和许可，不能以示例应用替代业务验收。

## 已读取的代表源码（补充）
- GitHub 插件读取了当前 [Number Ticker 源码](https://github.com/magicuidesign/magicui/blob/main/apps/www/registry/magicui/number-ticker.tsx) 和 [Bento Grid 源码](https://github.com/magicuidesign/magicui/blob/main/apps/www/registry/magicui/bento-grid.tsx)。
- Number Ticker 文件含 `use client`，依赖 React、`motion/react` 的 `useInView`/`useMotionValue`/`useSpring` 与项目 `cn` helper；仅在视口出现后用定时器设置运动值，组件清理时清除 timer。源码没有读 `prefers-reduced-motion`，确认静态降级需在接入层实现。小组件的触发由 `useInView(... once: true)`，不是实时数据更新机制。
- Bento Grid 由 `BentoGrid` 与 `BentoCard` 导出；卡片接受标题、说明、Icon、链接、CTA、背景节点和 className 等配置，点击/hover 提示采用 CSS transitions。读取到的实现依赖项目 Button、cn 和 Radix ArrowRightIcon（本次读取版本）（应由 registry item 和导入项逐项确定是否需安装）。本次未核对完整 Bento registry JSON，不把其依赖列表写作已确认。
- 本次读取到 GitHub 插件返回的实际源码正文；没有运行shadcn CLI；Bento适配的构建与浏览器检查见末节。Border Beam 仍未读取。

## 0.8.0 实际Bento适配验收

临时React/Tailwind页面复制并适配已读BentoGrid/BentoCard：保留卡片数据/背景槽组合，复用本地按钮/图标实现，不引第二套控件；统一主题，手机单列，CTA直接可见而非只靠hover。类型检查与生产构建通过，桌面/320px实际渲染和CTA路由检查见本版验收记录。此结果仅覆盖该源码适配，不代表整个MagicUI库已验收。
