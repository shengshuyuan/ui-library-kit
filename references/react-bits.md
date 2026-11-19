# React Bits
读取日期：2026-11-19（Asia/Shanghai）

## 核验结论
- 官网现列 Text Animations、Animated Content、Aurora 等分类与条目。[目录](https://www.reactbits.dev/c/text-animations) · [Animated Content](https://www.reactbits.dev/animations/animated-content) · [Aurora](https://reactbits.dev/backgrounds/aurora)
- 当前安装页入口采用源码复制和 shadcn registry；仓库根含 jsrepo.config.ts，显示 registry 发布也在维护。[安装](https://www.reactbits.dev/get-started/installation) · [仓库](https://github.com/DavidHDev/react-bits)
- 当前仓库 LICENSE 明确为 MIT + Commons Clause，限制单独或打包售卖、再分发组件本身及工具；不能概括成 MIT。 [LICENSE](https://github.com/DavidHDev/react-bits/blob/main/LICENSE.md)
- 仓库目录及 LICENSE、README、package.json、jsrepo.config.ts 已由 GitHub 插件读取。代表源码行级内容未完整取证，不能据此宣称源码行为实测。

## 条目与适用方法
1. **Split Text / Text Animations**：适合首页标题逐词/逐字出现；把动效限于一句核心标题，副标题提供静态语义。入口：[分类目录](https://www.reactbits.dev/c/text-animations)；安装页支持 TS/JS、Tailwind/CSS 选择并从 registry 添加。实际 props、依赖及 SSR 边界需在具体条目页和安装输出中核对。
2. **Count Up**：适合结果页、案例页中已有明确终值的数字强调；常驻数据表和后台指标优先静态显示，避免每个数都跳动。入口：[分类目录](https://www.reactbits.dev/c/text-animations)。不要把动画数字当实时数据机制。
3. **Animated Content**：适合滚动进入视口时揭示分组内容，可选择位移、模糊、3D、wipe、stagger、scrub 等表达；建议按一个内容区块整体揭示，避免逐元素重播造成阅读打断。入口：[文档](https://www.reactbits.dev/animations/animated-content)。文档 props 含 scroll container、threshold、once 等；需要核对客户端观察器与卸载行为。
4. **Aurora**：适合品牌落地页头部作为低对比背景，配静态前景文字；提供暂停、时间驱动、鼠标交互、DPR 上限等控制。入口：[文档](https://reactbits.dev/backgrounds/aurora)。不应用于信息密集后台；多个 WebGL/canvas 背景或高 DPR 会增加 GPU 与耗电成本。

## 可复用的设计方法
- 先选“信息揭示”还是“装饰背景”，每个视口仅一处高注意力动画。
- 用静态文字与结构先保证内容完整；动效只改变进入和强调。
- 滚动动画用 once、阈值、容器边界控制触发面；背景允许暂停并降低 DPR。
- 每条具体组件单独核查 registry 的 CSS/TS variant，不能照抄另一个组件的 CLI 名称。

## 依赖、SSR、无障碍和性能风险
- registry 源码拷入本地，依赖按组件区分；CountUp 的 TS/Tailwind variant 使用 Motion，其他条目仍需逐条查 imports。
- 动画常依赖浏览器滚动、指针、canvas、窗口尺寸等能力。服务端渲染需要把客户端组件边界放在最小的动画叶节点，并检查 hydration 是否使用随机/时间初值。
- 已读取页面未证明所有条目遵守 prefers-reduced-motion。验收时测试系统减少动态效果；为持续动画提供 paused/静态替代，避免只调慢速度。
- 控制器需考虑键盘、触屏、对比度和文本可读性。性能关注持续帧循环、监听器清理、DPR、粒子数量及移动设备。
- LICENSE 含 Commons Clause；可将许可源码用作应用组成部分，但不可销售或再分发组件本身/近似托管复制品。核对具体文件是否含额外资产许可。

## 读取证据与缺口
- 文件：该技能 references/react-bits.md；GitHub 读取仓库根、src 目录、LICENSE；官网目录、安装页、Animated Content、Aurora。
- 已直接看到源码树含 src/components、src/content、src/docs 与 registry 配置，并读取 CountUp 源码/registry；这不是其他条目的源码检查或构建验收。
- 下一轮针对计划接入的其他条目取官方 registry JSON 与源文件，再做实际构建、键盘/触屏与 reduced-motion 验收。

## 已读取的代表源码（补充）
- 读取 [CountUp TS 默认源码](https://github.com/DavidHDev/react-bits/blob/main/src/ts-default/TextAnimations/CountUp/CountUp.tsx) 与 [CountUp TS/Tailwind registry JSON](https://github.com/DavidHDev/react-bits/blob/main/public/r/CountUp-TS-TW.json)。源码开头含 `use client`，依赖 `motion/react` 与 React hooks；通过 `useInView(... once: true)` 控制视口触发，使用 Motion `animate`，并在 effect cleanup 调 `controls.stop()`。
- 该源码在启动动画时读取 `prefers-reduced-motion`，匹配时立即完成计数；未见订阅偏好变化事件，所以运行中偏好改变不保证立即切换。registry 标注 dependency `motion@^12.23.12`，registryDependencies 为空。必须根据安装变体选择依赖；该事实仅对应此 TS/Tailwind 变体。
- 实际读取为 GitHub 插件 `get_contents` 返回的文件内容，未安装 registry、未构建或浏览器验收。证据仅覆盖CountUp，不代表其他条目也核实过。
