# Bencho：可复用的免费 React 组件

核实日期：2026-11-19。按真实页面问题查下表；只加载所选条目的源码、样式与 README，不全量安装。

## 已核实证据与边界

- 官网 https://bencho.dev/ 首页结构化目录181项，不等于181项均免费。
- 主页指向作者官方仓库 https://github.com/cabralorenzo/bencho ，本次通过GitHub插件读取 main 的README、LICENSE、package.json、THIRD_PARTY.md及blocks目录。
- README写48个免费React组件和24个Web Audio合成音效；实际blocks目录48个。仓库metadata描述仍写41，以目录和README为准，不保证永久数量。
- LICENSE为MIT，须保留版权许可；Bencho品牌名称、山羊标记及官网照片不包含。THIRD_PARTY列cuelume声音及依赖各自许可。免费仓库不等于Pro组件许可。
- 详情页 https://bencho.dev/blocks/upload-dropzone 明确free with full source；https://bencho.dev/blocks/jelly-tabs 、https://bencho.dev/blocks/form-error 明确included with Bencho Pro。
- 官网MCP页面称可把真实组件贴入项目；仓库README明确MCP属于Pro，不需要为免费组件安装MCP或购买Pro。

## 接入方式（官方文档已核实，未运行 registry CLI）

- README给shadcn registry https://bencho.dev/r/registry.json ，单项命令：`npx shadcn@latest add https://bencho.dev/r/like.json`。upload-dropzone、one-time-code、toast-stack、context-meter、image-compare各README均给同名/r/<slug>.json命令。
- 手动复制blocks/<slug>的TSX与CSS，读取该条目README所列依赖，映射tokens到现有主题。不要安装整个仓库的所有依赖。
- 注意两种形态不同：仓库README说明registry会加--bencho-前缀且dark跟随.dark；手动源码tokens.css实际为--bg、--card、--ink等通用变量、data-theme=dark和系统偏好，不能全局原样导入覆盖项目。应局部作用域或映射后核对。
- package.json是private的bencho-free示例工程，不是核实可npm install bencho的证据。展示工程React19.2.8、framer-motion13.1.1等不是所有条目的最低版本要求。

## 已逐条读取README的代表条目

官方源码入口均为 https://github.com/cabralorenzo/bencho/tree/main/blocks/<slug>。

| slug / 导出 | 实际用途 | 依赖与参数 | 生产接入缺口 |
| --- | --- | --- | --- |
| upload-dropzone / Upload | 文件进入目标、吸入进度、拒绝后弹回 | framer-motion + lucide-react；speed=50、bounce=40(0–100)、corner=20(0–24px) | 已读Upload.tsx：NO REAL FILES，内置假文件与5MB固定限额，没有真实桌面文件读取或网络发送。必须接真实File/校验/上传进度，不能把speed调节当真实上传功能 |
| one-time-code / Otp | 单一输入语义+多个视觉格，粘贴/自动填充/退格 | lucide-react；answer=Accept或Reject、length="4"或"6"、corner=12 | 已读Otp.tsx：answer是演示结果开关，不是验证码验证。必须连接认证服务，不能客户端固定Accept |
| toast-stack / ToastStack | 压缩消息堆栈，展开后显示消息 | framer-motion + lucide-react；peek=14(6–24px)、gap=10(4–20px)、corner=20 | README展示hover展开，需补focus/触屏入口、真实消息队列与可访问播报 |
| context-meter / ContextMeter | AI上下文按分类聚合为堆叠条或点阵 | framer-motion；style=Bar或Field、colour=Colour或Mono | 必须连接实际token统计与compact行为，不复制演示数字/声音作为真实处理 |
| image-compare / Compare | 前后图片拖动比较、弹性分割线 | 无React以外依赖；labels=true、corner=20 | 已读Compare.tsx：内部W=270/H=360、图片路径常量/blocks/xray-2.jpg与yak.jpg（官网照片不许可）；替换真实且获授权图片、开放响应式尺寸和来源props，补键盘控制 |

## 免费库用途索引（已确认目录，未逐项读取源码/API）

- 状态反馈与操作：like、emoji-reactions、checklist、todo-tower、step-player、generate、slide-confirm、pull、toast-stack、dynamic-island。
- 表单/选择/导航：label-input、one-time-code、picker、magnet-select、liq-toggle、liq-create、seek、dock、aspect、rolling-counter。
- AI/数据/工作流：context-meter、stacked-bars、action-node、huddle、asset-swap。
- 内容展示：image-compare、image-accordion、card-stack、card-swap、carousel、tilt。
- 媒体与指针输入：upload-dropzone、signature、voice-note、sound、time-scrubber、wake-slider、slosh。
- 品牌/实验性表达：ascii-wake、particles、heat-word、glass-bubble、eye-tracker、folding-bill、slingshot、drag-ball、palette、escape。最后一类仅有明确表达目标时使用，escape不得替代重要操作按钮。

## 选择规则

- 在保留现有效果模式，优先提取单个状态反馈/展开/数字变化方法，用现有组件与动效实现，不擅自更换全套控件。
- 局部升级或改版中，按真实任务映射上述免费组件，并适配统一视觉token；不要把组件墙搬成页面。
- 区分官方组件（可取得的MIT源码/registry）、设计方法适配（用现有栈实现）、Pro仅视觉参考；来源报告分别标注。
- 原生CSS/Vue不引入React；参考交互原理，或优先项目原有兼容组件。
- 拖拽、hover、长动画、声音都需键盘/触屏/reduced-motion/静音降级。音效需用户手势且默认不主动打开；网页示例成功/进度必须连接真实业务。

来源研究覆盖文档、许可、目录与代表源码；0.8.0新增Like源码适配的实际构建和浏览器验收，见末节。未运行registry CLI，也不声称其他组件或生产业务即插即用。

## Registry endpoint 后续核实

已成功读取 upload-dropzone.json 与 like.json 原始JSON。二者type=registry:component，dependencies为framer-motion@^13.1.1和lucide-react@^1.31.0，registryDependencies为https://bencho.dev/r/bencho-tokens.json。Upload含16328字符TSX与7375字符CSS，Like含6413字符TSX与4855字符CSS：确认真实源码可取，不只是文档声称。仍未执行CLI安装；Like手动源码适配已实测，见末节。直接复用registry时注意其精确依赖可能升级现有包，先对照锁文件与项目兼容性。

## 固定证据入口

- [免费组件仓库与接入说明](https://github.com/cabralorenzo/bencho)
- [MIT 与授权边界](https://github.com/cabralorenzo/bencho/blob/main/LICENSE)
- [第三方许可](https://github.com/cabralorenzo/bencho/blob/main/THIRD_PARTY.md)
- [免费组件目录](https://github.com/cabralorenzo/bencho/tree/main/blocks)
- [Upload 源码](https://github.com/cabralorenzo/bencho/tree/main/blocks/upload-dropzone)
- [OTP 源码](https://github.com/cabralorenzo/bencho/tree/main/blocks/one-time-code)
- [Compare 源码](https://github.com/cabralorenzo/bencho/tree/main/blocks/image-compare)
- [Like registry](https://bencho.dev/r/like.json)
- [Upload registry](https://bencho.dev/r/upload-dropzone.json)

## 接入顺序

先检查 registry 内 files、dependencies、registryDependencies 的完整闭包，计算锁文件中实际新增/升级的依赖与文件覆盖，比较单项 registry 安装、手动适配、已有实现三条路径；遵守 SKILL.md 的依赖与改动范围边界后，再用当前包管理器执行选定路径；检查 diff，保留项目的现有组件和锁文件。若精确依赖导致框架或动效库升级，先选手动适配已有版本的路径，不通过 legacy-peer-deps 或强制覆盖掩盖冲突。接入后验证真实数据的成功、错误、取消、重复操作、长文本、窄屏与路由卸载。

不把官方 MCP、额外 Skill 或 Pro 购买作为免费组件使用前置。免费库里的声音也不是默认启用项。

真实上传需要现有接口或用户明确授权的后端工作；没有接口时说明缺口，不把视觉技能扩大成部署存储服务。

## Like 的真实接线与本轮验证

已读取 `like.json` 的完整 TSX/CSS。导出 `Like` 的演示 API 是 `start`、`effect`、`burst`、`color`、`corner`，内部用 useState 切换 liked，原版没有服务端提交、受控状态或失败回滚。应用内使用时需显式增加受控 selected/onToggle/pending 入口，或把视觉层适配到现有受控Button；不能让内部计数与服务端状态各存一份。

0.8.0 临时React验收项目实际适配并构建了此源码：保留形变/计数效果，映射主题，加入受控状态与异步防重；状态只在本地验收API成功后变更，失败保持原值，刷新重新读取。最初原生 disabled 导致焦点丢失，修正为 aria-disabled/aria-busy 配合事件保护后，连点与键盘Enter回归通过。真实媒体偏好reduce时粒子层隐藏、数位无位移，仍可切换收藏。验证只覆盖此Like适配，不覆盖48项免费组件或生产认证服务。
