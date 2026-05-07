---
name: project-init
description: Use when starting a new project and need to create a standard project directory with Claude Code configuration, status tracking, and git initialization. Triggers on phrases like "new project", "init project", "create project folder".
---

# Project Init

Initialize a standard project directory with Claude Code 4-piece config (CLAUDE.md + claude/{RULES, STATUS, MEMORY, memory/}), git repo, and status tracking. Aligns with the Ob_vault entity-model 4-piece convention (CLAUDE.md visible at project root + `claude/` directory NOT `.claude/`).

## Workflow

1. **Ask** the user for a project description (if not already provided)
2. **Name** the project directory — English, lowercase-with-hyphens, reflects the project
3. **Create** the directory structure and all files (see below)
4. **Git init** + initial commit
5. **Report** what was created

## Target Directory

Default: current working directory. The user can override.

## Directory Structure

```
<project-name>/
├── DEMAND.md                          # requirements from user input
├── <project-name>.md                  # entity entry note (frontmatter: entity: project)
├── CLAUDE.md                          # project startup protocol (visible, at project root)
├── .gitignore                         # git ignore rules
├── claude/                            # 4-piece config (visible directory, not .claude/)
│   ├── RULES.md                       # project-specific rules (not duplicating global)
│   ├── STATUS.md                      # project status tracking
│   ├── MEMORY.md                      # project memory index
│   └── memory/                        # project memory entries (one .md per entry)
└── tmp/
    └── .gitkeep
```

## File Contents

### DEMAND.md

Write the user's project description here. Keep the user's original wording.

### `<project-name>.md` (entity entry note)

```markdown
---
entity: project
created: <today>
status: active
---

# <project-name>

<one-line description from DEMAND.md>

## 入口

- [[CLAUDE]] — startup protocol
- [[DEMAND]] — full requirements

## 关键路径

- 状态：`claude/STATUS.md`
- 经验：`claude/MEMORY.md`
- 规则：`claude/RULES.md`
```

### CLAUDE.md (project root, visible)

```markdown
# <project-name> · 启动协议

## 身份

- entity: **project**
- 范畴：<one-line scope from DEMAND.md>

## 启动加载顺序

1. 读本文件
2. 读 `claude/RULES.md` —— 本 project 硬规则
3. 读 `claude/STATUS.md` —— 当前状态
4. 读 `claude/MEMORY.md` 索引（按需深读 `claude/memory/*.md`）
5. 读 `DEMAND.md` —— 需求描述

## 关键路径

- 入口 note：`<project-name>.md`
- 需求文档：`DEMAND.md`
- 状态：`claude/STATUS.md`
- 经验：`claude/MEMORY.md` + `claude/memory/*.md`
- 规则：`claude/RULES.md`
- 临时文件：`tmp/`

## 完成任务后

- 用户口令"更新 status" → 写 `claude/STATUS.md`
- 目录变更 → 同步 `CLAUDE.md`
- 经验沉淀 → 起草 `claude/memory/<name>.md` 草稿，等用户审过再 promote
```

### claude/RULES.md

```markdown
# <project-name> · 规则

<!--
  仅填写本 project 专属硬规则。
  全局规则在 `~/.claude/RULES.md` / `~/.claude/CLAUDE.md`，不重复。
  条目化、祈使式、一行一条。建议条数 ≤20。
-->
```

### claude/STATUS.md

```markdown
---
updated: <today>
phase: 项目已初始化
---

## 当前正在做

- <初始任务>

## 下一步

- <next step>

## 已完成

- <today>: 项目初始化
```

### claude/MEMORY.md

```markdown
# <project-name> · 记忆索引

<!--
  项目专属经验 / 教训 / 模式。
  格式：- [标题](memory/文件名.md) — 一句话钩子
-->
```

### .gitignore

```
tmp/
.venv/
__pycache__/
*.pyc
.DS_Store
```

### tmp/.gitkeep

Empty file.

## Post-Creation

After creating all files:

```bash
cd <project-name>
git init
git add -A
git commit -m "init: project scaffold (4-piece convention)"
```

## Important

- Project name: English, lowercase-with-hyphens
- All 4-piece content (CLAUDE.md / RULES.md / STATUS.md / MEMORY.md) written in Chinese; only code identifiers, file paths, shell commands stay original
- 4-piece directory is **`claude/`** (visible), NOT `.claude/` — aligns with Ob_vault entity-model convention
- CLAUDE.md goes at the project root, not inside `claude/`
- RULES.md starts empty (or near-empty) — global rules already cover general behavior
- STATUS.md frontmatter `updated` and `phase` filled with actual values
- DEMAND.md must contain the user's actual project description, not a placeholder
- If the project will be a child of an Ob_vault entity (vault / management), set `parent` or `reports_to` in `<project-name>.md` frontmatter
