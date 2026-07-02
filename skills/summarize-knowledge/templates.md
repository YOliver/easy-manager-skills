# 文件模板

本文件收录知识库各类文件的模板，供 `summarize-knowledge` skill 在写入知识时按需查阅。

## 新知识文件（`knowledge/主题名.md`）

```markdown
# 主题标题

> 首次记录: YYYY-MM-DD

## YYYY-MM-DD: 洞见标题

内容写在这里。保持简洁，通常一段话足矣。

**关键点:**
- 用要点列出关键收获
- 相关的命令或配置片段
```

## 追加到已有文件

追加时，在文件底部新增一个 `##` 小节：

```markdown
## YYYY-MM-DD: 新洞见标题

新内容写在这里。
```

## INDEX.md 格式

```markdown
# 知识索引

## 工具配置
- [Claude Code 配置与 Skills](claude-code-配置.md) — settings.json 层级、Skills 注册机制、CLAUDE.md 作用

## 编程概念
- [Python 并发](python-并发.md) — asyncio、线程池、GIL

## 调试技巧
...
```
