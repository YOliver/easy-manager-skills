---
name: summarize-knowledge
description: 当用户说 "/总结"、"知识总结"、"总结一下"，或要求把对话中的经验保存为可复用知识时使用。将当前对话中的洞见提取为结构化的知识文件。
---

# 知识总结

## 概述

从对话中提取可复用的知识点，保存为结构化文件，存放在本 skill 的 `knowledge/` 目录下。跨机器可移植——只需复制整个 skill 目录即可。

## 何时使用

- 用户说 `/总结`、`/总结一下`、"知识总结"
- 用户说 "把这个记下来"、"save this"
- 用户想沉淀当前对话中的洞见

## 知识存储

```
skills/summarize-knowledge/
├── SKILL.md                 # 本文件：流程与规则
├── templates.md             # 各类文件的模板（按需查阅）
├── references/
│   └── REMOTE.md            # git 远程仓库地址
└── knowledge/
    ├── INDEX.md             # 带链接的分类索引
    ├── claude-code-配置.md   # 每个主题一个文件
    ├── python-并发.md
    └── ...
```

文件可移植：把 `summarize-knowledge/` 复制到另一台机器的 `~/.claude/skills/` 即可使用。

## 流程

### 1. 确保是 Git 仓库并同步最新

首先，检查本 skill 目录是否为 git 仓库：

```bash
cd <本-skill-目录>
git rev-parse --is-inside-work-tree 2>/dev/null
```

- **若不是 git 仓库** → 自动初始化（远程地址从 [references/REMOTE.md](references/REMOTE.md) 读取，下方 `<远程地址>` 用该文件中的地址替换，如果此文件缺失，创建并提示用户尽早填写远程地址）：
  ```bash
  git init
  git remote add origin <远程地址>
  git fetch origin
  git checkout main --track origin/main
  ```
  如果远程为空（首位贡献者），则：
  ```bash
  git init && git checkout -b main
  git remote add origin <远程地址>
  ```

- **若已是 git 仓库** → 同步：
  ```bash
  git pull --rebase origin main
  ```
  - 若因网络原因拉取失败，提醒用户但继续本地工作
  - 若发生 rebase 冲突，中止 rebase 并通知用户

### 2. 回顾对话

识别出符合以下特征的洞见：
- **非显而易见** —— 你需要解释它，用户从中学到了东西
- **可复用** —— 适用范围超出本次会话
- **不在代码里** —— 无法通过阅读代码得出

跳过：临时的任务细节、进行中的工作、已有文档记录的内容。

### 3. 为每个知识点分类

| 分类 | 示例 |
|----------|----------|
| 工具配置 | Claude Code 设置、shell 配置、开发环境 |
| 编程概念 | 并发模型、设计模式、语言特性 |
| 调试技巧 | 错误排查、性能分析、日志分析 |
| 工作流程 | Git 工作流、CI/CD、部署流程 |
| 框架/库 | React 模式、Python 库用法、API 设计 |

### 4. 保存每个知识点

检查 `knowledge/` 中是否已存在该主题的文件：
- **主题已存在** → 追加到该文件，追加格式见文件模板章节。
- **新主题** → 用模板创建新文件，新文件模板见文件模板章节。

### 5. 更新 INDEX.md

在对应分类下新增或更新索引条目。格式见文件模板章节。

### 6. 提交并推送（Git 同步）

提交所有改动并推送到远程：
```bash
git add .
git commit -m "知识更新: <简要描述>"
git push origin main
```
- 若推送失败，提醒用户（commit 已保留在本地，可稍后推送）

## 文件模板

各类文件的具体模板（新知识文件、追加格式、INDEX.md 格式）见 [templates.md](templates.md)。写入知识文件前按需查阅。

## 作用范围（硬性约束）

- **本 skill 只能读写自身 skill 目录内的文件**（即 `summarize-knowledge/` 及其子目录 `knowledge/`、`references/` 等）。
- **严禁跳出 skill 目录**：不得读取、创建、修改或删除 skill 目录以外的任何文件。
- git 操作（pull/commit/push）也仅针对本 skill 目录这个仓库，不得触碰其他仓库或上级目录。
- 若某个操作需要访问 skill 目录以外的内容，应停止并告知用户，而非越界执行。
- 允许对本仓库做网络 git 操作（push/pull 到已配置的远程），只是不碰本地文件系统里skill目录以外的内容。

## 核心规则

- **一个主题一个文件**，而非一次对话一个文件
- **追加到已有文件** —— 不要覆盖。知识是累积的
- **每条记录标注日期**，让读者知道何时记录的
- **保持简洁** —— 这是一份参考资料，不是教科书
- **交叉引用** —— 知识文件都在 `knowledge/` 内，用 `[主题](主题名.md)` 互相链接
- **可移植路径** —— 在 knowledge 目录内使用相对链接
