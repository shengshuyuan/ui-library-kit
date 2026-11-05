# 安装与调用

## 手动安装（无 Node.js 要求）

把完整 `ui-library-kit` 文件夹复制到客户端技能目录，保留 `SKILL.md`、`references` 和 `agents`。不要只复制主文件。

- Codex：`~/.codex/skills/ui-library-kit/`；客户端支持共享目录时也可放 `~/.agents/skills/`。新建聊天后用 `$ui-library-kit`。
- Claude Code：`~/.claude/skills/ui-library-kit/`。新会话中用 `/ui-library-kit`，或自然语言指定。
- 项目级共享放在对应客户端的项目 skills 目录，遵循项目规则。

本机已验证上述两个客户端的发现/调用，不代表其他客户端或所有操作系统均已验证。Windows 使用客户端实际支持的用户目录，不照抄 macOS 绝对路径。

## GitHub / Skills CLI

从公开仓库安装：

```sh
npx skills add shengshuyuan/ui-library-kit --skill ui-library-kit
```

安装器可选择客户端，默认项目级安装；需要全局时按当前 CLI 帮助加 `-g`。CLI 的 Node.js 版本要求以其当前文档为准。官方说明：https://github.com/vercel-labs/skills

## 更新、移除与故障

- 手动安装：先备份已有目录，再替换整个技能文件夹；不要覆盖同名自定义技能。
- CLI 安装：使用 `npx skills list` 查看已安装项，`npx skills update ui-library-kit` 更新；按当前 CLI 帮助移除；手动复制项不保证由 CLI 管理。
- 找不到技能：确认名称/目录、完整文件、正确客户端目录，再新开会话；能发现不代表实际项目已接入组件。
- 同机可以用软链接共享一份源目录，源目录移动或删除会让链接失效。公开包必须包含真实文件，不能依赖作者电脑上的链接。
- 不自动安装各来源的 skill/MCP，不需要额外服务器或密钥。检索最新文档需要网络；构建与浏览器检查需要项目自身工具。

调用示例见主文件及 README。本技能只接入免费且许可明确的组件，不配置第三方付费授权；模型和项目运行费用另计。
