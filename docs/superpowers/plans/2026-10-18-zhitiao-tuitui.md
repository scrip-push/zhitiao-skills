# 纸条推推 Implementation Plan

**Goal:** 在公开独立仓库提供可安装的纸条推推技能，复用纸条 CLI 完成只读求职数据查询。

**Architecture:** 技能主文件定义流程与边界，参考文档保存 CLI 参数和错误语义，UI 元数据提供发现与调用入口，README 描述安装。原 CLI 负责身份、权限和网络通信。

**Tech Stack:** Markdown、YAML、Node.js 24+、@hrbully/zt-cli@0.1.0。

## Global Constraints

- 从新仓库最新 origin/main 开发，不修改其他项目。
- 不新增环境变量、服务或凭证处理逻辑。
- 不保存生产查询内容、个人信息或 token 到仓库。

## 步骤

- [x] 编写 skills/zhitiao-tuitui/SKILL.md、references/cli.md、agents/openai.yaml 和根 README。
- [x] 使用 skill-creator 的 quick_validate.py 验证技能，核对参考链接及 CLI 帮助。
- [x] 运行已发布 CLI 的授权状态、筛选发现、三类列表与详情，只记录验证摘要。
- [ ] 审查后通过 PR 合入 main，验证远程文件和仓库公开状态。
