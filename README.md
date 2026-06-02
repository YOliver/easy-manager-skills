# Skills 共享仓库

本仓库收录了两个可复用的 Claude / CodeBuddy Skill，开箱即用，可直接拷贝到本地 skills 目录使用。

## 包含的 Skills

| Skill | 说明 |
|-------|------|
| [plan-manager](./skills/plan-manager) | 基于 Markdown 的工作计划管理 skill，支持计划创建、步骤跟踪、操作日志、归档管理，内置 Git 自动同步 |
| [summarize-knowledge](./skills/summarize-knowledge) | 从对话中提取可复用知识点，按主题分类保存为结构化 Markdown 文件，支持 Git 多设备同步 |

## 目录结构

```
easy-manager-skills/
├── README.md
└── skills/
    ├── plan-manager/
    │   ├── SKILL.md            # Skill 定义与使用说明
    │   ├── AGENT_PROMPT.md     # Agent 行为提示词
    │   └── plan-manager.md     # 计划管理核心逻辑
    └── summarize-knowledge/
        └── SKILL.md            # Skill 定义与使用说明
```

## 关于运行时数据

**本仓库仅包含 Skill 定义文本（SKILL.md / 行为指令 / Prompt），不包含任何运行过程中产生的数据。**

具体而言，以下目录/文件在共享前已被清空或未纳入仓库：

- `skills/plan-manager/todo/`、`skills/plan-manager/done/` —— 个人工作计划文件
- `skills/summarize-knowledge/knowledge/` —— 个人知识库内容
- 其他在 skill 运行过程中由用户数据生成的中间文件

这样做的原因：

1. **避免泄露个人/工作隐私信息** —— 计划、知识库、操作日志通常包含敏感内容
2. **Skill 本身才是可复用的资产** —— 数据应由每位使用者按自己的场景独立积累
3. **保持仓库轻量** —— 仅同步行为定义，避免历史数据膨胀

## 使用方式

1. 克隆或下载本仓库
2. 将需要的 skill 子目录拷贝到你本地的 skills 路径下，例如：
   - Claude Code：`~/.claude/skills/`
   - CodeBuddy Code：`~/.codebuddy/skills/`（或对应平台的 skill 目录）
3. 首次使用时，对应 skill 会在自己的目录下创建运行时所需的子目录（如 `todo/`、`knowledge/` 等）
4. 如需启用 Git 同步功能，请按各 skill 内文档自行配置 `git remote`，**不要复用本仓库地址**作为个人数据的远端

## 各 Skill 的更多说明

请参见各子目录下的 `SKILL.md`：

- [plan-manager/SKILL.md](./skills/plan-manager/SKILL.md)
- [summarize-knowledge/SKILL.md](./skills/summarize-knowledge/SKILL.md)

## 许可证

MIT License
