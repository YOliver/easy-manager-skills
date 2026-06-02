---
name: plan-manager
version: 1.0.0
description: 基于Markdown的工作计划管理skill，支持计划创建、步骤跟踪、操作日志、归档管理，内置Git自动同步功能
author: Oliver
tags:
  - task-management
  - productivity
  - planning
  - git-sync
platform: Knot Claw
language: python
requirements:
  - python >= 3.6
---

# plan-manager Skill

基于 Markdown 的工作计划管理工具，计划文件存储在 skill 目录内部，支持 Git 自动同步到远端，方便在其他平台实时查看工作计划状况。

## 功能特性

- **计划管理**: 创建、查看、管理工作计划（.md 格式）
- **唯一编号**: 自动生成 `PLAN-YYYYMMDD-XXX` 格式的唯一计划编号
- **步骤跟踪**: 为每个计划添加执行步骤，支持完成状态跟踪
- **操作日志**: 自动记录所有操作历史（带时间戳）
- **状态管理**: 自动状态流转（待开始 → 进行中 → 已完成）
- **归档管理**: 支持将已完成计划移动到 done 目录进行归档
- **Git 同步**: 每次操作后自动 commit + push，远端实时同步

## 使用方法

### 创建新计划
```
创建一个计划：项目开发计划，目标完成日期 2026-06-01
```

### 添加执行步骤
```
给'项目开发计划'添加步骤：需求分析
```

### 标记步骤完成
```
标记'项目开发计划'的步骤1为已完成
```

### 添加操作日志
```
给'项目开发计划'添加日志：需求分析会议已完成
```

### 查看计划列表
```
列出所有进行中的计划
当前未完成工作计划
```

### 查看计划详情
```
查看'项目开发计划'的详细内容
PLAN-20260518-001
```

### 归档已完成计划
```
归档'项目开发计划'
```

## API 参考

### create_plan(title, target_date="")
创建新的工作计划。

**参数:**
- `title`: 计划标题（用作文件名）
- `target_date`: 目标完成日期（可选）

**返回:** 操作结果，包含 plan_id

### add_step(plan_title, step_description, position=None)
为指定计划添加执行步骤。

**参数:**
- `plan_title`: 计划标题
- `step_description`: 步骤描述
- `position`: 插入位置（可选，默认追加末尾）

### complete_step(plan_title, step_number)
标记指定计划的步骤为已完成。

**参数:**
- `plan_title`: 计划标题
- `step_number`: 步骤编号

### add_log(plan_title, log_message)
为指定计划添加操作日志。

**参数:**
- `plan_title`: 计划标题
- `log_message`: 日志消息

### list_plans()
列出所有未完成工作计划。

**返回:** 计划列表（含编号、标题、预计完成时间、工作内容概述）

### show_plan_content(plan_title)
展示指定工作计划的完整文档内容。

**参数:**
- `plan_title`: 计划标题

### move_to_done(plan_title)
将已完成的工作计划归档到 done 目录。

**参数:**
- `plan_title`: 计划标题

## 文件格式

每个计划存储为独立的 Markdown 文件，包含：
- 标题和计划信息（编号、创建日期、状态、预计完成时间）
- 计划目标
- 执行步骤（支持复选框）
- 操作日志（带时间戳）

## 使用约束

### 文件命名规则
- **必须**：文件名使用计划标题，如 `项目开发计划.md`
- **禁止**：不允许使用 plan_id 作为文件名
- plan_id 仅写入文件内部元数据

## 存储结构
```
plan-manager/
├── SKILL.md          ← 技能描述文件
├── plan-manager.md   ← 行为指令文件
├── todo/             ← 进行中的计划
│   ├── 计划A.md
│   └── 计划B.md
└── done/             ← 已完成归档
    └── 已完成计划.md
```

## 依赖项

- Python 3.6+（如作为 Python skill 使用）
- Git（用于自动同步）
- 标准库：os, datetime, re

## 许可证

MIT License
