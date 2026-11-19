# Animate UI
读取日期：2026-11-19（Asia/Shanghai）

## 核验结论
- 当前站点区分 React、Vue 路线；React 文档依赖 `@animate-ui/react` 和 `motion`。当前官网给出 Primitive 与 Components 导航。[React docs](https://animate-ui.com/docs/react) · [目录](https://animate-ui.com/docs/primitives)
- LICENSE 是 MIT + Commons Clause，商业使用可行，但不能单独售卖/再分发组件本身。[LICENSE](https://github.com/imskyleen/animate-ui/blob/main/LICENSE.md)
- GitHub 当前默认分支仓库根和 LICENSE 已由 GitHub 插件读取。仓库是多包 monorepo；源码具体路径与各 primitive 依赖尚未逐个取得。

## 代表条目
1. **Accordion（Base UI）**：适合 FAQ、设置区和分组表单折叠。保留 Base UI 交互 API，只给 Panel 加动效；检查默认打开时页面加载动画是否符合预期。文档：[Accordion](https://animate-ui.com/docs/primitives/base-ui/accordion)。页面写明 reduced motion 下高度瞬间变化，但 fade 与 blur 仍播放。
2. **Dialog（Radix UI）**：适合编辑、确认等模态流程；保持触发、Portal、焦点等 Radix 结构，只动画 Overlay/Content。文档：[Dialog](https://animate-ui.com/docs/primitives/radix-ui/dialog)。文档写明 reduced motion 下淡入与去模糊保留，旋转、缩放、位移移除；注意退出动画期间焦点恢复和页面滚动锁。
3. **Navigation Menu（Radix UI）**：适合多层产品导航；菜单内容切换时平滑改变 viewport 尺寸，突出当前项。文档：[Navigation Menu](https://animate-ui.com/docs/primitives/radix-ui/navigation-menu)。 reduced motion 下尺寸即时改变、元素只淡入。
4. **CLI 转换流程**：不是单纯加一个包装器。官方 CLI 会读取 components.json，替换 primitive imports 并删除 Motion 接管的 CSS 动画类；支持的库含 Radix、Base UI、React Aria。先 dry-run 单组件、审查 diff，转换定制组件后检查剩余 class。文档：[CLI](https://animate-ui.com/docs/cli)。

## 可复用的设计方法
- 先保持底层无障碍 primitive 与交互状态模型，再给面板、遮罩、切换内容补动画；不自造焦点管理。
- 对关键打开/关闭路径写出状态时序：进入、保持交互、退出、卸载/隐藏；避免旧 CSS 与 Motion 两套动画叠加。
- reduced-motion 应改变运动幅度/位移策略，不只是整体变短；检查各 primitive 文档的具体退化行为。
- 全站择一体系（Radix、Base UI、React Aria），不要为动画引入第二套不一致 primitive。

## 依赖、SSR 和接线风险
- React 安装示例：`@animate-ui/react motion`；每个 primitive 还复用对应上游 primitive 及项目已有 shadcn 组件。不要把 React 包用于 Vue 文档。
- Motion 动效需要浏览器运行时。SSR 项目应确保被转换文件的 client/server 边界和依赖 import 合法；本次没有构建 Next 项目，因此不报 SSR 已通过。
- CLI 会原地改文件；定制类、导入别名和自定义 primitive 有覆盖风险。先查看 dry-run 与 git diff，只转换实际使用组件。
- 动画退出期间 DOM 仍挂载是交互和可访问性验收点；测试键盘打开、焦点返回、关闭后读屏不可见、reduced motion。
- LICENSE 有 Commons Clause；确认公司许可政策接受后再拷入商业应用。

## 读取证据与缺口
- 文件：技能 references/animate-ui.md；GitHub 插件读取 imskyleen/animate-ui 根和 LICENSE；官网 Primitive、Accordion、Dialog、Navigation Menu、CLI 与 React 安装页。
- 官网文档能核对当前 API 和声明的动效行为；已额外读取 v1 Accordion 源码，但未运行 CLI，也未检查当前 v2 registry 输出、bundle 体积或 SSR 构建。
- 若计划接线，先以 dry-run 生成一个具体组件的 diff，再根据当前项目的 primitive 来源与定制审查。

## 已读取的代表源码（补充）
- GitHub 插件读取了当前仓库路径 [Base Accordion primitive](https://github.com/imskyleen/animate-ui/blob/main/apps/v1/apps/www/registry/primitives/base/accordion/index.tsx) 和 [Accordion component wrapper](https://github.com/imskyleen/animate-ui/blob/main/apps/v1/apps/www/registry/components/base/accordion/index.tsx)。两条源文件均在 `apps/v1` 路径，作为实际实现样例；当前站点文档为 v2，不能将 v1 源码的完整细节直接推定为当前发布版本实现。
- primitive 文件含 `use client`，导入 `@base-ui-components/react/accordion`、`motion/react` 的 `AnimatePresence`/`motion`、项目内 context 与受控状态 hook；Panel 使用高度/透明度/y 位移动画，并以 `keepRendered` 分支控制关闭面板保留和退出。wrapper 依赖 lucide-react 和项目 cn helper，映射到该 primitive 并保留 Base UI prop 形态。
- 在已读 v1 primitive 文件中未找到 `prefers-reduced-motion` 分支；当前站点 v2 文档明确声明 Accordion reduced-motion 下高度即时变化但 fade/blur 仍播放。两者是不同证据层：当前体验以当前文档为准，生产接入仍应核对当前 v2 registry 输出源码。
- 本次 GitHub `get_contents` 返回了具体文件正文；未运行安装、CLI、构建或渲染。上述文件路径为 `apps/v1`，不声称其等同 v2 npm 包的源码快照。
