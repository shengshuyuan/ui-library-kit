# Libraries.dev
核实日期：2026-11-05。用于 AI 状态、边框光效、语音及流体效果。

官方入口：https://libraries.dev/
- 官方 skill：https://libraries.dev/skill
- 官方 skill 安装入口：`npx skills add Jakubantalik/Libraries.dev`
- Border beam 文档：https://libraries.dev/beam
- 源码：https://github.com/Jakubantalik/Libraries.dev

按具体效果页确认 API、平台与包许可，官网目录及各页可能更新不同步，不硬编码总数。免费包与 Pro Studio/skill 功能分开，采用普通文档也能接入免费效果。

已核实的 React 最小示例（使用前仍检查当前包版本）：
```jsx
import { BorderBeam } from 'border-beam';
<BorderBeam active={isGenerating}><YourCard /></BorderBeam>
```
包：`border-beam`。官方页面列出 React 18+；需分别核实其他效果的额外依赖。active 应连接真实业务状态；核查 reduced-motion 行为，必要时在项目中降低或关闭动画。原生端不能直接复用 Web 包，需查对应官方平台版本与可用性。
