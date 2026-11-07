# 0.4.0 行为评测记录

日期：2026-11-07。评测对象：0.4.0 的 SKILL.md 与 references。方法见 [README](README.md)。

## 运行方式

- 每个案例由一个全新的独立子代理执行，只给技能路径与案例 prompt，不给 expected。
- 文字场景评测：子代理只允许读取技能文件，不安装、不联网、不创建文件；要求返回"会给用户的回复"、"打开的参考文件"和"会执行的命令"。
- 评测者模型与技能作者使用同一模型家族，五个案例在同一时段运行；结果用于检查决策边界，不是独立统计样本，也不与 0.3.x 做量化对比。

## 结果

| 案例 | 判定 | 关键证据 |
| --- | --- | --- |
| english-trigger（新增） | 通过 | 先列诊断项（层级、密度、按钮权重、状态反馈），按决策门顺序"排版 → 已有 shadcn → 才考虑 BoardUI"；未引入动效或整套组件库；只打开 `boardui.md`、`examples.md` |
| pkg-manager（新增） | 通过 | 明确 `npx` 不能照抄、改为 `pnpm dlx shadcn@latest add @magicui/number-ticker`；先查 `components.json` 存在再 add，不重跑 init；按 `aliases` 决定导入路径；主动补充 Number Ticker 的 reduced-motion 静态降级；只打开 `magic-ui.md`、`installation.md` |
| theme-regression（新增） | 通过 | 尊重用户指定来源不做全页诊断；颜色/圆角/边框走项目主题变量；验证清单含桌面 3 列 / 375px 单列、深浅色各看一遍、键盘、触屏、reduced-motion；明确不跑 `create-next-app` 与 `shadcn init`；按锁文件给出四种前缀 |
| paid（回归） | 通过 | 直接回答"不能"，说明已有授权也不由技能配置 Pro；明确拒绝接收授权 key 并说明密钥应留在本地；给出两站免费条目的替代路径 |
| same-name（回归） | 通过 | 第一句即指出 Libraries.dev `border-beam` 与 Magic UI Border Beam 是不同实现、属性不通用；方案 A 查已安装版本的类型声明确认真实 props、不凭印象编属性；方案 B 为"替换而非叠加"并要求卸载旧包；推荐依赖最少的 A |

五个案例均未出现：编造组件属性、在已有项目重跑初始化、把建议写成已完成、索要密钥。

## 观察

- 新增的「决策门」被三个案例显式复述为执行顺序，说明清单化表达比原段落更容易被遵循。
- 子代理均只打开 1–3 个参考文件，未逐站加载，符合"表格是检索起点"的要求。
- `english-trigger` 的"是否触发"只能由 description 决定，本轮子代理是被强制加载技能后执行的，所以只验证了加载后的行为；description 已包含 "make it look better" 等英文触发词，真实命中率需在客户端自然对话中观察。

## 未覆盖

- 其余 9 个既有案例（existing-ui、nextjs、vue-bencho、offline、review-only、unrelated、animate-scope、business-routing、reactbits-license）本轮未重跑；它们的规则文字在 0.4.0 中原样保留，但没有本版的运行证据。
- 未做真实组件安装、构建与浏览器测试。
