---
name: project-init
description: Use when starting a new project and need to create a standard project directory with Claude Code configuration, status tracking, and git initialization. Triggers on phrases like "new project", "init project", "create project folder".
---

# Project Init

Initialize a standard project directory with Claude Code configuration files, git repo, and status tracking.

## Workflow

1. **Ask** the user for a project description (if not already provided)
2. **Name** the project directory — English, lowercase-with-hyphens, reflects the project
3. **Create** the directory structure and all files (see below)
4. **Git init** + initial commit
5. **Report** what was created

## Target Directory

Default: current working directory (`~` if not specified). The user can override.

## Directory Structure

```
<project-name>/
├── DEMAND.md                          # requirements from user input
├── .gitignore                         # git ignore rules
├── .claude/
│   ├── CLAUDE.md                      # project entry: structure + paths + session protocol
│   ├── RULES.md                       # project-specific rules (not duplicating global)
│   ├── MEMORY.md                      # project memory index
│   ├── memory/                        # project memory files
│   └── STATUS.md                      # project status tracking
└── tmp/
    └── .gitkeep
```

## File Contents

### DEMAND.md

Write the user's project description here. Keep the user's original wording.

### .gitignore

```
tmp/
.venv/
__pycache__/
*.pyc
.DS_Store
```

### .claude/CLAUDE.md

```
## 目录结构

<!-- 
  目录变更时更新此处。
  只展开顶层，不深度展开。
-->

<tree with # Chinese comments, matching actual created structure>

## 关键路径

- 需求文档：`DEMAND.md`
- 项目规则：`.claude/RULES.md`
- 项目记忆：`.claude/MEMORY.md`
- 项目状态：`.claude/STATUS.md`
- 临时文件：`tmp/`

## 会话启动协议（必须执行）

1. 先读取 `STATUS.md` — 了解当前阶段、计划和代码状态
2. 完成任务后更新 `STATUS.md`；目录变更时同步 `CLAUDE.md`；有经验教训时更新 `MEMORY.md`
```

### .claude/RULES.md

```
# 项目规则

<!-- 
  仅填写项目专属规则。
  通用规则已在 ~/.claude/RULES.md 中维护，不在此重复。
-->
```

### .claude/MEMORY.md

```
# 项目记忆索引

<!-- 
  项目专属的经验教训和架构决策。
  不重复全局记忆中的内容。
  格式：- [标题](memory/文件名.md) — 一句话描述
-->
```

### .claude/STATUS.md

```
## 项目状态

更新于：<today's date>

## 当前阶段

项目已初始化。

## 近期进展

## 下一步

## 交付物

## 待办事项
```

### tmp/.gitkeep

Empty file.

## Post-Creation

After creating all files:

```bash
cd <project-name>
git init
git add -A
git commit -m "init: project scaffold"
```

## Important

- Project name must be English, use hyphens for spaces
- All document content (headings, section names, body text, HTML comments) must be written in Chinese; only code identifiers, file paths, and shell commands remain in their original form
- RULES.md starts empty — global rules already cover general behavior
- CLAUDE.md directory tree must use `# Chinese comments`
- STATUS.md date must be filled with actual date
- DEMAND.md must contain the user's actual project description, not a placeholder
