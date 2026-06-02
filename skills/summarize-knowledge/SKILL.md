---
name: summarize-knowledge
description: Use when the user says "/总结", "知识总结", "总结一下", or asks to save conversation learnings as reusable knowledge. Extracts insights from the current conversation into structured knowledge files.
---

# Knowledge Summary

## Overview

Extract reusable knowledge points from conversations and save them as structured files under this skill's `knowledge/` directory. Portable across machines — just copy the skill directory.

## When to Use

- User says `/总结`, `/总结一下`, "知识总结"
- User says "把这个记下来", "save this"
- User wants to capture insights from the current conversation

**Don't use for:**
- Code patterns, project structure (derivable from reading code)
- Git history, recent changes (derivable from `git log`)
- One-off solutions with no reuse value
- Things already documented in CLAUDE.md

## Knowledge Storage

```
skills/summarize-knowledge/
├── SKILL.md
└── knowledge/
    ├── INDEX.md              # Categorized index with links
    ├── claude-code-配置.md    # One file per topic
    ├── python-并发.md
    └── ...
```

Files are portable: copy `summarize-knowledge/` to another machine's `~/.claude/skills/` and it works.

## Git Sync (Multi-device Collaboration)

This skill directory is a git repository, enabling multi-device, multi-model knowledge sharing.

**Remote repository:** `https://git.woa.com/oliveryin/summarize-knowledge.git`

### Git Workflow

Every time this skill is invoked, follow this sync protocol:

1. **Pre-write sync** — Before making any changes:
   ```bash
   cd <this-skill-directory>
   git pull --rebase origin main
   ```
   - If pull fails due to network, warn user but continue (work locally)
   - If rebase conflicts occur, abort rebase and notify user

2. **Post-write commit & push** — After all knowledge files are written:
   ```bash
   cd <this-skill-directory>
   git add .
   git commit -m "知识更新: <topic-summary>"
   git push origin main
   ```
   - Commit message should briefly describe what knowledge was added/updated
   - If push fails, warn user (commit is preserved locally for later push)

### First-time Setup

If this directory is not yet a git repo, initialize it:
```bash
cd <this-skill-directory>
git init
git add .
git commit -m "初始化知识库"
# git remote add origin <REMOTE_URL>  ← user provides this
# git push -u origin main
```

## Process

### 1. Ensure Git Repo & Sync Latest

First, check if this skill's directory is a git repository:

```bash
cd <this-skill-directory>
git rev-parse --is-inside-work-tree 2>/dev/null
```

- **If NOT a git repo** → Auto-initialize:
  ```bash
  git init
  git remote add origin https://git.woa.com/oliveryin/summarize-knowledge.git
  git fetch origin
  git checkout main --track origin/main  # adopt remote history
  ```
  If remote is empty (first contributor), just:
  ```bash
  git init && git checkout -b main
  git remote add origin https://git.woa.com/oliveryin/summarize-knowledge.git
  ```

- **If already a git repo** → Sync:
  ```bash
  git pull --rebase origin main
  ```
  - If pull fails due to network, warn user but continue locally
  - If rebase conflicts, abort rebase and notify user

### 2. Review the Conversation

Identify insights that are:
- **Non-obvious** — you had to explain it, the user learned something
- **Reusable** — applies beyond this specific session
- **Not in code** — not derivable from reading the codebase

Skip: ephemeral task details, in-progress work, things already documented.

### 3. Categorize Each Knowledge Point

| Category | Examples |
|----------|----------|
| 工具配置 | Claude Code settings, shell配置, 开发环境 |
| 编程概念 | 并发模型, 设计模式, 语言特性 |
| 调试技巧 | 错误排查, 性能分析, 日志分析 |
| 工作流程 | Git工作流, CI/CD, 部署流程 |
| 框架/库 | React模式, Python库用法, API设计 |

### 4. Save Each Knowledge Point

Check if a file already exists for this topic in `knowledge/`:
- **Topic exists** → Append to the file, add `##` subsection with new insight
- **New topic** → Create new file with template below

### 5. Update INDEX.md

Add or update the entry in the index under the appropriate category.

### 6. Commit & Push (Git Sync)

Commit all changes and push to remote:
```bash
git add .
git commit -m "知识更新: <brief-description>"
git push origin main
```

## File Templates

### New knowledge file (`knowledge/topic-name.md`)

```markdown
# Topic Title

> 首次记录: YYYY-MM-DD

## YYYY-MM-DD: Insight Title

Content here. Be concise. One paragraph is usually enough.

**关键点:**
- bullet points for key takeaways
- commands or config snippets if relevant
```

### Appending to existing file

When appending, add a new `##` section at the bottom:

```markdown
## YYYY-MM-DD: New Insight Title

New content here.
```

### INDEX.md format

```markdown
# Knowledge Index

## 工具配置
- [Claude Code 配置与 Skills](knowledge/claude-code-配置.md) — settings.json层级、Skills注册机制、CLAUDE.md作用

## 编程概念
- [Python 并发](knowledge/python-并发.md) — asyncio、线程池、GIL

## 调试技巧
...
```

## Key Rules

- **One topic per file**, not one file per conversation
- **Append to existing** — don't overwrite. Knowledge accumulates
- **Date each entry** so readers know when it was captured
- **Be concise** — this is a reference, not a textbook
- **Cross-reference** — link related knowledge files with `[topic](../topic-name.md)`
- **Portable paths** — use relative links within the knowledge directory
