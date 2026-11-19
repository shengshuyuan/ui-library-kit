# Dissolve：状态连续性、触感组件与粒子效果

核实日期：2026-11-19。官方入口：https://dissolve.rselmi.com/ 。官方源码：https://github.com/rs-4/dissolve 。已读 README、LICENSE、`registry/index.json`、主包元数据，以及 Web 的 skeleton、button、inputs、core 实现。MIT，复制或适配上游代码保留版权与许可。不把第三方源码打包进本技能。

## 先分清两条复用路径

- **网页**：优先读取官方 `web/cp/` 下 DOM/Canvas 实现及对应 CSS，再按项目框架适配；`web/engine/` 是效果引擎，`web/cp/kit.js` 是共享工具。官网能在浏览器演示，不等于 npm 主包可用于 React DOM。源码可复用，但尚无本技能对 Web 适配的构建/渲染验收。
- **React Native / Expo**：`react-native-dissolve` 为原生引擎；本次包元数据版本 0.0.5，组件目录为 29 项。CLI 把源码复制到 `components/dissolve`，不是从 npm 主包直接导入所有组件。组件需要 NativeWind v5、react-native-css、Tailwind v4、tailwind-merge；引擎需要 Skia、Reanimated、worklets、gesture-handler 等，必须核对现有项目版本。网页优化不安装这套原生依赖。

原生路径仅在项目本来就是原生应用时使用：当前官方入口为 `npx react-native-dissolve list`、`init`、`add <name>`；已有配置不重跑初始化、不覆盖定制。项目的包管理器、版本和 CLI 当前帮助优先，不照搬官网升级整套运行时。

## 按页面问题选择组件

下列名称来自官方 registry；Web 路径为对应实现的检索入口。部分模块只完成目录定位，深读范围见后文。不要把所有交互一次塞进页面。

| 问题 / 使用位置 | 组件名 | Web 检索入口 | 应吸收的能力与边界 |
| --- | --- | --- | --- |
| 保存、订阅等动作反馈跳变 | morph-button、split-button | `web/cp/core.js` | 同一控制承接待处理/结果；危险确认保持明确文字和焦点 |
| 主要 CTA 缺少按压反馈 | glossy-button | `web/cp/button.js` + button.css | 细边缘光、内层次、按下缩放；只强化主操作，不全站玻璃化 |
| 内容加载前后跳动 | skeleton | `web/cp/skeleton.js` + skeleton.css | 占位与真实布局同位置；按阅读顺序交接，低动态模式短淡入 |
| AI 等待不知当前步骤 | thinking-orb、image-reveal | `web/cp/ai.js` | 状态映射到真实生成阶段；图像渐进揭示不伪装真实进度 |
| 上传、任务完成无局部反馈 | upload-tile、task-check | `web/cp/ai.js` | 上传进度、失败、取消；勾选后保持可撤销与布局连续 |
| 删除导致列表断裂 | swipe-to-delete、toast | `web/cp/core.js` | 退出后收拢列表、Undo；网页保留可见删除入口与键盘操作 |
| 点赞与反应缺少触感 | like-button、reaction-picker | `web/cp/core.js`、messages.js | 状态切换的短反馈；回滚错误，长按之外提供点击入口 |
| 验证码、确认动作难理解 | otp-input、slide-to-confirm | `web/cp/inputs.js` | 校验/提交状态连续；支持粘贴与键盘，滑动不是支付授权本身 |
| 数字变化抖动、步进难用 | number-ticker、quantity-stepper、ruler-picker | `web/cp/inputs.js`、controls.js | 稳定数位、范围与禁用状态；提供直接输入与读屏数值 |
| 图片或卡片展示缺少操作关系 | before-after、card-stack | `web/cp/surfaces.js` | 对比拖杆与卡片聚焦；拖动之外提供按钮/键盘方案 |
| 播放与展开脱节 | voice-note、mini-player | `web/cp/messages.js`、controls.js | 播放/暂停/进度、展开收回同一对象；替换模拟音轨 |
| 列表刷新与排序缺少位置线索 | pull-to-refresh、reorder-list | `web/cp/pull.js`、reorder.js | 跟手与让位；网页保留刷新按钮、键盘移动、滚动可用 |
| 多选项切换焦点不清 | segmented-control、gooey-menu | `web/cp/segmented.js`、core.js | 选中位置连续；保留 tab/menu 语义、键盘与溢出处理 |
| 数据、签名或揭示交互 | bar-chart、signature-pad、scratch-card | `web/cp/controls.js`、messages.js、scratch.js | 数据探查、撤销与导出、揭示阈值；提供可访问的等效操作 |
| 章节或内容状态切换 | sweep-transition | `web/cp/ai.js` | 旧/新状态有方向与遮罩关系；不阻塞路由和首屏阅读 |

深读与适配要点：

1. **Skeleton**：源码用块形状描述占位并在加载结束后接上内容；还调用 snapshot 与 morph，不能只拷一段组件声明。网页先复用项目现有 Skeleton，确有视觉需求才移植粒子交接。骨架尺寸、结束后布局不跳、快速请求不闪、错误状态不悬空比粒子更重要。
2. **Morph / Split / Toast**：core.js 有共享状态与挂载逻辑，不是独立 React 导出。拆出状态转换与样式，移除演示自动播放，把保存、撤销、取消接到真实异步结果；动画中重复点击不得重复提交。
3. **GlossyButton**：源码把边缘光、内阴影、外环与按下缩放分层，适合主 CTA 的局部材质。接入时使用真实 `<button>` 或已有 Button，补齐 focus-visible、disabled、loading 和低动态效果；不要复制演示调色器、默认品牌颜色。
4. **Slide / OTP / Ticker**：inputs.js 的 Web 展示包含模拟支付等待与固定标签。必须删除模拟计时，连接真实确认/校验/数值；组件名相同不代表 Web 与原生 props 相同，不把官网 JSX 文档当作 DOM API。

## 效果词汇：按语义使用

官方目录分为退出 dust、sand、ember、pixel、vortex、shatter、stream；进入 gather、rain、print；手势 snap、fling、charge；特殊 morph；轻松表达 peel、melt。共 16 个目录变体，另有 scrub/cascade/screen 等组合使用方式，勿混计组件数量。

- 移除、归档、送往目标：退出/stream；恢复、新内容：进入；状态间接续：morph；跟手：scrub/fling/peel。
- 默认优先短促的 opacity/transform 或项目现有动画。高频后台流程、长列表和低性能设备不默认粒子化；不要把空闲页面变成持续播放的展示墙。
- 粒子动画不能删除唯一 DOM 信息。保留语义与焦点，装饰层不接管指针；低动态效果直接完成状态更新。

## 移植与验收清单

先读实际组件的 import 和 CSS 依赖，只引入必要部分。核对 kit.js 的生命周期作用域、事件监听、帧循环与取消能力；组件卸载必须清理 Canvas、RAF、监听、计时器和待完成任务。多实例不能用互相冲突的全局选择器或 ID。主题颜色转入现有 token。

Next.js 等 SSR 环境在客户端生命周期内访问 window、document、matchMedia 和 Canvas；若模块在导入时就访问浏览器对象，挂载后动态导入或改造模块；仅写 use client 不保证 SSR 安全。提供无动画首屏及 API 不支持的降级。快照失败、跨域图片无法采样、ResizeObserver 改变尺寸时保留原始内容，不出现空白；离屏或后台暂停持续工作。键盘/触屏、快速连点、异步失败、撤销、路由卸载、窄屏与深浅色均按实际采用的组件验证。

**证据级别**：官方目录/许可与上述四组源码已核实；模块位置不等于移植通过，未做 React DOM/原生安装或浏览器交互验收。接入真实项目后按实际结果提升状态，不声称全站组件已安装。

## 核实入口

- [组件索引](https://github.com/rs-4/dissolve/blob/main/registry/index.json)
- [Web 组件模块](https://github.com/rs-4/dissolve/tree/main/web/cp)
- [Web 效果引擎](https://github.com/rs-4/dissolve/tree/main/web/engine)
- [原生包元数据](https://github.com/rs-4/dissolve/blob/main/packages/react-native-dissolve/package.json)
- [MIT 许可](https://github.com/rs-4/dissolve/blob/main/LICENSE)
