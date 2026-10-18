# 纸条推推 Skills

配合 [纸条 CLI](https://github.com/scrip-push/zhitiao_cli) 使用的 AI 技能。用自然语言检索招聘公告、查看自己的面经与投递进展。主站：https://offertt.cn 。

## 安装 CLI

需要 Node.js 24+：

```sh
npm install -g @hrbully/zt-cli@0.1.0
zt --version
zt auth login
```

在官网完成微信登录并确认设备授权。已有有效授权可直接使用，无需重新登录。CLI 的授权由本地密钥环管理；技能不需要额外环境变量或服务。

## 安装技能

将仓库中的 `skills/zhitiao-tuitui` 文件夹放入所用 AI 工具的技能目录。Codex 用户可让内置 `skill-installer` 安装，发送：

> 使用 skill-installer，从 GitHub 仓库 scrip-push/zhitiao-skills 的 skills/zhitiao-tuitui 路径安装技能。

也可手动克隆本仓库，再复制 `skills/zhitiao-tuitui` 到 `~/.codex/skills/zhitiao-tuitui`（配置了 CODEX_HOME 时放入对应的 skills 目录）。已有同名技能时先检查差异，避免覆盖。安装后下一轮对话即可使用新技能。其他支持 SKILL.md 的工具按其技能目录规则安装；首次验证使用 Codex。

## 使用

安装后直接描述招聘信息或个人求职记录的查询需求，AI 可按需求自动选择技能并调用 CLI。用户无需说“用纸条推推”，也无需输入技能名或 CLI 命令。例如：

- “帮我找央国企的软件开发招聘信息，按截止时间整理。”
- “查一下杭州的 Java 岗位招聘。”
- “有哪些快截止的校招公告？”
- “看看我有哪些业务面试的面经，整理需要复习的知识点。”
- “汇总我最近更新的投递记录，按阶段整理进展。”

技能入口：[SKILL.md](skills/zhitiao-tuitui/SKILL.md)；参数与错误处理：[CLI 参考](skills/zhitiao-tuitui/references/cli.md)。

首版配套 CLI 0.1.0，支持招聘公告、本人面经和本人投递的列表与详情。招聘按岗位等信息筛选，返回公告。CLI 只读；主站会员权限、分页上限和授权规则仍适用。
