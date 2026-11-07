# 0.4.0 结构重构验收

日期：2026-11-07。发布至 https://github.com/shengshuyuan/ui-library-kit 。

## 变更

本版不改变来源范围（仍为 10 个免费来源）和行为规则，只改变 SKILL.md 的组织方式：

- 长段落拆成五步工作流（诊断 → 决策门 → 选型 → 接入 → 验证），每步用短句或可勾选清单表达。
- 选型路由表新增框架 / 前置条件 / 许可提示三列，把原本散落在各参考文件里的排除条件前置到主文件。
- 新增「决策门」「常见坑」「交付格式」三节，把原来隐含在段落里的决策顺序、已知误区和输出模板显式化。
- 接入规则补充包管理器前缀换算与"CLI 是否修改现有源码"检查；验证清单补充深浅色主题。
- frontmatter description 加入中英文触发词，便于英文请求命中。
- 安装说明与 README 补充 Cursor 目录。

## 验证范围

- frontmatter：name / description / license / metadata.version 解析正常，version 与 README、CHANGELOG、cases.json 一致为 0.4.0。
- 结构断言：SKILL.md 行数在 500 行以内；路由表 10 行；`references/` 内相对链接全部存在；无本机绝对路径。
- cases.json：JSON 合法，14 个案例 ID 唯一；新增 english-trigger、pkg-manager、theme-regression。
- 来源入口：10 个来源的主要入口 URL 于 2026-11-07 请求均返回 HTTP 200；仅确认可达，参考文件内容未逐页重读，其核实日期保持 2026-11-05。
- 本机三份副本（`~/.claude/skills`、`~/.agents/skills`、GitHub main）逐文件一致。
- 远程 Skills CLI 在临时项目安装成功，安装文件与发布提交一致。

- 行为评测：新增三个案例（english-trigger、pkg-manager、theme-regression）与两个回归案例（paid、same-name）由独立子代理运行，五项全部通过，详见 [results-0.4.0.md](results-0.4.0.md)。

## 未验证

- 其余 9 个既有案例未在 0.4.0 上重跑；评测者与作者同模型家族，不作量化结论。
- 未重新接入组件或做浏览器测试；真实接入证据仍见 [0.3.1 报告](release-0.3.1.md)，不因本版扩大。
- Windows / Linux 与 Codex、Claude Code、Cursor 以外的客户端未验证。
