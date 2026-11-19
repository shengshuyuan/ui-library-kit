# Libraries.dev — component-method 组件方法与接入

核实日期：2026-11-19。证据来自官网页面和可读的 registry/repository 入口；没有安装包或运行 demo，因此不把源码读取当作构建验收。

## 来源与许可

- [目录](https://libraries.dev/)列出 Border Beam、Thinking Orbs、Gooey、Voice、Voice Ring、Distortion Waves、Bot Avatars、Metal、Image，以及 Magnetic UI / Voice Position 模型。目录 FAQ 称五个首发库 MIT、npm 免费；页面目录已有更多项目，不能把“五个”外推到全部当前条目。
- [Agent Skill](https://libraries.dev/skill)给出 `review`（只读定位）、`apply`（挑库、接包、用真实状态连线）、`tune`（Pro）三个工作层次。免费版会覆盖七个库的公开选项，Pro 增加 Studio 选项；不需要安装 Pro Skill 才能使用公开包。
- [GitHub 源码仓库](https://github.com/Jakubantalik/Libraries.dev)含顶层 MIT `LICENSE` 和独立 `LICENSE-MODEL.md`；模型许可必须与 UI 包区分。逐包仍以对应当前发布包元数据和文件声明为准。
- 官网 FAQ 说明 React 18+、大多数库零运行依赖、Image 需 `three`；Beam / Orbs 另有 SwiftUI、React Native 页面，其他平台支持不应假定。

## 代表组件：问题 → 组合方法 → 接线与验收

| 页面问题与选择 | 组合方法/设计原则 | 入口与风险 |
|---|---|---|
| AI 回复等待没有可读进度：用 **Thinking Orbs**，用 `state` 区分 searching / working 等阶段。 | 让动画状态承载真实任务语义，而非永远转圈；可用 20px 行内或 64px 头像尺寸，`paused` 停止动画。 | [文档/API](https://libraries.dev/orbs)，包 `thinking-orbs`；`state` 应绑定真实 agent 状态。检查动效偏好、静止状态说明和暂停行为。 |
| 正在生成的输入框缺少“任务进行中”反馈：用 **Border Beam** 包住现有输入区域。 | 动效沿边缘表达当前处理状态；将 `active` 绑定请求生命周期，保留原输入、焦点和提交语义。 | [文档/API](https://libraries.dev/beam)，包 `border-beam`；size、colorVariant、strength 有公开参数。React 18+；检查 `active=false` 后确实停播及 reduced-motion 处理。 |
| 语音输入/回答中状态不明显：用 **Voice Glow**，输入流做视觉强度源，处理阶段切换 sweep。 | 麦克风轨迹（`MediaStream`）与 `processing` 是两种状态输入；若无录音权限，也可把本地 0–1 音量表接到 `level`。 | [文档/API](https://libraries.dev/voice)，包 `voice-glow`；真接 mic 要求用户授权和准确的状态/停止控件。SwiftUI 端还需麦克风用途描述。 |
| 语音对话头像缺乏“在听/说/处理”状态：用 **Voice Ring** 表示环形状态，`state` + `stream` 或 `level` 绑定播放器。 | 组件展示已有音频信号，不负责理解音频；emotion / direction 是显式业务数据，不能伪装为组件推断。 | [文档/API](https://libraries.dev/voice-ring)，包 `voice-ring`。网页端轻量 WebGL 边缘镜片；检查无 WebGL 的替代、麦克风权限与颜色可读性。 |
| 升级或主 CTA 想强调而不改按钮语义：用 **MetalFx** 包住现有原生按钮。 | 只装饰现有控件，键盘和读屏仍使用 button；WebGL2 缺失时页面说明有 plain-child fallback。 | [文档/API](https://libraries.dev/metal)，包 `metal-fx`；`preset/variant/strength/theme/paused`。检查低端设备性能与可见焦点环，动效不可承担唯一状态指示。 |
| 图片生成从占位到结果的等待态：**ImageGeneration** 包图片卡片，用公开图片 URL 驱动 reveal。 | 占位块尺寸先固定，图片完成后交给组件 reveal，避免布局跳动；数据流接生成任务结果。 | [文档/API](https://libraries.dev/image)，包 `img-fx` + peer dependency `three`。WebGL 可用性、内存和失败图兜底需要实测。 |

可迁移的方法不是“给界面加特效”，而是先找页面状态落差，再选最小包覆点，把组件参数接到已有业务状态，并保留原有交互语义。Gooey 适合一组会合并的浮动按钮/工具项，不应用于正文文字：见 [Gooey API](https://libraries.dev/gooey)（`Liquid` 包住多个 `Liquid.Item`，React 18+）。

## 访问缺口/验收边界

- 已读官网详情与安装片段；本轮未调用 `npx skills`、未安装依赖、未跑浏览器/端到端演示。
- 页面提到 reduced motion / accessibility 文档入口，但未逐段核实实现；把暂停、降动效、键盘焦点、WebGL fallback 列为使用前验收，不宣称库默认都满足。
- 用户项目需先读 package manifest、组件状态生命周期、SSR/client 边界；不能根据官网片段直接宣告已接线。
